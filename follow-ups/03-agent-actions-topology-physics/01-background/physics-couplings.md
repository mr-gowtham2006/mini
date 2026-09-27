# Physics couplings beyond C1–C6 — coupling table for the Verdandi twin

Date: 2026-09-16 · Parent spec (DO NOT re-derive, reference by ID):
`follow-ups/01-sim-depth-physics-ml/04-synthesis/upgrade-spec.md`
(C1 drive-current CH8 · C2 energy CH9 · C3 wear-knee WSTATE · C4 thermal-lag probation ·
C5 impulse probation · C6 air-header CH10).
Current twin baseline: `clean = base + 0.5σ·sin + AR1(0.6)`, state offsets
RUN 0 / STARVED −2σ / BLOCKED −1σ / DOWN −3σ, clamp ±6σ; temperature = memoryless
uniform band; throughput 0/1; no current/power/pressure/flow/wear/energy/ambient.

Goal: rows a developer can turn into Linear issues behind config flags
(default OFF until battery-admitted — same probation discipline as C4–C6).
Every equation is per-step O(1), in-step Euler, seeded RNG only.

---

## 0. Short answer to the user's example question

**Yes — rising temperature should decrease efficiency (thermal derating), and the
cheap way is an algebraic derate factor, not a thermal FE model.** Two one-line
mechanisms cover ~90% of plant realism here:
(a) winding resistance rises with temperature (copper α = 0.00393/°C) → same
mechanical load draws more current → more I²R heat (weak positive feedback);
(b) above a rated temperature the drive/controller derates usable load/speed
(NEMA MG1 / IEC 60034 practice: rated output assumes ≤40 °C ambient and class
temperature rise; sustained operation above that forces derate or trip).
Both are algebraic, O(1), zero new RNG. Details: rows X1a/X1b.

---

## 1. Coupling table (new rows X1–X9; C1–C6 referenced, not repeated)

### X1a — Winding temperature → resistance → current (I²R feedback)
- **Cause → effect:** THERM `T_m` → CH8 `I_m`.
- **Equation:** `R_m(t) = R_0·[1 + α_cu·(T_m(t) − 20)]`, `α_cu = 0.00393 /°C` (copper, exact physical constant).
  Current correction: `I_m(t) += I_load,m(t)·α_cu·(T_m(t) − 20)`, where `I_load` is the
  load-proportional part of C1 (i.e. total minus `I_idle`). Consumed-power heat source for C4:
  `T_ss,m += k_cu·I_m(t)²`, closing the C1→C4 loop (replaces C4's pure `ΔT_class·L` steady state
  with `T_ss,m = T_amb + ΔT_class·L_m + k_cu·I_m²` — still O(1)).
- **Parameters:** `α_cu = 0.00393` (fixed); `k_cu` calibrated so I²R term ≈ 20–40% of `ΔT_class`
  at rated load (range 0.1–0.5 × ΔT_class/I_rated²); `T` in °C from C4 state.
- **Unlocks:** load→current→temperature→current slow positive-feedback drift (a genuine
  plant-like runaway precursor, still bounded by X1b trip); makes sustained-overload episodes
  look different from brief ones — a slow-subtle trace the graph complement (H4) can walk.
- **CPU:** +0% replay (algebraic, no RNG). **Gate:** include in the C4-probation battery arm
  (R1/R2); admit iff ΔF1_raw ≥3pp or ΔAC@1 ≥10pp vs C4-without-X1a.

### X1b — Thermal derating + trip (temperature → usable load)
- **Cause → effect:** THERM `T_m` → effective load `L_eff,m` (throughput/cycle-time channel).
- **Equation:** `L_eff,m(t) = L_m(t)·D(T_m)`,
  `D(T) = 1` for `T ≤ T_rated`; `= 1 − κ_der·(T − T_rated)` for `T_rated < T < T_trip`;
  `= 0` (forced DOWN + BREAKDOWN label + reason code ELEC/MOTOR_OVLD) for `T ≥ T_trip`.
  Mirrors NEMA/IEC practice (motors rated to 40 °C ambient / class rise; protection per
  IEC 60034-11 thermistor/RTD trip) and DOE guidance to size motors ~75% load for peak efficiency.
- **Parameters:** `T_rated` = class band top + 10–15 °C (plausible 60–90 °C winding-equivalent
  in twin units); `κ_der` = 0.005–0.02 /°C (≈ NEMA harmonic-derating magnitudes: ~3.5% @ 5% HVF);
  `T_trip = T_rated + 25–40 °C`. Hysteresis on reset (restart only when `T < T_rated + 5 °C`)
  to avoid chatter — one boolean per machine.
- **Unlocks:** over-temperature slowdown (cycle stretch = natural `delay`-fault generator with a
  physical root cause); thermal-trip breakdowns traceable T→L→starvation downstream
  (CASCADE_CHILD hop chains, §4 of spec). **CPU:** +0%. **Gate:** same battery as X1a; trip
  arm additionally checked against T4 flip-rate (<40%).

### X2 — Bearing temperature ↔ vibration bidirectional coupling
- **Cause → effect (both directions):** vib-RMS obs ↔ THERM `T_m`.
- **Equation:** heat side: `T_ss,m += μ_bv·max(0, V_m(t) − V_ref)` (friction from excess vibration,
  `V_m` = vib channel obs in σ units, `V_ref ≈ 1.0σ`);
  vibration side: `V_obs,m(t) += λ_tv·max(0, T_m(t) − T_warn)` (hot bearing runs rough;
  lubricant degradation → friction → roughness).
  Follows the SKF-documented damage sequence (envelope → vibration → heat: early spall in
  enveloped acceleration, advanced spalling raises vibration AND temperature; sudden
  temperature rise under unchanged operating conditions = developed damage / imminent failure).
  **Deliberately envelope/RMS-grade only** — no BPFO/BPFI/BSF (spec §9 boundary stands).
- **Parameters:** `μ_bv` = 0.1–0.3 (°C per σ, small — heat is a lagging indicator);
  `λ_tv` = 0.2–0.5 σ/°C above `T_warn` (= `T_rated` − 10 °C); both class-scalable.
- **Unlocks:** THE canonical H4 SENSOR_VS_PROCESS pair: temp-sensor drift (S-DRIFT,
  parity-violating: T rises, V flat) vs real bearing degradation (P-DRIFT, parity-consistent:
  V and T rise together, separable only via CH8/CH10-style cross-check + SHF flags).
  Also feeds X9 life consumption.
- **CPU:** +0% (algebraic, no RNG). **Gate:** dedicated SENSOR_VS_PROCESS battery arm:
  mode-accuracy (S-DRIFT vs P-DRIFT classification) must beat the no-coupling arm by ≥10pp;
  else cut (F7 method rule).

### X3 — Wear → cutting force → surface quality chain
- **Cause → effect:** WSTATE `w_m` → cutting-force proxy `F_m` → quality/reject channel (ASM focus).
- **Equation:** `F_m(t) = F_0·L_m(t)·(1 + ζ·w_m(t))`, `F_0 = 1.0` normalized;
  reject probability: `p_rej,m(t) = r_0 + ρ·max(0, w_m(t) − w_knee)` with existing quality-fault
  range (15–40% reject) as the ceiling clamp. Force proxy exported as ML feature (no new channel
  required; optionally folded into CH8 via C1's existing `φ·w` term — X3 only ADDS the quality leg).
  Grounding: Zorev/flank-force literature — flank forces negligible at low wear, comparable to
  rake-face forces at high wear / hard materials (i.e. super-linear in wear: the knee shape C3
  already has is the right functional form); Taylor `V·T^n = C` / Colding / Archard-Ståhl lineage
  for the wear-rate side; end-of-life criterion VB ≈ 0.3 mm uniform / 0.5 mm localized maps to
  `w_knee = 0.8 → w = 1.0` window.
- **Parameters:** `ζ` = 0.5–1.5 (force rise at w=1); `ρ` scaled so post-knee reject rate lands
  inside the existing 15–40% FAULT_RANGE (no config-range change needed); `r_0` = baseline reject.
- **Unlocks:** P-WEAR-KNEE → quality drift-chain faults at ASM2 (the machine the current
  `quality` fault already targets): wear, current, force, and reject rate all drift together —
  a 4-channel parity-consistent signature ideal for T8 drift-subset causal test
  (wear-drift AC@1 ≥50% via complement). **CPU:** +0–1% (one Bernoulli draw from machine stream
  per loaded step). **Gate:** T8 battery; admit iff drift-subset AC@1 gain ≥10pp with <10pp
  abrupt-fault regression.

### X4 — Motor start inrush + voltage-sag coupling (extends C1)
- **Cause → effect:** state transition (any → RUN / DOWN → restart) → CH8 transient; plant bus → all CH8.
- **Equation:** on start: `I_m(t+s) += I_inrush·exp(−s/τ_inr)`, `s = 0,1,2…` steps since start,
  `I_inrush = 4–6 × I_rated` at s=0 (NEMA Design B locked-rotor magnitudes; only the envelope is
  modeled — sub-step inrush shape is out of scope at 1 sample/step, the decay tail is what the
  historian sees). Bus sag (cheap global): `I_avail factor = 1 − κ_sag·(n_starting(t)/N)` applied
  to all `I_m` the same step (simultaneous starts visibly interact — a plant-like commissioning/
  restart phenomenon nearly free).
- **Parameters:** `I_inrush/I_rated` = 4–6; `τ_inr` = 1–3 steps (tail visible 2–5 steps);
  `κ_sag` = 0.02–0.05 per concurrent starter.
- **Unlocks:** restart-after-maint coordination faults (MAINT_EVENT → simultaneous restart →
  sag → slow ramp — a cascade with a benign root, testing the "don't flag recovery" discipline);
  inrush tails as labeled transients for the H3 healthy-window replay pool (transient-inclusive).
- **CPU:** +1% (per-machine start counter + one exp per starting machine). **Gate:** T5
  precision-side battery — admit iff healthy-window alert rate does not worsen at any of the
  20/150/5 budgets (transients must be learnable-as-normal, not new FP sources).

### X5 — Pneumatic extensions: leak fault + actuator slowdown (extends C6)
- **Cause → effect:** leak state → CH10 `P_air`; `P_air` → cycle time (delay-fault coupling).
- **Equation:** leak fault (new FAULT_MODES entry, physical-propagation): `P_air(t+1) -= λ_leak`
  per step while active (`λ_leak` = 0.01–0.05 bar/step — DOE compressed-air data: 20–35% of
  output routinely lost to leaks; 2 psi ≈ 0.14 bar setpoint change ≈ 1% energy). Actuator
  slowdown: machine cycle `c_m,eff = c_m·[1 + κ_pneu·max(0, P_min − P_air)]` (low pressure
  stretches cycles before it derates load — two-stage response: slow THEN derate at C6's 5.2 bar).
- **Parameters:** `λ_leak` range above; `P_min` = 5.5 bar (slowdown band 5.5→5.2, derate below 5.2);
  `κ_pneu` = 0.1–0.3 per bar (10–30% stretch per bar — matches CAGI case-study magnitudes).
- **Unlocks:** leak → header sag → multi-machine slowdown → throughput dip with NO machine fault —
  a shared-cause cascade (flood-grouping narration hook, MINIPRO-17 P2); leak-vs-demand-spike
  ambiguity (is the header sagging from a leak or from coincident demand? — needs the
  compressor-duty signal as parity channel). **CPU:** +1% (one global state + per-machine
  cycle scaling already computed). **Gate:** T2 cascade battery (AC@1 on leak-rooted cascades,
  depth ≤3, ≥10pp graph ablation).

### X6 — Ambient second-order terms (humidity + sensor cross-sensitivity)
- **Cause → effect:** ENV `H(t)` (new scalar, same cost as `T_amb`) → obs bias (S-DRIFT generator)
  + quality channel.
- **Equation:** `H(t) = H_base + H_swing·sin(2πt/300 + φ_h)` (diurnal, period = episode);
  sensor cross-sensitivity: `obs_m(t) += κ_h·(H(t) − H_ref)` on temperature-class channels
  (MOX/thermal-sensor literature: systematic Rs/baseline drift with T/RH; TI rule of thumb
  ≈ −3 %RH reading per +1 °C local heating — same algebraic shape); prolonged high-H exposure:
  `p_rej += κ_hq·max(0, H − H_corr)` (corrosion/finish effect, ASM only).
- **Parameters:** `H_base` = 40–60 %RH, `H_swing` = 10–20 %RH; `κ_h` small (≤0.1σ per 10 %RH —
  second-order BY DESIGN, must stay below fault band); `H_corr` = 70 %RH threshold.
- **Unlocks:** cheapest S-DRIFT fault generator (humidity-driven sensor bias is observation-only,
  parity-violating vs X2's parity-consistent bearing heat — the pair the mode classifier trains on).
  **CPU:** +0%. **Gate:** X2's SENSOR_VS_PROCESS battery (X6 ships only as the "sensor side" of
  the pair; never alone — an unpaired synthetic bias with no parity partner teaches detectors
  to chase ghosts).

### X7 — Lubrication/contamination slow state (feeds X2/X9)
- **Cause → effect:** lube state `ℓ_m ∈ [0,1]` (scalar per machine, decays with loaded steps,
  restored by MAINT_EVENT) → vibration noise floor + X9 life rate.
- **Equation:** `ℓ_m(t+1) = ℓ_m(t) − α_ℓ·L_m(t)` (α_ℓ ≈ 1/400 — slower than wear α=1/240);
  `V_obs noise σ_vib,m = σ_0·(1 + 0.5·(1 − ℓ_m))` (dry bearing = noisier); X9 rate multiplier
  `×(1 + (1 − ℓ_m))`. (SKF/Fluke practice: poor lubrication → smearing/fretting → heat+noise;
  relubrication causes 1–2 day natural temperature excursion — model as brief `+ΔT_relube`
  after MAINT_EVENT, `ΔT_relube` = 1–2 °C decaying with τ=10 steps.)
- **Parameters:** `α_ℓ` = 1/300–1/600; relube bump 1–2 °C, τ = 5–15 steps.
- **Unlocks:** maintenance-quality ambiguity (post-maint temperature bump must NOT score as
  fault — H3 healthy-window + I11 pre/post-maintenance labeling precedent); lube-neglect drift
  as a second slow-subtle root besides wear. **CPU:** +1%. **Gate:** T5 precision battery
  (post-maint bump must not raise healthy-window alert rate) + T8 drift battery for the neglect arm.

### X8 — Energy extensions: idle-waste + peak-demand (extends C2/CH9)
- **Cause → effect:** state + CH8 → plant power `P_plant(t)` → E_unit attribution + demand peak.
- **Equation:** `P_plant(t) = Σ_m I_m(t)·V_nom` (already have terms — pure rollup, +0% CPU);
  idle-waste ledger: `E_idle = Σ over BLOCKED/STARVED-but-powered steps` (C1's `I_idle > 0`
  makes this nonzero — the signal old offsets could never produce, now monetized);
  demand peak: `P_peak = max over sliding-15-step window of P_plant` (demand-charge proxy;
  ISO 50001 EnPI-style: report `E_unit`, `E_idle/E_total`, `P_peak` per episode — EnPIs as simple
  ratios/regression baselines per DOE EnPI tool practice: energy vs production-volume regression,
  NOT absolute kWh claims).
- **Parameters:** none new (window 15 steps; V_nom = 400 V inherited). ** диапазон sanity:**
  compressed air ≈ most inefficient plant utility (~80% of compressor energy lost as heat;
  1 hp air motor ≈ 7–8 hp electric — use to sanity-check P_plant magnitudes, not as sim logic).
- **Unlocks:** energy-anomaly detection surface (idle-waste spike = starvation detector via CH9;
  E_unit drift = wear/derate detector via energy — orthogonal to signal-residual detectors);
  ISO 50001 EnPI reporting pattern for downstream consumers. **CPU:** +0% (rollups).
  **Gate:** ships with C2 (no separate battery — derived quantities; only their DETECTOR USE is
  gated: E_unit-drift detector arm in R1, same ΔF1 ≥3pp bar).

### X9 — Vibration → bearing-life consumption (L10/Palmgren-Miner scalar)
- **Cause → effect:** vib-RMS `V_m` (+ X7 lube) → life scalar `b_m ∈ [0,1]` → breakdown probability.
- **Equation:** `b_m(t+1) = b_m(t) + α_b·(V_m(t)/V_ref)^p·(1 + (1 − ℓ_m))·L_m(t)`,
  `p = 3` (rolling-bearing life exponent, standard L10 `L ∝ (C/P)^3`);
  breakdown hazard: `p_fail,m(t) = p_base + κ_b·max(0, b_m − b_warn)` (feeds existing BREAKDOWN
  fault sampler — no new fault machinery, just a load/condition-dependent rate).
  Same integrator shape as C3 wear (reviewers already accepted that pattern once).
- **Parameters:** `α_b` ≈ 1/600 per loaded step at V_ref (bearing outlives tool: slower than wear);
  `p = 3` (fixed, textbook); `b_warn = 0.7`; `κ_b` set so post-warn MTTF ≈ 30–60 steps.
- **Unlocks:** vibration-degradation → breakdown causal chain (the prognostic-labeled episodes
  §5 "episode splits" wants: pre-knee/post-knee analog = pre-warn/post-warn); life-stratified
  train/test splits. **CPU:** +1%. **Gate:** T8-style prognostic battery: post-warn episodes
  must show elevated breakdown rate (sanity, simulation check not ML) AND T1 regression check
  (no ≥5pp F1 drop from the added breakdowns shifting class balance).

---

## 2. Sensor-vs-process ambiguity matrix (H4 fault pairs — what each coupling buys)

| # | Sensor-side (observation-only, parity-VIOLATING) | Process-side (physical, parity-CONSISTENT) | Parity channel(s) | Coupling rows |
|---|---|---|---|---|
| P1 | Temp-sensor drift / bias (S-DRIFT; V flat, I flat) | Bearing degradation (V↑ + T↑ together) | vib-RMS + CH8 | X2 (+X6 as drift source) |
| P2 | Current-sensor bias (I↑, T flat, throughput flat) | Real overload/wear (I↑ + T↑ + F↑ + E_unit↑) | THERM + X3 force/E_unit | C1 + X1a + X3 + X8 |
| P3 | Pressure-sensor bias (P_air reads low, cycles normal) | Leak / demand surge (P_air low + cycles stretched) | cycle-time stretch | X5 |
| P4 | Humidity-driven temp bias (X6, slow, diurnal-shaped) | Thermal-derate slowdown (T↑ + throughput↓) | throughput + CH8 | X6 vs X1b |
| P5 | Dropout/stale-hold (SHF=DROPOUT, I12 pattern) | True steady operation (all parity channels live) | SHF + any live channel | spec §4 SHF (no new row) |

Design rule: **no sensor-side generator ships without its process-side partner in the same
battery** (X6-gate states this explicitly). Unpaired synthetic bias is the mechanism by which
over-modeled sims teach detectors ghost signatures (see §4 contradiction evidence).

---

## 3. Cheap-implementation patterns (why everything above stays CPU-trivial)

All rows use the same three primitives the literature converges on for cheap plant sims —
**never CFD/FEA** (FE spindle models, e.g. Zhao et al. 2007 full-structural FE, are the
explicit anti-pattern: accurate but orders of magnitude beyond the <600s budget):

1. **First-order lumped node** (C4 pattern): `x += (dt/τ)(x_ss − x)`. The standard surrogate for
   motor/spindle thermal behavior — LPTN literature (2nd–4th order grey-box identified networks:
   NREL/ORNL stator work) shows 2–3 nodes capture winding/lamination/housing dynamics; we use
   ONE node (C4 already decided) + algebraic I²R source (X1a). Stability unconditional at dt=1,
   τ≥5 (all our τ: 8–25).
2. **Algebraic derate factor** (X1b/X5 pattern): `L_eff = L·D(·)`. This is how NEMA/IEC derating,
   DOE compressed-air rules of thumb (2 psi ≈ 1%), and CAGI assessments express plant behavior —
   multiplicative factors, not ODEs. Zero RNG, zero state.
3. **Scalar integrator with knee/power-law** (C3/X7/X9 pattern): `s += α·load·f(s, obs)`.
   Archard (`Q ∝ W·d/H`), Taylor (`V·T^n = C`), L10 (`(C/P)^3`), Miner accumulation are all this
   shape in discrete time. Three such scalars per machine (w, ℓ, b) ≈ 96 floats for 32 machines.

Total new state: THERM T_m (C4) + WSTATE w_m (C3) + ℓ_m (X7) + b_m (X9) + P_air (C6) + H(t) (X6,
global) + start-counters (X4) ≈ 130 floats + existing. Total CPU delta admitted payload:
C1–C6 ≤12% (spec §8) + X-rows ≈ +4–6% → stays well inside <600s. Determinism: all new draws
from existing 36-stream SeedSequence (X3/X9 Bernoulli + X4 none + X5 leak uses `fail` stream
like other faults + X7 none) — **no new streams**, 0-diverge ×5 preserved.

---

## 4. Contradiction evidence (why every row is gated, not assumed)

1. **Lower fidelity can transfer better (Truong et al., CoRL 2023, "Rethinking Sim2Real").**
   Large-scale eval (Habitat/iGibson, 3 robots, real world): added fidelity did NOT help learning —
   slow sim prevented large-scale learning + policies overfit to sim-physics inaccuracies; simple
   real-data-grounded motion models generalized better. → Our reading: each coupling must earn its
   place via ablation (F7 rule already encodes this); sim speed (<600s) is a FEATURE for
   large-scale episode generation, and X-rows preserve it.
2. **Extra modeled moments can DEGRADE real-world detection (TII 2020, digital-twin Siamese AE
   for anomaly detection).** Twin simulated power/energy well at coarse windows but NOT switching
   transients; adding skewness/kurtosis features HURT classification on real data. → Direct
   precedent for X4's gate (inrush transients must prove they don't become FP sources, T5) and for
   keeping C5 impulse envelope-domain-only (F12).
3. **Domain randomization ≥ hand fidelity for bridging reality gaps (Tobin et al. 2017; Tremblay
   et al. 2018; Muratore et al. reviews; production case +15% from procedure MIX, not fidelity).**
   Non-photorealistic randomized sim beat hand-detailed sim; combinations of cheap procedures
   outperformed any single high-fidelity one. → Our FAULT_RANGES randomization is the DR analog;
   X-rows add MECHANISM DIVERSITY (new coupling procedures), not precision — the correct lesson
   is more cheap mechanisms + wider ranges, never finer physics per mechanism.
4. **ISP-AD / mixed-supervision lesson (J. Intell. Manuf. 2026): even a SMALL amount of real
   defect data beats pure synthetic; benchmarks on synthetic-only overestimate real performance.**
   → Spec §9 boundary #2 (no Sim2Real claim without ≥50 paired-real windows) already encodes this;
   X-rows must not be cited as transfer evidence, only as battery-scoped mechanism coverage.

Net: the literature's failure mode is always the same — **unvalidated fidelity presented as
progress**. The F7 ablation gate + §9 claim boundaries are the load-bearing walls; this file's
"gating" column exists to extend them to every new row.

---

## 5. Libraries verdict: hand-rolled per-step updates are correct — adopt nothing

| Candidate | Verdict | Reason |
|---|---|---|
| `scipy.integrate` (ODE solvers) | REJECT for replay path | In-step Euler on first-order nodes is analytically sufficient (τ≥5, dt=1, unconditionally stable); solver overhead + adaptive-step nondeterminism threaten 0-diverge ×5. Allowed ONLY offline (already spec'd: GES2N-style filters, PCMCI+/FCI analysis). |
| LPTN/motor-thermal libs, FEA/CFD tooling | REJECT | 2nd–4th order identified networks need sensor-calibrated R/C parameters we don't have (grey-box ID requires lab data); FE (Zhao-style spindle) is budget-incompatible by orders of magnitude. One-node C4 + X1a algebraic source captures the detection-relevant signature (lag + drift). |
| `scikit-friction`/tribology or bearing-life packages | REJECT | X9 is 3 lines (power-law accumulator); no package earns its dependency weight for `(V/V_ref)^3`. Same for Archard (C3 already hand-rolled, 1 line). |
| SimPy extensions / DES frameworks | NO CHANGE | Core stays SimPy DES (hard constraint); couplings are in-step algebraic updates, no framework interaction. |
| `numpy` only (already in replay path) | ADOPT (status quo) | Everything above is `numpy` scalar/vector ops + seeded `Generator`. No new dependency, no SciPy in replay, CPU budget holds. |

Justification in one line: every candidate library solves a HARDER problem (multi-node thermal ID,
distributed wear, waveform MCSA) than the twin needs (RMS-grade historian signatures at
1 sample/step) — adopting any of them spends determinism/CPU/dependency budget against zero
battery-measurable gain, which is exactly the over-modeling failure §4 documents.

---

## 6. Admission-gate summary (which battery proves each row — downstream issue checklist)

| Row | Default | Battery / gate | Metric bar | Notes |
|---|---|---|---|---|
| X1a I²R feedback | OFF | C4-probation arm (R1/R2) | ΔF1 ≥3pp or ΔAC@1 ≥10pp | folds into C4 flag (`THERMAL.i2r`) |
| X1b derate+trip | OFF | C4-probation arm + T4 flip | same + flip <40% | trip label = BREAKDOWN w/ ELEC code |
| X2 bearing T↔V | OFF | SENSOR_VS_PROCESS arm (new) | mode-accuracy +10pp | needs X6 partner in same battery |
| X3 wear→force→quality | OFF | T8 drift-subset | drift AC@1 ≥50%, +10pp, <10pp abrupt regression | ASM-scoped first |
| X4 inrush+sag | OFF | T5 precision-side | no worsening @ 20/150/5 budgets | transient-replay pool inclusion |
| X5 leak+slowdown | OFF | T2 cascade | AC@1 depth≤3 + ≥10pp ablation | new FAULT_MODES leak entry |
| X6 humidity bias | OFF | X2 pair battery ONLY | never standalone | ENV scalar, global |
| X7 lube state | OFF | T5 (bump) + T8 (neglect) | no T5 worsening + drift gain | MAINT_EVENT coupling |
| X8 idle/peak rollups | ON* | R1 detector-use arm | ΔF1 ≥3pp for E-feature detectors | *rollups free; only detector USE gated |
| X9 bearing-life | OFF | prognostic sanity + T1 no-regression | post-warn MTTF 30–60 steps; ΔF1 > −5pp | feeds BREAKDOWN sampler |

Config pattern (all rows): `COUPLING_Xn = {enabled: False, ...params}` in `src/config.py`
(default OFF; X8 rollups ON, detector-use flags OFF) — identical discipline to C4–C6 probation.

---

## 7. Sources consulted (10 queries, 2026-09-16)

Derating/standards: EN IEC 60034-30-1:2026 (IE classes, 25 °C rating basis); NEMA MG1 service-factor/
derating practice; IEC 60034-11 thermal protection; DOE motor sizing (~75% load) + compressed-air
rules (2 psi ≈ 1%, leaks 20–35%, air-motor 7–8× inefficiency); CAGI pressure-drop case study;
Vyas/ACEEE compressor+manufacturing co-simulation (receiver start/stop equations).
Bearing/thermal: SKF condition-monitoring sequence (envelope → vibration → temperature);
Beckhoff bearing-monitoring (BPFO/BPFI phenomenology — explicitly NOT modeled, §9);
ORNL/NREL LPTN grey-box stator identification (2nd/3rd-order sufficiency → our 1-node choice);
Zhao et al. 2007 spindle FE (anti-pattern reference); Coelho et al. 2025 open machine-tool
thermal dataset (surrogate-training direction, not adopted).
Wear/force/quality: Archard 1953 + Ståhl reformulation (geometry/force-aware wear function);
Taylor/Colding tool-life lineage; Zorev flank-force result (wear-dependent force nonlinearity);
Luo et al. 2005; ASME Appl. Mech. Rev. contemporary Archard review.
Current: ORNL/Kryter-Haynes MCSA (motor-as-transducer principle — the C1/X4 license);
Thomson MCSA tutorial (sideband/load-dependence caveats); MIT nonintrusive MCSA 2023.
Energy: ISO 50001:2018 + EnPI/EnB concepts; DOE EnPI regression-tool practice; NIST unit-process
energy methods.
Ambient: Abdullah et al. 2022 MOX T/RH cross-sensitivity regression correction; TI HDC3020/
SNA-A427 (−3 %RH per +1 °C local heating rule of thumb, drift-correction practice); Vaisala
HMP110 drift statistics (±0.5 %RH/yr, ±0.02 °C/yr fair estimates).
Cheap-sim patterns: NIST lumped-capacitance path-level AM framework (CAPL — precedent for
lumped-capacitance-at-scale); Jaffer switched-capacitor thermal stepping (stability intuition).
Contradictions: Truong et al. CoRL 2023 lower-fidelity-transfers-better; TII-2020 Siamese-AE
twin paper (higher moments degrade real-world classification); Tobin/Tremblay/Muratore DR lineage;
ISP-AD 2026 mixed-supervision lesson.

*End — 10 coupling rows (X1a–X9 + X8 rollup), 5-pair ambiguity matrix, cheap-pattern rationale,
4 contradiction cases, library verdict (adopt nothing), per-row admission gates.*
