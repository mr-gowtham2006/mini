# Verdandi factory twin — current-state brief (factual, 2026-09-16)

Sources read verbatim: `src/twin.py` (1834 lines), `src/config.py` (179 lines, Table 3.1),
`docs/SIM_SPEC.md` (456 lines). `ELENCHUS_DISCOVERY.md` not needed — config rationale is
present inline (config.py header + SIM_SPEC §§2–4). Upgrade-spec C1–C6 from
`follow-ups/01-sim-depth-physics-ml/04-synthesis/upgrade-spec.md` (2026-09-13).

Global scalars: `T=300`, `CAL_WIN=120` (faults only in `[120,300)`), `N_MACHINES=32`,
`N_BUFFERS=31` (roster) + 1 hidden `_C7TAIL` store = 32 live Stores, `N_STREAMS=36`.

---

## 1) Machine inventory (32) — roles: redundant vs unique vs gateway/bottleneck

| Group | Machines | Class / params (base, σ, cycle, MTTF, MTTR) | Role |
|---|---|---|---|
| Line A (10) | A0 | feed 50.0/1.0/c4/2000/15 | REDUNDANT clone of B0/C0 (independent feed head, no shared stock) |
| | A1 | form 60.0/1.2/c5/1500/12 | REDUNDANT clone of B1/C1 |
| | A2–A7 (×6) | process 70.0/1.5/c6/800/20 each | REDUNDANT serial clones (all six identical; same for B2–B7, C2–C6) |
| | A8 | finish 55.0/1.1/c4/1200/10 | REDUNDANT clone of B8 (C has no finish; C7 covers tail) |
| | A9 | inspect-tail 48.0/1.4/c3/1500/8, tail cap 15 | GATEWAY (tail → GA9 → AGV → ASM0 kit). Never SBUF-direct |
| Line B (10) | B0–B9 | identical mirror of A0–A9 (same params per class) | REDUNDANT line; B9 gateway via GB9 |
| Line C (8) | C0/C1 | feed/form, same params as A0/A1 | REDUNDANT |
| | C2–C6 (×5) | process, same params as A2 | REDUNDANT |
| | C7 | inspect-tail 48.0/1.4/c3/1500/8 ("finish" label is line shorthand only; params are inspect) | GATEWAY via dedicated `_C7TAIL` store (cap 15, AGV-drained; NOT the C67 gap) |
| Assembly cell (3) | ASM0 | assembly-kit 65.0/1.3/c5/1000/12 | UNIQUE + BOTTLENECK: kitting gate — STARVED unless kit A,B,C each ≥1 |
| | ASM1 | assembly-join 66.0/1.3/c6/1000/12 | UNIQUE join (ASM01→ASM1→ASM12) |
| | ASM2 | test 45.0/σ2.0/c3/1200/10, sink + rework tap (no buffer_cap) | UNIQUE sink; documented known-noisy tail (VETO_ASM2 compensates; do not quiet σ) |
| Rework (1) | RWK0 | rework 62.0/1.6/c8/900/18, return cap 10 | UNIQUE rework station; no natural-breakdown draws (bit-identical clean path to T6) |

Canonical index: A0–A9=0–9, B0–B9=10–19, C0–C7=20–27, ASM0/1/2=28/29/30, RWK0=31.
Draw order per step is MACHINE_INDEX order.
Redundant = byte-identical operating point to ≥1 sibling. Unique = single instance of its
function. Gateway/bottleneck = tails (A9/B9/C7), ASM0 kit, ASM2 sink/tap, AGV pool, SBUF.

---

## 2) Buffer inventory + caps + routing rules

### 2a) Buffer roster (31 named + 1 hidden)
Gap cap mirrors upstream machine's `buffer_cap`. Exact caps (`src/config.py: _BUFFER_ROWS`):

- A-gaps (9): A01=20, A12=20, A23=25, A34=25, A45=25, A56=25, A67=25, A78=25, A89=15.
- B-gaps (9): identical (B01=20, B12=20, B23–B78=25, B89=15).
- C-gaps (7): C01=20, C12=20, C23=25, C34=25, C45=25, C56=25, C67=15.
- Cell (4): ASM01=25, ASM12=25, GA9=15, GB9=15.
- Rework return (1): RWK_RET=10.
- Shared overflow (1): SBUF=30 (`SBUF_CAP`).
- Hidden (not in 31-count): `_C7TAIL` cap 15 (= C7 `buffer_cap`; AGV-drained; sharing C67 would deadlock C6 vs C7 — twin.py:179-184).

### 2b) Rework entry/exit (T7, `twin.py: _asm_mid_process` + `_rwk0_process`)
- Entry (ASM2 sink, per completion at step t): `rej = (part.flag==REJECT) or (quality_rate>0 and rng_place()<rate)`.
  Reject rate 15–40% inside quality windows only; outside windows every part accepted
  (old id-modulo scrap placeholder removed).
  - If `rej` and `passes >= REWORK_MAX_PASSES(=2)` → scrap sink (`sunk++, scrapped++`, REJECT_ROUTE→scrap, parts disposition=scrap).
  - Elif `RWK_RET` has space → `passes+1`, `rwk.put`, REJECT_ROUTE→RWK0, `rejected++`, tput=1.
  - Else (RWK_RET full) → ASM2 holds BLOCKED with flag/passes intact (no re-roll).
- RWK0 (cycle 8, STARVED on empty intake):
  - Intake at `passes>=2` → scrap directly (livelock guard, no third pass).
  - Else run cycle; at completion while quality window active on ASM2 → requeue into RWK_RET with `passes+1` (rework surge; intake cap guarantees termination via scrap).
  - Else `passes+1`, `part.line="C"`, `kit["C"].append(part)`, `reworked++`, disposition=reworked.
  - Reworked kits re-enter via kit-C intake: kitting constraint still bites (A and B also required); part object (flags, pass count) keeps flowing, never cloned.

### 2c) AGV dispatch (`_agv_dispatcher` + `_agv_xfer`)
- Pool: `simpy.Resource(capacity=AGV_CAP=2)`. Trip hold `U{AGV_STEPS=(4,8)}` drawn on `rng_agv` at request time.
- Bounded queue: gate `_gate_open() = count + len(queue) < AGV_CAP + 2` (= 4: 2 in service + 2 queued). Beyond that dispatcher holds off spawning → tail buffers fill → BLOCKED backpressure (plus SBUF divert) propagates instead of hiding WIP in an unbounded queue.
- Priority per tick: SBUF first, then tails (GA9/A9, GB9/B9, _C7TAIL/C7) in `_TAILS` order.
- On delivery (`t_del<T`): `kit[part.line].append(part)`, `xfer_open--`, `agv_waits[] += {t,part,hold,wait}`, `parts[] += {delivered|diverted, passes, flag}`, `sbuf.drained++` if src==SBUF, AGV_WAIT event. Mid-transfer episode-end: part stays in `xfer_open` (conserved, never double-logged).

### 2d) Assembly kit funnel (`_asm0_process`, cycle 5)
- `kit = {A:[], B:[], C:[]}` plain lists (not Stores). Per step not-held: `missing=[ln for ln in (A,B,C) if not kit[ln]]`; any missing → STARVED (+ `kit_missing` detail on edge). Else pop 1 per line, run cycle (+delay d), then if ASM01 has space emit new kit part `{id new, line ASM, kit:[3 ids], passes=max, flag=max-severity(REJECT>DEGRADE>OK)}`, `asm_created++`, tput=1; else BLOCKED.

### 2e) SBUF shed/divert (SIM_SPEC §2.2; owner ruling 2026-09-12)
- Eligible: `cfg["class"] in SBUF_DIVERT_CLASSES={process,finish,inspect-tail}` AND `name not in _TAILS(A9,B9,C7)`. Feed/form never divert (hold through repair); tails ride AGV only.
- Two triggers: (i) on DOWN entry (injected or natural) with held WIP and SBUF space → shed (`_shed_to_sbuf`, DIVERT_SBUF event, `diverted++`); (ii) on BLOCKED finish (downstream full) with space → divert held part to SBUF, tput=1. Full SBUF → hold, stay BLOCKED/DOWN (no loss).
- Tails never divert SBUF-direct (callers guard); C7 stages in `_C7TAIL`.
- SBUF high-util flag: `max_occupancy`, `high_util = max>=0.8*30=24`, `high_util_steps[]` logged.

---

## 3) Every cross-variable coupling currently modeled (equation + parameter)

| # | Coupling | Equation / rule (all O(1) per machine-step) | Params / stream |
|---|---|---|---|
| K1 | State offsets into vibration obs | `obs = clean + STATE_OFFSETS[state]*σ + faultdev`, `STATE_OFFSETS={RUN:0, STARVED:-2, BLOCKED:-1, DOWN:-3}` (config.py:36) | deterministic |
| K2 | AR1 coloured noise | `AR1(t)=0.6*AR1(t-1)+eps`, `eps~N(0,σ*0.5)`, `AR1(0)=0`; `clean=base+0.5*σ*sin(2π*(t mod cycle)/cycle)+AR1` (SIM_SPEC 4.1; twin.py:391-399) | per-machine noise stream 0–31 |
| K3 | Obs clamp | `obs=clip(val, base±_CLAMP_SIGMA*σ)`, `_CLAMP_SIGMA=2*ENVELOPE_SIGMA=6.0` (±6σ; twice ±3σ envelope, SIM_SPEC 8) | deterministic |
| K4 | Temperature (NO inertia) | `temp~Uniform(TEMP_RANGES[class])`: feed/form 20–45, process/finish 60–95, inspect-tail/test/rework 20–60, assembly 25–55 °C. Independent uniform per step — no lag, no load coupling, no ambient | noise stream |
| K5 | Pile-up / BLOCKED–STARVED dynamics (emergent, mass-conserved) | Finite FIFO Stores; `down.full at finish → BLOCKED (hold part)`; `up.empty at start → STARVED`; `DOWN preempts both`; propagation delay = cycles + queue + AGV wait (never a fixed lag). Measured pin: seed 287 delay-A5 → A01 piles 7→cap 20, A0 BLOCKED ×8 on 6-step rhythm (SIM_SPEC 9, measured note) | structural (caps §2a, cycles §1) |
| K6 | AGV contention (only shared-resource coupling) | `Resource(2)`, bounded queue 4, `hold~U{4..8}`, `wait=now-t_req` logged per part; SBUF-first priority; backpressure when gated | `rng_agv` (stream 34) |
| K7 | Fault signal deviations (origin-only; downstream NEVER gets a scaled copy) | spike(pulse): `+mag*σ` rect in `[t0,t1)`; drift: `+mag*σ*min(1,(t-t0+1)/dur)` ramp; bias: `+mag*σ` const. `mag∈[4,7]σ` (FAULT_RANGES) | `rng_place` (stream 32) at episode start, fault-list order |
| K8 | Delay (slow-cycle) | `rem = cycle + d` sampled at part-start while window active; `d∈[3,6]`; causes upstream BLOCKED cascade + downstream STARVED; arrival shift nominal+d per hop | `rng_place` |
| K9 | Loss (drops) | Per step in window with prob `drop_rate∈[0.10,0.30]`: `obs[t]=obs[t-1]` (stale-hold; t=0 keeps fresh val) + held part flagged DEGRADE | `rng_drop` (stream 33) |
| K10 | Breakdown (forced DOWN) | Window `[t0, t0+ceil(dur*mttr_mult))`, `mult∈[1,3]`; `dur∈[8,25]`; state DOWN, tput 0, sheds like natural DOWN. `STUCK` aliases to breakdown | `rng_place` |
| K11 | Quality (reject-rate) | While window active on origin: ASM2 reject flips with `rate∈[0.15,0.40]` per completion on `rng_place`; see §2b routing. `DEGRADE/REJECT` flag rides the part object (ch-4), never a signal copy | `rng_place` |
| K12 | Part-carried quality flag (flow coupling, replaces `0.45^lag` echo) | While any spike/drift/bias/delay/loss window covers the machine at step t, held part (or ASM0 batch) `.flag=DEGRADE`; propagates downstream on the part entity; downstream machines read it on arrival only | deterministic given windows |
| K13 | Natural breakdown/repair + GT-exclusion | Per running step (skip inside ANY fault GT window): `fail~U()<1/MTTF → DOWN`, `down_left=Geometric(1/MTTR)-1`; any natural DOWN spanning into a GT window is repaired at the edge (`down_left=0`), so natural-DOWN steps are provably outside fault windows. DOWN/UP events carry `natural/gt_excluded/fault_id`. RWK0 draws none | `rng_fail` (stream 35) |
| K14 | Draw order + RNG discipline (K4 determinism) | Per machine per step: fail-stream draw first, then noise AR-eps, then temp uniform; loss flips on rng_drop inside loss windows; ASM2 rejects on rng_place per completion; AGV holds on rng_agv at request. `SeedSequence(seed).spawn(36)`: 0–31 noise by MACHINE_INDEX, 32 place, 33 drop, 34 agv, 35 fail. 5× same-seed → 0-diverge; replay digest sha256 over sorted-key JSON minus wall/clock keys | numpy only |
| K15 | Fault-window placement | `t0~U[CAL_WIN=120, 300-dur]`, `dur~U{8..25}` (manifest) or given; same-machine windows need ≥5-step gap (validate + `_try_place` 50-retry, dur-8 fallback); multi-fault episodes allowed | `rng_place` |

Explicitly ABSENT (no equations): motor current/power/energy, air/fluid pressure, tool wear,
thermal inertia/ambient coupling, lubrication, vibration spectra/impulses, sensor-health/parity,
reason codes, energy-per-unit. Temp is stateless uniform; throughput is 0/1 per completion
(ASM0 kit counts at creation); buffer level is integer occupancy per Store.

---

## 4) Dependency list (pinned / quirky)

`requirements.txt` (runtime, CPU-only, no torch): `simpy==4.1.2`, `tigramite==5.2.10.1`,
`networkx==3.6.1`, `numpy==2.4.6`, `psutil==7.2.2`, `scipy==1.18.1`,
`stumpy==1.14.1` (offline-validation only, killed as detector per ADR-0009).
`requirements-ci.txt`: `pytest==8.3.4`, pytest-cov, pytest-xdist, ruff, mypy, bandit, pip-audit.
`services/sim_bridge/requirements.txt`: `fastapi==0.141.1`, `uvicorn==0.52.4`, `sse-starlette==3.4.11`.
Environment: Python 3.14.7, CPU-only, no GPU/ROCm (SDD §1, TECHNICAL.md).

Quirks relevant to costing:
- `src/twin.py` imports ONLY `numpy` + `simpy` (+ stdlib: argparse, concurrent.futures, hashlib, itertools, json, math, os, sys, time). `scipy`/`networkx`/`tigramite` are pinned but NOT used on the replay path — scipy is allowed by the upgrade-spec only if a per-change CPU budget passes; replay path must stay NumPy-only Euler steps or 0-diverge breaks. `networkx`/tigramite serve downstream walk/PCMCI (evidence-only, partitioned, never full-32-node).
- `factorysimpy==0.1.0b3` installs but REJECTED (ADR-0012: no trace/fault/RNG API) — hand-rolled SimPy only, real `Resource`/`Store` objects.
- `numpy==2.4.6` RNG assert: `N_STREAMS==36` asserted at spawn; SeedSequence-spawn-indices only (no `(seed,hash(id))` — closeout measured ±1pt F1 jitter from that pattern); zero bare `default_rng(int)`.
- Battery runner lives in `twin.py` (T8): episode seed = `master*1000+row_index` (master default 12345), `Executor.map` order-preserving, wall budget 600 s, tripwire 500 s, per-episode watchdog 120 s.

---

## 5) Weakest structural facts (why line length adds little tracing value)

1. Six (A2–A7, B2–B7) / five (C2–C6) serial `process` machines are byte-identical: same base 70.0, σ 1.5, cycle 6, MTTF 800, MTTR 20, gap caps 25, temp band 60–95 °C, same 7 output channels — no per-machine signature to trace with.
2. No branching, merging, or divergent routing inside any line: pure linear FIFO Stores. Every interior machine has in-degree = out-degree = 1, so a depth-3 walk from any interior symptom has exactly one upstream candidate — length adds steps, not information.
3. Fault physics is origin-only signal deviation + generic flow backpressure: downstream machines observe only identical ±1σ (BLOCKED) / ±2σ (STARVED) offsets and 0/1 throughput dips regardless of which identical clone originated the fault; no machine-differentiating coupling (no wear, thermal lag, current, pressure) exists to separate A3 from A5.
4. Part-carried DEGRADE flag is binary and set identically by 5 of 7 fault classes (spike/drift/bias/delay/loss) — it marks "degraded somewhere upstream" but not which clone.
5. Measured twin output confirms the indistinguishability: at seed 777 the F-21 (drift B5) episode shows zero non-RUN states at B5/B6/B7 and B56 max 2/25 (SIM_SPEC §9 measured note) — i.e. even the running example leaves no localising congestion signature; the B6-BLOCKED/B7-STARVED narrative in the §9.1–9.3 JSON examples is illustrative, not measured.
6. The only true topological discriminators (kit gate ASM0, AGV pool, SBUF divert classes, RWK loop, tail gateways) all sit OUTSIDE the long serial runs — shortening A/B from 10→fewer or C from 8→fewer preserves every branch/join/contention decision while cutting the identical-clone chain that contributes no tracing evidence.

---

## 6) Upgrade-spec C1–C6 status: implemented vs pending

| Item | Status | Evidence / gap |
|---|---|---|
| C1 Drive-current coupling CH8 (`I_m = I_idle + k_m·L_m + φ·w_m + η`, class-scaled k, φ=0.6, idle-powered draw) | PENDING — not implemented | No current channel; no load variable `L_m`; idle BLOCKED/STARVED produce only −1σ/−2σ offsets, no `I_idle>0` draw signal |
| C2 Energy integration CH9 (`E_m+=I·V·Δt`, `E_unit` rollup) | PENDING — not implemented | No energy state or rollup; derived-channel prerequisite C1 absent |
| C3 Wear-knee scalar (`w+=α·L·(1+β·1[w>0.8])`, α=1/240, knee 0.8, β=4, γ=2σ drift coupling, MAINT_EVENT reset) | PENDING — not implemented | No wear state, no knee, no MAINT_EVENT, no slow-subtle drift chain; drift faults are fixed-window ramps, not wear-driven |
| C4 Thermal lag probation (`T_m` first-order, τ=25, `T_amb=22+2sin`, `N(0,0.3°C)`) | PENDING — not implemented | Temp is memoryless `Uniform(class band)` per step; no `T_m` state, no τ, no ambient coupling, no RoC historization |
| C5 Wear-coupled impulse probation (`p=p0+κ·w`, p0=0.005, κ=0.05, A~U(2.5,4.0)σ envelope-only) | PENDING — not implemented | No impulse term; obs noise is pure AR1(0.6)+sine; envelope statistic absent |
| C6 Shared air-pressure header CH10 (`P_air`, τ=8, P_set=6.0 bar, derate <5.2 bar ×0.9, 2nd contention after AGV) | PENDING — not implemented | No header state; AGV pool is the SOLE shared-resource contention; no pneumatics, no derate |
| Supporting (§4/§5/§7): effect-mode tags, CASCADE_CHILD/MULTI_ROOT/SENSOR_VS_PROCESS labels, RC tree, SHF flags, CH8/CH10 parity, multi-scale export, episode wear splits, per-channel calibration checklists | PENDING — not implemented | Fault records carry `{id,class,origin,t0,dur,mag_sigma,extra}` only; no mode/mode-guess, no root_id/hop, no RC/SHF channels; export is 1 sample/step base tick only |

Net: 0 of 6 equation changes implemented; 0 of 9 channel-table rows beyond CH0–CH6 present; fault taxonomy extensions, sampling/ML-split design, and calibration checklists all pending. Hard constraints the reshape must keep: SimPy DES core, T=300, CAL_WIN=120, 1 sample/step, <600 s wall, 0-diverge ×5 on the 36-stream SeedSequence, per-term ablation gate (ΔF1≥3pp or ΔAC@1≥10pp).

---

*End of brief. All values cross-checked against `src/config.py` constants and `src/twin.py` call sites cited above.*
