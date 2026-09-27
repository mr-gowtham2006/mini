# Verdandi weakness register — hostile internal review (2026-09-16)

Scope: repo `/home/shreyas/projects/Verdandi` read verbatim (`src/twin.py` 1834 lines,
`src/config.py` 179 lines, `docs/SIM_SPEC.md`, `docs/SDD.md`, `docs/TEST_PLAN.md`,
`docs/TEST_CASES.md`, `docs/M0B_PREREG.md`, `docs/TECHNICAL.md`, `docs/BUILD_BACKLOG.md`,
`PLAN.md`, `ELENCHUS_DISCOVERY.md`, `spike/REPORT_CLOSEOUT.md`), plus research record
(`04-synthesis/findings.md` F1–F6, follow-ups 01 upgrade-spec C1–C6, 02 training spec,
03 `current-twin-state.md` / `factory-topology-options.md` / `agent-actions.md` /
`physics-couplings.md` X1–X9). No web search. No feature proposals — measurements only.
Settled ADRs are not re-litigated; holes in their stated rationale are in scope.

Central fact conditioning everything below: **`src/` contains exactly two modules —
`twin.py` + `config.py`. `detect.py`, `veto.py`, `walk.py`, `pcmci_job.py`,
`narrate.py`, `verify.py`, `chaincards.py`, `replay.py`, `trail.py` do not exist.**
Every stage downstream of the twin is spec + spike-quarantine only. Any weakness
citing "unimplemented" is therefore structural, not a nit.

Bars/tripwires referenced: K1 (AC@1<30% or flip>40% → cut learning), K2 (F1 drop>30pts →
quantile mandatory), K3 (>5% ungrounded → fallback), K4 (any diverge → subgraph-only),
K5 (ROCm>1wk → CPU), M0b prereg (`docs/M0B_PREREG.md`), M1/M2/M3 gates, MINIPRO-31
(FactoryRCA-Bench internal-first; bench harness does not exist in repo).

---

## §1. Pipeline walk — weakest link per stage (file/line specifics)

### S1. Twin realism — weakest link: observability-equivalent serial clones + memoryless temperature (P0)
- **W-S1a. Six-deep identical process runs carry no tracing signature.** `src/config.py:88-94`
  A2–A7 byte-identical (70.0/1.5/c6/800/20, gap caps 25); same for B2–B7 (`:98-104`),
  C2–C6 (`:108-112`). Same class → same temp band (`TEMP_RANGES`, `:52-62`), same fault
  physics (`twin.py:_fault_dev:344-356`, `_delay_d:377-382`). Downstream observes only
  generic `STATE_OFFSETS` BLOCKED −1σ / STARVED −2σ (`config.py:36`) + 0/1 throughput dips
  regardless of which clone originated. Depth-3 walk from any interior symptom has exactly
  one upstream candidate (in-degree = out-degree = 1). Corroborated by the repo's own
  measured note: `SIM_SPEC.md:253-259` — seed-777 F-21 (drift B5) shows **zero non-RUN
  states at B5/B6/B7, B56 max 2/25**; the B6-BLOCKED/B7-STARVED narrative in §9.1–9.3 is
  illustrative, not measured. Length adds CPU + nodes, not evidence.
- **W-S1b. Temperature is memoryless uniform — C4 has nothing to couple to yet.**
  `twin.py:398-399`: `temp = rng.uniform(tlo, thi)` per step, no `T_m` state, no load
  coupling, no ambient (`current-twin-state.md §3-K4`). Any thermal-derate / X1a / X1b /
  X2 mechanism admitted later changes every process-machine baseline simultaneously;
  current detector thresholds and F1 baselines are conditioned on a channel that will be
  replaced, not refined.
- **W-S1c. Breakdown GT window overruns validation.** `twin.py:_inj_down:319-324` extends
  forced-DOWN to `t0+ceil(dur*mttr_mult)`, mult∈[1,3] — but `_validate:215-227` checks only
  `t0+dur<=T`. Extended end can exceed T and always exceeds the logged `faults[].t1`
  (`_materialize:293`, `t1=t0+dur`). Scoring windows vs physical DOWN windows disagree by
  up to 2×dur; F1/AC@1-lat3 on breakdown faults measure window-bookkeeping, not detection.
- **W-S1d. GT-exclusion sterilises the background.** `twin.py:508-512,704-705,800-801,932-933`:
  any natural DOWN spanning into ANY fault window is repaired at the edge; `enable_natural_breakdown`
  draws are skipped inside windows (`_gw_at:327-332`). Detector never sees fault + natural
  breakdown coincident — the hardest real case (fault during degraded plant) is
  constructionally absent. Recall is measured on a cleaner background than deployment.
- **W-S1e. Loss model contradicts the spec it claims to implement.** Spec
  (`SIM_SPEC.md:179`) mandates clean-median imputation before detection; code
  (`twin.py:603-605` and 3 clones) does stale-hold `obs[t]=obs[t-1]`. Stale-hold injects
  plateaus the quantile detector then flags as its own evidence — LOSS F1 0.739
  (`REPORT_CLOSEOUT.md:20`) is partly self-inflicted signal, and the "naive median"
  baseline the report claims was never the code path.
- **W-S1f. Hidden 32nd store + non-Store kit lists.** `twin.py:1063-1065` creates `_C7TAIL`
  outside the `BUFFERS` roster (`N_BUFFERS=31` assert at `:1066` counts roster only);
  `kit={A:[],B:[],C:[]}` (`:1106`) are unbounded plain lists, not `simpy.Store`. Buffer
  monitor (`_monitor:1036-1042`) records roster order only — kit depth (the ASM0 bottleneck
  state) is invisible to channel-6 logging and to any PCMCI partition window built from
  `buffers[]`. The bottleneck's queue has no time series.
- **W-S1g. RWK0 draws no natural breakdowns** (`twin.py:913-914` comment: "No
  natural-breakdown draws here … bit-identical to T6"). The rework loop — the only cycle
  in the graph, highest H4 cascade value — is the one machine guaranteed failure-free in
  the background. Single-machine exception, unmeasured effect on rework-surge statistics.

### S2. Detection — weakest link: q0.99 estimated from 120 samples; 3/20 faults at R=0 (P0)
- **W-S2a. The flagship threshold is statistically vacuous at CAL_WIN=120.**
  `Q_DET=max(q0.99, Q3+1.5·IQR)` fit on 120 clean steps (`TECHNICAL.md:12`, `twin.py:1194-1197`).
  q0.99 of n=120 is the ~1.2th order statistic — noise, not a quantile. The 1%-FPR story
  the T5 precision bar needs (~300 normals/channel is itself directional-only per
  upgrade-spec §5) cannot be told from 120 samples. M0b prereg §2 freezes "fixed percentile"
  without freezing n — the calibration-sample leg is missing from the prereg.
- **W-S2b. Residual misses are sensitivity, and attribution work is spent.**
  `PLAN.md:33-34` (ADR-0011): echo-attribution killed, ceiling ≈0.76 < 0.85 bar;
  F-06/F-12/F-14 at R=0. Detector file does not exist; the only detector code path is the
  spike battery. F1 0.725 pooled (`REPORT_CLOSEOUT.md:38-39`) with precision 0.602 base —
  the bar failure is false positives + total misses jointly, but all M0 effort went to
  attribution (precision side). No scheduled experiment distinguishes threshold-shape vs
  feature vs cal-window causes (TC-002 lists the classification, no arm exists to run it).
- **W-S2c. Five of seven channels are decorative for detection.** Thresholds apply to
  channels 1–2 only (`SIM_SPEC.md:209-212,229`); channels 3–7 feed "walk context" through
  a walk module that does not exist. Throughput-dips, BLOCKED/STARVED states, buffer
  levels, DEGRADE flags — the only signals that survive observability-equivalence (W-S1a)
  — have no scoring function, no threshold, no ablation. The multi-channel taxonomy is a
  logging schema, not a detector input.
- **W-S2d. M1 hardening deliverables do not exist.** Versioned quantile-refit artifact +
  K2 drop>30pts alarm (`PLAN.md:8`, `BUILD_BACKLOG.md:6-8`): no artifact file, no version
  field in any JSONL schema (`TECHNICAL.md:40-45` trace schemas carry `quantile{}` with no
  version key), no alarm code. K2 cannot fire because its sensor is unbuilt.

### S3. Causal discovery — weakest link: tau_max=2 vs delay d∈[3,6] misspecified by construction; N_FLOOR=800 unfillable from T=300 (P0)
- **W-S3a. DELAY faults outlive the lag window by design.** `REPORT_CLOSEOUT.md:27-31`
  admits it: "long-trace truth at lag-4 with tau_max=2 is misspecified by construction."
  Delay `d∈[3,6]` (`config.py:43`) vs `tau_max=2` (`TECHNICAL.md:16`). The prescribed
  mitigation ("widen PCMCI window to tau≥d evidence-only") is a per-partition promise with
  no cost accounting: tau 6 on ≤10-node partitions triples the CI-test budget the <600s bar
  was sized for at tau 2, and no tau≥d run exists anywhere in the spike traces.
- **W-S3b. N_FLOOR=800 cannot come from one T=300 episode.** `TECHNICAL.md:16`, `SDD.md:4.5`:
  floor 800 samples/partition; episode gives 300 steps, 120 clean. The 800-sample window
  must concatenate across episodes/seeds/faults — a nonstationarity the flip metric then
  punishes or rewards opaquely. KQ1 ("400 unstable, 800 stable") is cited with no trace
  artifact in repo (`spike/traces/` holds v0–v6 + timing only). The floor's own evidence
  is unlocatable.
- **W-S3c. Masked-vs-blind and 10%-corruption arms never ran.** TC-003a/003b are the entire
  empirical basis for F4's "topology-masked" posture (parent findings F4: mask credit
  unseparated from tau/n/averaging). `spike/trace_battery.jsonl` carries one PCMCI block;
  no blind arm, no corrupted-mask arm exists in any trace. The 14.4% number prices a mask
  whose marginal contribution is unmeasured — prior-fragility literature (parent F4,
  contradicting 5) predicts the mask can hurt, and nothing in-repo tests it.
- **W-S3d. Cross-partition discovery is a promissory note.** `SIM_SPEC.md:400-405`: gateway
  edges "evaluated as pairwise boundary checks, never full-graph discovery." No boundary-check
  code, no gateway-edge flip metric, no cross-partition PCMCI trace exists. Walk's "one
  gateway hop exempt from depth" (`SDD.md:4.4`) means production walk is depth ≤3+1=4 while
  every bar and the AC@1 metric are stated at depth ≤3. Cross-partition AC@1 (M0B_PREREG
  gateway quota 7/24 ≈29%, `M0B_PREREG.md:40`) is scored by a walk whose cross-partition
  leg has no evidence job behind it.

### S4. Ranking — weakest link: the ranker does not exist; VETO_ASM2 is the only prior (P0)
- **W-S4a. No `walk.py`, no scoring function, no top-k code.** Only `twin.py`+`config.py`
  exist in `src/`. `walk(alarm, edges, scores)` signature (`SDD.md:4.4`) is spec text.
  AC@1 0.80–0.8125 numbers come from spike scripts, unreproducible from `src/`. The
  "ranked trace" half of the project title has no build artifact.
- **W-S4b. Single-mask prior, single noisy machine.** `VETO_ASM2` (`SDD.md:4.3`,
  `TECHNICAL.md:12`): ASM2 needs 2× margin else demote. ASM2 σ=2.0 is the *declared*
  noisy tail — but C4/X-mechanisms (thermal, current, impulse) will add new noisy channels
  on every machine, and RWK0/B9/C7 tails already have distinct noise-adjacent dynamics
  (AGV-wait variance, rework surge). No other mask exists; no mask-learning or
  per-channel veto is scheduled. First new noisy channel breaks the one-mask design.
- **W-S4c. Orchestration contradiction: subgraph-only replay vs whole-flow context.**
  TC-006 demands "whole-plant replay context (full partition traces + gateway buffers, not
  subgraph-clipped at inject time)" while TC-008/K4 demand subgraph-only execution.
  `SDD.md:2.7-S4` claims "subgraph-only, whole-flow context preserved" — both directions
  in one clause, no boundary-condition spec (what seeds the subgraph edge buffers?).
  Partition-edge faults (the gateway quota, 29% of M0b) are exactly where subgraph
  clipping changes the trajectory. Unmeasured either way.

### S5. Narration/verification — weakest link: verifier checks shape, not truth; control arm never ran (P1)
- **W-S5a. Triple regex proves resolvability, not correctness.** `SDD.md:4.7` verifier:
  regex `\[FW-\d{3} \| E-M\d{2}->M\d{2} \| DET-q95-m\d{2}\]` + set-membership. A sentence
  naming the wrong-but-well-formed fault window + existing edge + real detector peak
  passes. This is precisely the forced-hallucination-to-satisfy-grammar mechanism parent
  F5c cites as pressuring template ≥95% (BoundaryML/TianPan/TDS). TC-005-ext names the
  escape taxonomy (i–v) but the verifier implements none of classes (i)/(iii)/(iv) —
  membership ≠ attribution-correctness.
- **W-S5b. 1.00 grounding on n=37, control arm at n=0.** `TECHNICAL.md:44`:
  `trace_rq3.jsonl` (37 recs). TC-005-ext requires n≥50 same-set template-vs-open+RAG;
  the open+RAG arm was never run. Parent F5c already refuted universal "open ≤65%"
  (FIDES 92–94%, Grounded Decoding, TAMO-FoA) — the live hypothesis is that open+RAG
  matches template on this task, which would revoke verifier-necessity. The single
  experiment that decides H4's fate is fully owed.
- **W-S5c. Wiring, NLI critic, caps: all spec text.** Gemini/Ollama wiring, deberta-v3 NLI
  critic (TC-005-ext "NLI-critic-only"), per-run caps `$0.005/2.5k tok/iter` with abort
  logic (`TECHNICAL.md:20-21`) — none exists in `src/`. K3's sensor (grounding-rate
  accounting) and actuator (fallback engagement) are both unbuilt; K3 cannot fire.
  Fallback-fire-rate≈100% tripwire (system effectively fallback-only, parent F5c alt (2))
  is unlogged because fallback itself is unbuilt (chain-cards 0.39s/30 measured in spike
  only, `TECHNICAL.md:19`).

### S6. Replay — weakest link: determinism proved on the old graph, claimed on the new one (P1)
- **W-S6a. 0-diverge ×5 predates every structural change.** Spike RQ4/RQ5 ran on the
  6-machine signal-copy twin. Current `twin.py` (SimPy Stores, AGV dispatcher with
  `Resource` queue, `_C7TAIL` hidden store, kit lists, rework loop, GT-exclusion) never
  underwent the 5×5 determinism battery at 32 nodes. `replay_digest` (`twin.py:1357+`)
  hashes a record whose schema the digest's own exclusions (`_WALLCLOCK_KEYS:80`) were
  chosen for — self-graded. SimPy `Resource` queue ordering under `concurrent.futures`
  battery runner (T8, `current-twin-state.md §4`: `Executor.map`, per-episode watchdog
  120s) adds an ordering nondeterminism source outside `SeedSequence` (spawn order vs
  completion order, `agv_waits` insertion order across processes within a step).
- **W-S6b. Numpy-version assert is a portability trap.** `SDD.md:4.9`: "numpy version
  recorded+asserted." Viva grader on a different numpy gets assert-fail instead of
  replay — determinism gate converted into environment fragility. No tolerance policy
  (hash RNG streams, not library version) is specced.

### S7. Correction/RL (A1–A7 + masked PPO) — weakest link: zero action hooks in the twin; RL trains against nothing (P1)
- **W-S7a. No M3.5 verb has a SimPy effect.** `agent-actions.md §6` vocabulary
  (REROUTE_WIP / DIVERT_REWORK / DIVERT_BUFFER / REASSIGN_AGV / THROTTLE_FEED /
  SPEED_OVERRIDE / DISPATCH_MAINT): twin has no routing table (lines are hardwired
  `Store` chains, `twin.py:1110-1124`), no arrival-rate parameter (feed heads create
  unconditionally, `:553-559`), no speed parameter (cycle fixed per Table 3.1),
  no AGV task API (dispatcher is autonomous, `:646-676`), no derate state (only
  DOWN). SimPy-rollout-as-shield (`agent-actions.md §4.3`) shields a policy from a
  simulator that cannot express the policy's outputs. Every A1–A6 verify gate
  (Δthroughput>+2%, buffer<95%, no-new-deadlock) is uncomputable.
- **W-S7b. PDR baselines absent, so the RL promotion gate is vacuous.**
  `agent-actions.md §5.2` mitigation: "RL ships only if > PDR on seeded fault suite."
  No PDR (SPT/EDD/CR/nearest-AGV) is implemented, logged, or baselined. RL-vs-heuristic
  comparison — the Contradiction-2 guard — has one side missing.
- **W-S7c. Oscillation guards are prose.** Cooldown + hysteresis + action-switch cost
  (`agent-actions.md §5.1`) have no per-action state, no timer, no cost term in any
  reward (no reward code exists). Reroute-on-every-alarm amplification is unprevented
  by construction, not merely untuned.
- **W-S7d. Topology-A removes the reroute target the agent needs.**
  `factory-topology-options.md §5-A` adds PKG branch as "reroute target" — but the
  reshape is undecided while the agent track assumes a divergent branch exists.
  On the current 32-machine linear topology there is nowhere to reroute *to* except
  SBUF (cap 30, class-gated). A1 REROUTE_WIP is nearly a no-op on today's graph.

### S8. Bench (FactoryRCA-Bench internal-first + causRCA cross-domain) — weakest link: prereg is a template; harness absent (P0)
- **W-S8a. M0B_PREREG fault table is unfrozen scaffolding.** `M0B_PREREG.md:14-38`:
  machines M1–M6/ASM-GW/RWK-GW match neither the A0–RWK0 roster nor topology-A ~22
  names; t0/dur/mag all "tbd" (§1: "Count locked at freeze time", "t0/dur/mag frozen
  per fault before run" — none frozen); `scoring_commit: <tbd>` (§7: "battery invalid
  without it"). The no-subsetting rule (§8) is unenforceable without a harness that
  locks the list. A prereg with tbd in every cell is a template, and batteries run
  against templates are re-seedable until green.
- **W-S8b. No bench harness exists.** MINIPRO-31 internal-first: no bench runner, no
  frozen fault pack, no versioned scoring script in repo. Internal-first currently means
  "unwitnessed." The 20-fault battery lives in quarantined spike scripts
  (`spike/battery_rq1_rq2.py`), which SDD simultaneously quarantines ("rewrite-don't-merge",
  `SDD.md:2.2`) and depends on for every baseline number.
- **W-S8c. causRCA cross-domain arm bridges an unbridged gap.** causRCA
  (`factory-topology-options.md §1b`): ONE CNC lathe, 92 vars, 104-edge expert graph,
  HIL recordings. Verdandi: 32 thin machines × ~1 informative channel. No adapter spec
  exists (which Verdandi channel maps to which causRCA subsystem? how does depth≤3 walk
  traverse a 104-edge expert graph? what is AC@1 when GT is subsystem-level, not
  machine-level?). "Cross-domain" is a label on a comparison that cannot be scored.

---

## §2. Kill-bar / tripwire coverage — what has NO covering bar (the findings that matter)

| # | Weakness | Covering bar? | Verdict |
|---|---|---|---|
| U1 | Multi-root overlap (≥2 independent roots, one alarm) | NONE — battery runs single-fault episodes; `MULTI_ROOT` labels specced (upgrade-spec §4) but no metric, no bar, no battery arm | **UNCOVERED** |
| U2 | Sensor-vs-process ambiguity (S-DRIFT vs P-DRIFT separable only via CH8/CH10 parity that does not exist) | NONE — no parity channel, no mode-accuracy metric, no S-vs-P battery arm | **UNCOVERED** |
| U3 | Calibration-sample insufficiency (q0.99 from n=120) | NONE — M0b prereg §2 freezes percentile, not n; K2 watches collapse vs baseline, not baseline validity | **UNCOVERED** |
| U4 | Breakdown window overrun (physical DOWN exceeds scored GT by up to 2×) | NONE — K-gates score within logged windows; window-bookkeeping error is invisible to all bars | **UNCOVERED** |
| U5 | Cross-partition root via untested gateway hops (evidence job is pairwise-check vapor) | PARTIAL — K1 "evaluated per partition" (`TEST_CASES.md:5`); gateway edges belong to no partition, so no partition's K1 owns them | **UNCOVERED in practice** |
| U6 | C1–C6/X stacking interactions (joint dynamics ≠ sum of marginal ablations) | NONE — F7 ablation gate is per-term (ΔF1≥3pp OR ΔAC@1≥10pp vs ablated arm); no joint-admission or interaction arm | **UNCOVERED** |
| U7 | Operator over-trust / misuse (verifier-passing-but-wrong rank acted on) | NONE — no trust study, no UI warning-effectiveness metric; REQ-009 covers safety-clearance text only | **UNCOVERED** |
| U8 | Latency under load at plant scale (p99 2.6ms is 6-machine; 32-node + partitions + narration unmeasured) | PARTIAL — TST-007/TST-004 assert p99≤3 steps + <600s, but V5 10× probe ran line-scale; plant-scale numbers are "re-measured at M0 exit" (deferred, not gated) | **UNCOVERED until M0 exit** |
| U9 | Distribution shift over semester (topology drift, sensor drift, wear accumulation across episodes) | NONE — "drift→veto-mask re-spike" (`TEST_PLAN.md:50`) is a contingency line, no drift battery, no recalibration trigger values | **UNCOVERED** |
| U10 | docs-vs-code drift (10 spec'd modules, 1 built; SIM_SPEC normative vs stale-hold loss, hidden store, kit invisibility) | NONE — no spec-conformance gate; SDD reuse rule ("rewrite-don't-merge") guarantees the build diverges from the spike baselines without a re-baselining tripwire | **UNCOVERED** |
| U11 | M0b prereg plasticity (tbd cells + no harness → re-seedable battery) | PARTIAL — no-subsetting rule exists on paper (§8) but scoring_commit tbd + no harness = self-policed | **UNCOVERED in practice** |
| U12 | Action-oscillation / reroute amplification (A1–A7 unguarded) | NONE for the twin track — cooldown/hysteresis live in the agent brief only, no twin state, no gate; contingent-track gating (post-M0-M3) means the guard arrives after the actuator | **UNCOVERED** |

Covered (for contrast, not findings): causal instability per-partition (K1), threshold
collapse vs baseline (K2), ungrounded rate (K3 — sensor unbuilt but bar exists),
same-seed diverge (K4), ROCm sink (K5), PA-protocol neutrality (TC-002b control arm —
designed, unrun, but the bar exists).

---

## §3. Plan-level weaknesses

- **W-P1. Topology-A reshape blast radius on frozen artifacts (P0).** 32-machine roster is
  normative in `SIM_SPEC.md §2`, `config.py` roster + `MACHINE_INDEX`, `twin.py` line tables,
  `M0B_PREREG.md §1` (already stale at M1–M6 names), `SDD.md §§2/4` partitions, `TEST_PLAN.md`
  TST-003, `TEST_CASES.md` TC-006 matrix, `TECHNICAL.md` module map. Reshape to ~22
  (Option A: feeders 10→5/5/4 + PAR pair + PKG branch + gate) rewrites all of them plus
  every carried baseline (F1 0.725, AC@1 0.8125, flip 14.4–30.3%, 17.7s/3.1s walls,
  0-diverge, 0.39s fallback). No artifact carries a topology-version pin; `replay_digest`
  hashes obs, not roster. Post-reshape, old and new numbers are silently commensurable.
  Falsifier: `grep -r "32" docs/ src/ | wc -l` stays >0 after reshape with no
  `TOPOLOGY_VERSION` constant consumed by digest + trail export.
- **W-P2. Build order inverts a dependency: M0b battery before twin freeze (P0).**
  `BUILD_BACKLOG.md` sequences M0b first ("only open bar"), but M0b's fault list, gateway
  quota, coverage matrix, and wall budget are all topology-conditioned. Running M0b on the
  32-machine twin then reshaping discards the battery; reshaping first discards the prereg.
  One of the two Burns. No plan addresses which is thrown away.
- **W-P3. Contingent-track gating is sound on paper, porous in practice (P1).**
  A1–A7 + masked-PPO are "contingent post-M0-M3" — yet `agent-actions.md` already fixes the
  7-verb space, RL architecture (GAT/MLP + PPO + masking + shield), and reward sketch, while
  the twin work that gates them (action hooks W-S7a, PDR baselines W-S7b) is scheduled
  nowhere (not in M0b–M5, not in MINIPRO-17). Contingent tracks with completed designs and
  missing prerequisites drift into shadow builds. Falsifier: any RL-training Linear issue
  moves to In-Progress before a PDR-baseline number is logged.
- **W-P4. C1–C6/X stacking order unspecified; per-term gate misses interactions (P1).**
  Upgrade-spec F7 admits per-term on marginal Δ; couplings interact by design (X1a needs
  C4's `T_m`; C3's wear drives C1's current via φ=0.6 and C5's impulse via κ; C6 derate
  changes AGV contention statistics the detector baselines assume). Admission sequence
  (C1→C3→C6? C6→C1?) changes every intermediate battery; joint arms (C1+C3, full-stack vs
  sum-of-marginals) are unscheduled. Worse: X1b thermal-trip *generates* delay/breakdown
  faults naturally, collapsing the seeded-fault GT the battery scores against (a trip is a
  fault without a fault record). Falsifier: full-stack ΔF1 differs from Σ marginal ΔF1 by
  >5pp in either direction on the same seeds.
- **W-P5. Spike-quarantine + reuse-by-rewrite guarantees re-baselining debt (P1).**
  SDD: "spike/ stays quarantine … rewrite, don't merge" while every baseline number ships
  from spike scripts. The build modules will reproduce-or-refute each number; no
  re-baselining tripwire fires if rewritten `detect.py` scores F1 0.65 where spike scored
  0.734 (is the module wrong or the baseline stale?). Falsifier: first built module's
  battery number differs from its spike baseline by >3pp with no verdict rule invoked.
- **W-P6. Linear state: 20+ issues all Backlog except M0.1 done (P2).**
  MINIPRO-17 (M0.2 acquisition), MINIPRO-10 (M0b sensitivity), M1–M5 all Backlog with M0b
  nominally "first." No WIP limit, no milestone exit criteria linked to issue state —
  the tracker cannot show the P2 burn (which battery invalidates which prereg).

---

## §4. Evaluation weaknesses — what the battery cannot catch; numbers likeliest to collapse on causRCA real data

### 4a. Battery blind spots (each maps to U-codes in §2)
1. **Multi-root overlap** (U1): single-fault episodes only; `_validate` permits multi-fault
   lists but no battery arm uses them. Two roots, one alarm → AC@1 undefined (which root
   is "top-1"?). Joint-root-set scoring (upgrade-spec §4) has no implementation.
2. **Sensor-vs-process ambiguity** (U2): S-DRIFT and P-DRIFT are the same `obs` deviation
   by construction (`_fault_dev` origin-only, no parity channel). Battery labels carry no
   mode; mode-accuracy is unscored. Twin cannot generate a pair its detector could separate.
3. **Distribution shift over semester** (U9): one master manifest (`build_faults(seed=12345)`),
   fixed FAULT_RANGES, no drift battery, no recalibration trigger values. Wear (C3) will
   shift baselines within T=300 by design once admitted — the battery that admits it has no
   pre/post-knee stratification gate scheduled (T8 exists in upgrade-spec §6, unscheduled
   in BUILD_BACKLOG).
4. **Operator misuse / over-trust** (U7): no operator study, no warning-effectiveness
   metric, no measured fallback-badge notice rate. The viva demo (examiner clicks one alarm,
   TC-011) rewards the most confident display, not the best-calibrated doubt.
5. **Latency under load** (U8): p99 2.6ms/3.7ms and 17.7s/3.1s walls are 6-machine spike
   numbers; plant-scale evidence jobs (4 partitions × 120s cap = 480s sequential) plus
   narration deadline 8s/alarm × 32 alarms have no measured sum. The <600s bar is asserted
   via partition arithmetic (`SIM_SPEC.md §10`), not a timed run.
6. **Calibration-sample regime** (U3): battery never varies CAL_WIN; the n=120-leg is
   untested because the battery fixes the very parameter that breaks it.

### 4b. Collapse ranking on contact with causRCA real data (most fragile first)
1. **Quantile F1 (most likely to collapse).** Calibrated on sim-clean uniform-temp +
   AR1(0.6) background with GT-exclusion-sterilised windows. Real HIL background is
   nonstationary with coincident degradations; q0.99-from-120 has no real-data analogue.
   Collapse mechanism: threshold learned on sterile background flags everything or nothing
   under real drift. Parent precedent: TCN-GAT SWaT 0.886→0.281 under protocol fix (F1).
2. **AC@1 via topology prior.** Sim prior = sim truth (mask correct by construction).
   causRCA truth is a 104-edge expert graph over subsystems, not machines; Verdandi's
   flat 32-node mask has no mapping onto it. Prior-fragility literature (parent F4:
   imperfect prior sharply degrades) predicts masked-flip *above* blind on real topology.
3. **Grounding rate.** Template triples reference (fault-window, edge-id, detector-output)
   — fault-windows do not exist in real data (no GT injection log). The verifier's FW leg
   is unresolvable off-twin; grounding collapses to detector+edge legs, and the ≥95% bar
   was never measured without the FW leg.
4. **Flip rate (undefined, not collapsed).** Flip needs ≥5 seeded re-runs of identical
   windows; real data has no seed control. The metric does not transfer — it evaporates.
   Any "flip on causRCA" number would require redefining flip (e.g. bootstrap), which is
   a new metric, not the gated one.
5. **0-diverge (vacuous off-twin).** Replay determinism is a property of the simulator,
   not of the world. On HIL recordings replay is inapplicable; the K4 leg of the wedge
   (T+P+R conjunction, parent F6) loses R, reducing the wedge to T+P — where P is already
   collapsed per (3). The conjunction most at risk is the whole wedge.

---

## §5. Severity-ranked register (P0 = threatens a ship bar; P1 = threatens a novelty claim; P2 = limits impact/credibility)

| ID | Sev | Weakness | Bar threatened | Falsifying measurement (broken iff …) |
|---|---|---|---|---|
| W-S2b | P0 | Detector sensitivity floor: 3/20 faults R=0, F1 0.725 vs 0.85 bar, attribution ceiling ≈0.76 | F1≥0.85 (M0b) | M0b battery re-run raw-F1 <0.85 with F-06/F-12/F-14 still R=0 |
| W-S1a | P0 | Observability-equivalent serial clones (no per-clone signature) | AC@1≥70% + ≥10pp ablation (T2) | Graph-free BARO arm within <10pp of topology-prior AC@1 on same seeds (mask adds nothing) |
| W-S3a | P0 | tau_max=2 vs delay d≤6 misspecified by construction | flip<40% per partition per class | Any DELAY-battery partition run with d≥4 shows flip>40% or lag-stability <0.6 |
| W-S4a | P0 | Ranker unimplemented (`walk.py` absent; AC@1 numbers are spike-only) | AC@1≥70% (M0/M1) | `ls src/walk.py` missing at M1 exit while AC@1 reported from spike scripts |
| W-S8a | P0 | M0B prereg unfrozen (tbd fault params, tbd scoring commit, roster-mismatched names) | All M0b bars (prereg validity) | Battery runs with any §1 cell still "tbd" or scoring_commit unpinned |
| W-P1 | P0 | Topology-A blast radius: no topology-version pin; old/new numbers commensurable | All bars (re-baselining) | Post-reshape trail export lacks `TOPOLOGY_VERSION` consumed by digest |
| W-P2 | P0 | M0b-before-twin-freeze ordering: one of battery/prereg burns | M0b validity | M0b runs on 32-machine twin before reshape decision is locked |
| W-S2a | P0 | q0.99 from n=120 calibration (order-statistic noise) | F1≥0.85 + T5 precision bar | Bootstrap CI on q0.99 from 120 samples spans >2σ_detector, or CAL_WIN sweep (120→300) moves F1 by >5pp |
| W-S3b | P0 | N_FLOOR=800 unfillable from T=300 single episode (concatenation nonstationarity) | flip<40% (KQ1) | Concatenated-window flip differs from single-episode flip by >10pp same seeds |
| W-S8b | P0 | No bench harness (internal-first = unwitnessed; spike-quarantined scripts score the bars) | Bench credibility (MINIPRO-31) | Any bar claimed green without a versioned runner + frozen fault pack + pinned scoring commit |
| W-S5b | P1 | Grounding 1.00 on n=37, open+RAG control at n=0 | grounding≥95% + verifier-necessity (H4) | Same-set n≥50 audit: template <90% OR open+RAG ≥80% |
| W-S5a | P1 | Verifier checks shape, not truth (well-formed-but-wrong passes) | grounding≥95% (zero-escape leg) | ≥1 crafted wrong-window/correct-format sentence passes `verify()` |
| W-S3c | P1 | Mask marginal contribution unmeasured (no blind / 10%-corruption arms) | F4 posture (topology-masked PCMCI+) | Same-battery blind within ±5pp of masked, or 10%-corruption degrades flip <3pp |
| W-S3d | P1 | Gateway evidence vapor; walk depth actually 4 (3+1 exempt hop) | Cross-partition AC@1≥70% | Cross-partition AC@1 <70% while intra-partition ≥70%, same battery |
| W-S6a | P1 | 0-diverge proved on 6-machine signal-copy twin, claimed on 32-node SimPy Stores twin | 0-diverge ×5 (K4) | 5× same-seed 32-node run shows any byte-diverge in obs/buffers/agv_waits |
| W-S4c | P1 | Subgraph-only vs whole-flow context contradiction at partition edges | TC-006 + TC-008 jointly | Gateway-quota fault replay diverges subgraph-only vs whole-flow on ranked cause |
| W-S7a | P1 | Zero M3.5 action hooks in twin (no routing/rate/speed/AGV-task parameters) | Correction track (contingent) | Any A1–A6 verb invoked against `run_episode` with no signature change (no-op proof) |
| W-S7b | P1 | PDR baselines absent → RL promotion gate vacuous | RL track (masked-PPO) | RL issue reaches In-Progress with no logged PDR number on the seeded suite |
| W-P3 | P1 | Contingent gating porous (designed agent, unscheduled prerequisites) | Track gating soundness | (same falsifier as W-S7b) |
| W-P4 | P1 | C1–C6/X interaction arms unscheduled; X1b trips generate unlogged faults | Ablation gate F7 | Full-stack Δ vs Σ-marginals differs >5pp, or any X1b trip lacks a fault record |
| W-S1c | P1 | Breakdown GT overrun (scored window ≠ physical DOWN, up to 2×) | F1/AC@1-lat3 on breakdown class | Breakdown-class audit: `ceil(dur*mult)` end exceeds logged `t1` on any battery fault |
| W-S1d | P1 | GT-exclusion sterilises fault+background coincidence | Recall/F1 (optimistic) | Fault+background-coincident arm (exclusion off) drops recall >5pp vs exclusion-on |
| W-S1e | P1 | Loss stale-hold contradicts spec median-impute (self-inflicted plateaus) | LOSS F1 0.739 interpretation | LOSS arm with spec-compliant median-impute differs >3pp from stale-hold code path |
| W-P5 | P1 | Rewrite-don't-merge with no re-baselining tripwire | All spike-carried baselines | First built module differs >3pp from spike baseline with no verdict fired |
| W-S2d | P2 | K2 sensor unbuilt (no refit artifact, no version key, no alarm) | K2 operability | `grep -r "refit_version\|K2" src/ \| wc -l` = 0 at M1 exit |
| W-S5c | P2 | Wiring/NLI/caps unbuilt (K3 sensor+actuator absent) | K3 operability | Same grep test for caps ledger / fallback engagement in `src/` at M2 exit |
| W-S6b | P2 | Numpy-version assert converts determinism into fragility | Viva reproducibility | Replay on grader numpy ≠ 2.4.6 asserts instead of reproducing |
| W-S1f | P2 | Kit queues invisible to channel-6 logging (bottleneck has no series) | TST-003 coverage honesty | ASM0 kit-depth time series absent from any partition PCMCI window |
| W-S1g | P2 | RWK0 background failure-free by exception | Rework-surge statistics | Background DOWN-rate at RWK0 = 0 across manifest while all other machines >0 |
| W-S4b | P2 | One-mask prior (VETO_ASM2) vs coming multi-channel noise | AC@1 robustness | First C4/X-noisy channel produces top-1 false-attribution to a non-ASM2 machine with no veto |
| W-S1b | P2 | Memoryless temp invalidates future C4-coupled baselines | Baseline longevity | C4 admission moves process-machine Q_DET by >10% vs pre-C4 thresholds |
| W-P6 | P2 | Tracker cannot show burns (all Backlog, no WIP/exit linkage) | Plan legibility | M0b starts with prereg tbd cells open and no issue-state transition recorded |
| W-S7c | P2 | Oscillation guards prose-only (no cooldown/hysteresis state) | Correction safety | Two alarms in one window commit conflicting reroutes with no cooldown rejection logged |
| W-S7d | P2 | No reroute target on linear topology (A1 near-no-op pre-reshape) | A1 relevance | REROUTE_WIP candidate set empty (no divergent target) on current roster |
| W-S2c | P2 | Channels 3–7 decorative for detection (no scoring function) | Multi-channel claim | Ablation dropping channels 3–7 changes F1 by 0.00 (proof they are unused) |

---

## §6. Uncovered-weakness priority (for downstream mapping — no fixes proposed)

Highest mapping priority (UNCOVERED, §2): U1 multi-root metric · U2 parity/mode-accuracy ·
U3 calibration-n leg · U4 breakdown-window bookkeeping · U5 gateway-edge ownership ·
U6 interaction arms · U7 trust/misuse study · U9 drift battery · U10 spec-conformance gate ·
U11 prereg freeze + harness · U12 oscillation guard. U8 latency-plant-scale resolves at M0
exit iff the timed run is gated, not merely scheduled.

*End — 12 uncovered (U1–U12), 8 stage weakest-links (S1–S8), 6 plan-level (W-P1–P6),
collapse ranking §4b. All severities carry one-line falsifiers (§5).*
