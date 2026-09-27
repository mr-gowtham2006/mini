# Twin upgrade spec — sim-depth-physics-ml (follow-up 01)

Date: 2026-09-13 · Targets: `src/twin.py` + `src/config.py` + `docs/SIM_SPEC.md`.
Hard constraints (inviolable): SimPy DES core · T=300 · CAL_WIN=120 · 1 sample/step base tick · <600s wall · 0-diverge ×5 seeded replay (36-stream SeedSequence: 32 noise + place/drop/agv/fail) · SciPy allowed iff per-change CPU budget passes.
Method rule (F7): EVERY new physics term ships if and only if it clears its ablation (ΔF1_raw ≥3pp OR ΔAC@1 ≥10pp vs ablated arm, R1–R5 of recommendations); below-delta terms are cut. Default payload on mechanism precedent: wear-knee + current/energy coupling IN; thermal lag + impulse term ON PROBATION (F8: windows-only default).
Claim boundaries (§9) are normative, not advisory.

---

## §1. Per-change physics equations (all O(1) per machine-step, in-step Euler, seeded RNG only)

Notation: `t` step index; `σ` channel sigma; existing base `clean = base + 0.5σ·sin(2πt/cycle) + AR1(0.6)` with state offsets RUN 0 / STARVED −2σ / BLOCKED −1σ / DOWN −3σ, clamp ±6σ — UNCHANGED.

**C1 — Drive-current coupling (CH8 source).** Per machine `m` with load `L_m(t) ∈ [0,1]` (1 when processing, 0.15 idle-powered, 0 when DOWN):
`I_m(t) = I_idle + k_m·L_m(t) + φ·w_m(t) + η_m(t)`, `I_idle = 0.15·I_rated`, `k_m = 0.85·I_rated` (class-scaled: A/B 1.0, C 0.7, ASM 0.5, RWK 0.3), `φ = 0.6` wear-to-current gain, `w_m` wear state (§3), `η_m ~ N(0, 0.05·I_rated)` from machine stream. Idle-but-powered draw (`I_idle > 0` when BLOCKED/STARVED) is the signal the old offsets cannot produce (I16). Coefficients in `config.py: CURRENT = {I_rated, k_scale, IDLE_FRac:0.15, PHI:0.6, NOISE:0.05}`.

**C2 — Energy integration (CH9, derived, no new RNG).** `E_m(t) = E_m(t−1) + I_m(t)·V_nom·Δt`, `V_nom = 400 V`, `Δt = 1 step`; episode rollup `E_unit = Σ_m E_m / units_out` (ISO 50001 energy-per-unit, I16). Derived channel — zero RNG, zero calibration beyond C1.

**C3 — Wear-knee scalar (slow-subtle mechanism).** Per machine: `w_m(t+1) = w_m(t) + α_m·L_m(t)·(1 + β·1[w_m > w_knee])`, `α_m = 1/240` per loaded step (≈ knee at ~200 loaded steps), `w_knee = 0.8`, `β = 4.0` post-knee acceleration (modified-Archard knee, A-S6, no geometry updates). Couplings: drift injection `+γ·w_m(t)` into obs with `γ = 2.0σ` at `w=1`; current via `φ` (C1); impulse rate via §3. `MAINT_EVENT` resets `w_m → 0` + logs reset flag (I12 overhaul-baseline-reset precedent; I11 pre/post-maintenance labeling). Coefficients in `config.py: WEAR = {ALPHA:1/240, KNEE:0.8, BETA:4.0, GAMMA_SIGMA:2.0}`.

**C4 — Thermal lag (PROBATION — ablation-gated).** First-order lumped node per machine (A-S9 hybrid-thermal direction, ROM-grade): `T_m(t+1) = T_m(t) + (dt/τ_m)·(T_ss,m(L) − T_m(t))`, `T_ss,m(L) = T_amb + ΔT_class·L_m(t)`, `τ_m = 25 steps` (class bands per existing temp table), `T_amb = 22 + 2·sin(2πt/300)` ambient coupling, `dt = 1`. Temp obs = `T_m + N(0, 0.3°C)` + RoC deadband historization note (I03: 0.25°F/day pattern). SciPy NOT needed (analytic Euler step). Coefficients in `config.py: THERMAL = {TAU:25, DT:1, T_AMB:22, T_SWING:2, NOISE:0.3}`.

**C5 — Wear-coupled impulse term (PROBATION — H2 battery decides; default OFF).** On obs channel only: with per-step probability `p_m(t) = p0 + κ·w_m(t)`, `p0 = 0.005`, `κ = 0.05`, add `A·σ`, `A ~ Uniform(2.5, 4.0)` (below the 4–7σ fault band by construction — sub-fault impulsive texture, never a fault label). Envelope-domain acceptance only (F12 instrument); raw-waveform matching prohibited. Coefficients in `config.py: IMPULSE = {P0:0.005, KAPPA:0.05, A_LO:2.5, A_HI:4.0, DEFAULT:"off"}`.

**C6 — Shared air-pressure header (CH10, plant coupling).** Single header state: `P_air(t+1) = P_air(t) + (dt/τ_air)·(P_set − P_air) − Σ_m c_m·L_m(t) + ξ(t)`, `P_set = 6.0 bar`, `τ_air = 8 steps`, `c_m = 0.02` (A/B), `0.01` (C/ASM), `ξ ~ N(0, 0.02)` from the `place` stream. Machines with `P_air < 5.2 bar` get efficiency derate `L_m × 0.9` (second shared-resource contention after AGV pool; I20 header+branch pattern, P1 priority). Coefficients in `config.py: AIR = {P_SET:6.0, TAU:8, C_AB:0.02, C_CASM:0.01, NOISE:0.02, DERATE_P:5.2, DERATE_F:0.9}`.

---

## §2. Channel table (CH8–CH10 new; existing CH0–CH6 unchanged + additions)

| Ch | Signal | Rate | Source equation | Noise spec (ships per §7) | Use |
|---|---|---|---|---|---|
| CH0–CH6 | existing 7 (vibration-RMS-grade, temp, throughput, state, buffer, AGV, quality) | 1/step | unchanged | existing Q_DET retained | detection/traceback base |
| CH8 | motor current `I_m` | 1/step RMS-grade | C1 | `N(0,0.05·I_rated)` + Q_DET auto-derived (§7) | P0: cheapest high-value channel (S2); idle-waste + wear-coupling carrier |
| CH9 | energy `E_m`, `E_unit` | 1/step (derived) | C2 | inherits C1 (no new RNG) | P0: energy-per-unit OEE calc (I16); rework-pass energy attribution |
| CH10 | header air pressure `P_air` | 1/step | C6 | `N(0,0.02 bar)` + Q_DET auto-derived | P1: shared-resource contention + cascade propagation medium |
| WSTATE | wear scalar `w_m` (internal + exported) | 1/step | C3 | deterministic given loads (seeded); export as ML feature | slow-subtle drift-chain mechanism; episode-split stratifier |
| ENV | impulse-envelope derived feature | multi-scale export (§5) | C5 (if admitted) | envelope-SNR statistic (F12) | P1-probation: envelope acceptance only |
| THERM | thermal state `T_m` | 1/step | C4 (if admitted) | `N(0,0.3°C)` + Q_DET auto-derived | P1-probation: lagged-temp drift context |
| RC | reason code (3-level tree) | event-driven | §4 taxonomy | categorical; top 20–30 modes cover 70–80% (I21) | P1: OEE downtime-code hierarchy; every unplanned stop ≥2 min coded (I19) |
| SHF | sensor-health flag | 1/step | parity outcome (§4) | binary/ternary: OK / SUSPECT / DROPOUT | P1: dropout→FP suppression (I12); parity-check output channel |

---

## §3. Wear state + impulse + thermal-lag summary (cross-reference)

Wear-knee (C3) is the slow-subtle backbone: knee at `w=0.8` (~200 loaded steps ⇒ reachable inside T=300 episodes on bottleneck machines, marginal elsewhere — that asymmetry is INTENTIONAL: drift-chain faults seed on high-utilisation machines). Post-knee `β=4` drives the drift/current/impulse couplings that the graph-distance complement (H4) must trace. Thermal lag (C4) and impulse (C5) are probation terms: implemented behind config flags (`THERMAL.enabled`, `IMPULSE.enabled`, default `false`), admitted only via R1/R2 batteries.

---

## §4. Fault-taxonomy extensions (file: `src/config.py: FAULT_RANGES` + new `FAULT_MODES`)

**Effect-mode tag (A-S13 MATERO direction, S5):** every fault class carries `mode ∈ {observation-only, physical-propagation}`. Mapping: spike/drift/bias + loss → `observation-only` (signal deviation by construction, no plant-state change); delay/breakdown/quality + NEW wear-knee drift + air-derate + cascade children → `physical-propagation` (counterfactual trajectory compatibility testable).

**Multi-origin overlap labels:** new fault kinds: `CASCADE_CHILD` (downstream starvation/blockage induced by an upstream fault — labelled with `root_id` + `hop` count, depth≤3 walk compatible); `MULTI_ROOT` (alarm with ≥2 independent roots — labelled with `root_ids[]`, joint-root-set scoring per A-S13); `SENSOR_VS_PROCESS` pair faults (simultaneous sensor-bias + process-drift on one machine, separable ONLY via parity channels CH8/CH10 cross-check + reconstruction contribution, A-S8a direction).

**Sensor-vs-process labelled mode taxonomy:** `S-DRIFT` (sensor drift, observation-only, parity-violating), `S-DROPOUT` (stale-hold, SHF=DROPOUT, I12 pattern), `P-DRIFT` (process drift, physical-propagation, parity-consistent), `P-WEAR-KNEE` (wear-driven, WSTATE>0.8, graph-distance traceable). Battery labels record the generating mode; detectors must output the mode guess (mode-accuracy scored alongside AC@1).

**Reason codes (RC, I21 3-level tree, P1):** L1 {MECH, ELEC, PNEU, CTRL, MATL, OPR}; L2 e.g. ELEC→{MOTOR_OVLD, DRIVE_FLT, SENSOR_FLT}; L3 machine-specific. `VETO_ASM2` + flip-gate consume RC (precision mechanisms, H3-P2). Every unplanned stop ≥2 min gets a code (I19).

**Sensor-health flags (SHF, I12 precedent, P1):** per channel per step: `OK / SUSPECT (parity residual >3σ) / DROPOUT (stale-hold detected)`. Dropout windows are EXCLUDED from detection scoring (no zero-value-as-fault FP, C05/C06) and logged for the H3 healthy-window replay.

---

## §5. Sampling / ML-dataset design (H2 surviving reading: tiered)

**Base tick:** 1 sample/step for ALL historized channels (CH0–CH10 + WSTATE + THERM + SHF) — MES-grade normative anchor (I04 scan classes 1 s/5 s/20 s/10 min; I08 OPC UA 1 Hz cap; I03 RMS-historized pattern). Never downsample fast derivations (A-S12 ≥8pp direction).

**Multi-scale export (ships by default):** per detection window, export 0.5/1.0/2.0-step-equivalent aggregates (mean, RMS, min/max, envelope-band energy) — A-S10 prescription. Envelope-band energy uses the F12-licensed statistic (GES2N-style targeted SNR or sub-band energy, pre-registered per R2).

**Impulse tier (probation):** C5 OFF by default; admitted only via R2 three-way battery. No bearing-frequency output ever (§9).

**Episode splits (prognostics-ready within T=300 limits):** export episodes with `episode_id` (seed), `wear_endpoint` (max `w_m`), `maint_flag` (MAINT_EVENT present/absent). Train/test splits are EPISODE-SEEDED (no window leakage across episodes sharing a seed trajectory); wear-stratified (pre-knee vs post-knee episodes balanced across folds); pre-/post-maintenance window labels (I11: pre-maintenance anomalous / post-maintenance normal; normal-only/one-class training supported with free GT from the twin).

**Imbalance + leakage discipline:** 50%-overlap FFT windows (I13) + SMOTE-family oversampling with resampling INSIDE CV folds only (I15 — leakage otherwise); minority classes = post-knee wear + MULTI_ROOT + SENSOR_VS_PROCESS pairs. Calibration normals: ~300/episode-channel guidance for 1%-FPR claims is DIRECTIONAL only (U10 — Deng constants do not transfer).

---

## §6. Acceptance tests (mapped to M0b battery bars + precision-side bar)

| # | Test | Bar | Maps to |
|---|---|---|---|
| T1 | Raw point-wise detection-F1 (PA-off, fixed-percentile), quantile vs GDN-light vs MP guardrail | F1 ≥0.85 raw (SIM_SPEC standing bar; M0b-scoped, conditional per parent F5) | parent F1/F5; H1 ΔF1 ≥3pp per-term gate |
| T2 | Ranked-trace AC@1 depth≤3 + ≥10pp graph ablation (BARO-style graph-free control) | AC@1 ≥70% + ablation gap ≥10pp | parent F5b; H4 Tripwires A/B/C + P3 |
| T3 | Template+verifier provenance vs open+RAG control | ≥90% + escape taxonomy (parent standing design) | parent F5c |
| T4 | Flip-gate + wall/diverge | flip <40%, <600s wall, 0-diverge ×5 (36-stream SeedSequence) | SIM_SPEC battery |
| T5 | NEW precision-side bar (H3 surviving reading) | Healthy-window alert rate measured at 20, 150 (edges) + 5 (stability) per 1000; demotion valid only if stable across ≥2 of 3 budgets; per-machine worst-zone rates recorded; transient-inclusive pool | F9; EEMUA 191 anchor (I23/I24) |
| T6 | NEW envelope-domain acceptance (H2) | Envelope-separation gain ≥15% + conditioned-F1 within ±3pp; three-way arms | F8; F12 instrument |
| T7 | NEW calibration firing test (H5) | Three arms (calibrated / uncalibrated-ported / global-auto); tripwire: uncalibrated >150/1000 OR ΔF1 ≤ −5pp; scoping: global/auto within ±2pp F1 & ±3/1000 of hand-calibrated | F11 |
| T8 | NEW drift-subset causal test (H4) | Wear-drift subset AC@1 ≥50% via complement, ≥10pp gain, <10pp abrupt regression | F10 |

---

## §7. Ship-with-calibration per-channel checklist (H5 auto-rule + manual fallback)

Each channel (CH8, CH10, THERM, ENV; CH9 inherits C1) ships ALL of:
- [ ] Auto-derived operating point: Q_DET threshold from nominal-residual distribution (M2AD-style GMM+Gamma layer or normalised rarity score or self-adapting limits — implementation choice logged).
- [ ] Noise characterisation: distribution + parameters + seed-stream attribution (table in §2, "Noise spec" column).
- [ ] Manual-spec fallback: hand-written Q_DET + noise spec reviewed and stored (audited fallback if auto arm underperforms per R5 scoping tripwire).
- [ ] Healthy-window rate: alerts/1000 at 20/150/5 budgets, global + worst-machine (T5).
- [ ] Recalibration triggers: detector/process/tooling/material change list (Deng direction, U10) + MAINT_EVENT baseline-reset + suppression windows (I12).
- [ ] Transfer-claim entry: "unvalidated — no paired-real" until §9 paired-real protocol completes.
A channel missing ANY box does not ship (R5 resolution rule; F11).

---

## §8. CPU / determinism budget per change + MINIPRO-17 mapping

Per-change wall budget (T=300, 32 machines, 5-seed replay; measured on reference CPU, logged per R1): C1 +2% / C2 +0% (derived) / C3 +3% / C4 +3% (probation) / C5 +1% (probation) / C6 +2% / RC+SHF logging +1% / multi-scale export +4% / Jaccard complement (offline analysis, outside replay) +0% replay. Total admitted payload ≤ +12% vs current baseline — well inside <600s (A-S14 event-stepped pattern; A-S15 heap-queue FIFO replay). Determinism: all new RNG from the existing 36-stream SeedSequence (no new streams except NONE — C5 draws from machine noise stream, C6-ξ from `place` stream); SciPy use restricted to offline analysis (GES2N-style CG filter, PCMCI+/FCI libraries) — replay path stays NumPy-only Euler steps. Any term breaking 0-diverge ×5 is cut regardless of metric gain.

MINIPRO-17 G1–G10 mapping: **P0** — CH8 current + CH9 energy + transfer-boundary documentation (§9 Sim2Real bar) + ablation-gate battery (R1). **P1** — CH10 air + reason-code tree + thermal lag (probation) + sensor-health flags + auto-calibration arms (R5) + precision-range battery (R3). **P2** — waveform-boundary enforcement (no-bearing-frequencies audit) + cascade-chaining labels + flood-grouping narration hooks (deferred to narration layer per S5: flood alarms group along flow, I25; alarm-flood narration NOT in the twin core).

---

## §9. Explicit claim boundaries (normative)

1. **No bearing frequencies.** The twin's 1/step channels are RMS-grade historian outputs (I03/I04/I08). Envelope statistics are licensed (F12); characteristic defect frequencies (BPFO/BPFI/BSF), resonance bands (2–10 kHz), and any waveform-level diagnosis claim are OUT OF SCOPE. Acceptance is envelope-domain only (T6).
2. **No Sim2Real without paired-real.** No transfer, deployment, or "plant-validated" claim ships on synthetic-only evidence (I14: sim-only 0.2516 → 0.8853 mAP only with 50 paired-real + 500 synthetic; domain randomisation via FAULT_RANGES retained per I14b, but randomisation ≠ validation). Paired-real protocol (Q10): ≥50 paired-real windows per new channel before any transfer magnitude is cited.
3. **No unlocked numerics as established.** Per-term ΔF1/ΔAC@1, ≥15% separation, demotion counts, drift-subset AC@1, calibration gaps — all conditional on the unrun M0b battery (F7–F11 contested). Only F12/F13 narrow claims are established.
4. **No platform/ROI framing** (parent F2 Locked): verdicts are battery-scoped; calibration labor is cost-ledger context, not value proof. **No PA metrics** (parent F1 Locked): all F1 bars raw point-wise, PA-off.
5. **Complex-anomaly coverage contract:** cascades/multi-origin via effect-mode tags + multi-root labels + CASCADE_CHILD hop chains (depth≤3); slow-subtle via wear-knee state + MAINT_EVENT resets + drift-chain subset scoring (T8); sensor-vs-process via parity channels (CH8/CH10 cross-checks) + SHF flags + labelled mode taxonomy (§4). A fault class lacking its §4 tags + §6 test mapping is unspecified, not covered.

---

*End — upgrade spec: 6 equation-defined changes (2 probation), 9-row channel table, 4-part fault taxonomy, tiered sampling + episode-split ML design, 8 acceptance tests, per-channel calibration checklist, per-change CPU budgets, MINIPRO-17 P0/P1/P2 mapping, 5 normative claim boundaries.*
