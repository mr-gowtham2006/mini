# Academic literature review — sim-depth-physics-ml (follow-up 01)

Date: 2026-09-13 · Scope: peer-reviewed evidence only (industry sources live in `review-industry.md`).
Twin under review: 32-machine SimPy plant twin, T=300 steps, CAL_WIN=120, CPU <600s wall, 0-diverge seeded replay, SciPy allowed.
Method note: TEXT ONLY pass — no image/PDF/diagram opened; retrieval via academic search + HTML/markdown fetch only. 8 content fetches + 3 blocked attempts (MDPI 403), well under the 25-fetch cap.

## 1. Search strategy (verbatim log)

| QID | Verbatim query | Covers | Retrieved → screened → kept |
|---|---|---|---|
| Q1 | `lumped thermal model machine tool manufacturing simulation fidelity` | (a) thermal | 8 → 4 → 1 (A-S9) |
| Q2 | `motor drive dynamics spindle current power digital twin manufacturing simulation` | (a) drive/current | 8 → 5 → 1 (A-S11) |
| Q3 | `tool wear model Archard Taylor manufacturing simulation degradation propagation` | (a) wear | 8 → 4 → 1 (A-S6) |
| Q4 | `multi-sensor sampling rate design predictive maintenance machine learning manufacturing` | (b) channels/sampling | 8 → 4 → 2 (A-S10, A-S12) |
| Q5 | `cascading anomaly detection root cause manufacturing multi-origin fault propagation` | (c) cascades/traceback | 8 → 5 → 1 (A-S13) |
| Q6 | `sensor fault versus process fault disambiguation manufacturing diagnosis` | (c) sensor-vs-process | 8 → 4 → 2 (A-S8a, A-S8b) |
| Q7 | `discrete event continuous co-simulation manufacturing CPU efficient deterministic SimPy` | (d) DES+physics cheap | 8 → 4 → 2 (A-S14, A-S15) |
| Q8 (CONTRA) | `high-fidelity digital twin failed improve anomaly detection negative result manufacturing` | contradiction-seeking | 8 → 3 → 2 (A-S5, plus A-S4 skew/kurtosis note) |
| Q9 (CONTRA) | `causal discovery fails time series manufacturing prior knowledge wrong PCMCI limitation` | contradiction-seeking | 8 → 4 → 2 (A-S7, A-S1-limits) |
| Q10 | `bearing vibration model spectral kurtosis envelope analysis fault frequency rotating machinery review` | (a) vibration realism | 8 → 4 → 1 (A-S2) |

Level rubric: L1 systematic review/foundational method · L2 peer-reviewed journal study · L3 peer-reviewed conference · L4 preprint · L5 official docs/grey. 16 sources, 10 at L1–L2.

## 2. Thematic synthesis

### T1 — Lumped-parameter thermal dynamics are the accepted cheap-fidelity tier (A-S9, Q1-screened ROM literature)
Consensus: HIGH. Machine-tool thermal literature converges on a fidelity ladder: full FE (accurate, slow) → model-order-reduced structural models (fast, high modelling effort) → lumped-parameter thermal networks / hybrid characteristic-diagram models (cheap, adequate for error compensation). The hybrid line (simulation-generated characteristic diagrams + structural decision algorithm, Appl. Sci. 2024, A-S9) exists precisely because neither pure-FE (slow) nor pure-empirical (load-blind) suffices. Screened ROM papers report ~78% thermal-field compute reduction at 0.38% model order with preserved accuracy — i.e. first-order lumped dynamics buy most of the realism per CPU.
Twin-gap mapping: twin temp is currently a uniform per-class band with NO inertia (config.py TEMP_RANGES, SIM_SPEC §8 ch.2). A per-machine first-order lag `T(t+1) = T(t) + (T_ss(load) − T(t))·dt/τ` with class τ is the literature-backed minimum upgrade; full FE/MOR is overkill per A-S5 (below).

### T2 — Drive electrical + mechanical dynamics belong in the twin's synthetic-data path (A-S11, A-S4)
Consensus: MODERATE-HIGH. Spindle/drive DT literature models the chain field-oriented control → armature current/torque (PID-regulated) → cutting forces → tool-tip vibration as coupled second-order ODEs, and uses the twin explicitly to generate ML training datasets with injected bearing-disturbance perspectives (SPEEDAM 2024, A-S11; Processes 2025 real-time-simulator synthetic-data paper, Q2-screened). Castellani et al. IEEE TII (A-S4, fetched full text) validate the pipeline end-to-end: DT-simulated normal-operation data + a handful of real labeled anomalies trains a Siamese autoencoder that beats SOTA unsupervised methods, with best FPR ≈9% / FNR ≈12%.
Twin-gap mapping: twin has NO motor current/power/pressure/flow channels — only `obs`+temp+throughput+flags+states (SIM_SPEC §8). A-S11/A-S4 jointly justify adding current/power as first-class channels: they are the cheapest physics-grounded features for later predictive models, generatable from drive ODEs without FE cost.

### T3 — Wear is two-phase (flat → rapid post-initiation) and modellable without geometry updates (A-S6)
Consensus: HIGH for the phenomenology, MODERATE for the cheap trick. Bang et al. (Materials 2022, A-S6, fetched): measured punch wear depth ≈0 (within measurement error) for thousands of strokes, then rapid nonlinear growth after failure initiation (e.g. CrN punch failed at 16.5k strokes, AlTiCrN at 59k). Conventional Archard is linear in strokes and wrong; a stroke-dependent wear coefficient (modified Archard with scale factor encoding wear-property change vs depth) reproduces the knee WITHOUT per-iteration geometry updates — i.e. CPU-cheap. Calibration is coating/shape-specific (1.4–3.7% deviation after 15M strokes in the GCU stamping validation, Q3-screened).
Twin-gap mapping: twin has NO tool-wear state at all; all 7 fault classes are rectangular/ramp windows (config FAULT_RANGES). Literature prescribes a cumulative wear state variable per machine with a knee function, feeding drift-magnitude and current-draw growth — this is the mechanism for "slow subtle" anomalies the user asked for, and it falls out of one scalar ODE per machine.

### T4 — Vibration realism means impulsive fault-frequency content, not sine+AR1 (A-S2)
Consensus: HIGH. The rotating-machinery diagnostics literature is unanimous: bearing/gear faults manifest as quasi-periodic impulses demodulated via spectral kurtosis band selection + envelope analysis at bearing characteristic frequencies (Antoni & Randall SK review, A-S2; 2024 condition-monitoring review + CWRU kurtogram study, Q10-screened). Envelope-spectrum analysis beats raw vibration analysis specifically for early-stage faults.
Twin-gap mapping: twin `clean = base + 0.5σ·sin + AR1(0.6)` (SIM_SPEC §4.1) contains no impulsive component and no fault-frequency structure — a detector trained on it learns the wrong signature (same audit logic as the killed 0.45^lag echo, SIM_SPEC §4.2). Minimum upgrade: add an impulsive shot-noise term whose rate/amplitude couples to the wear state (T3), keeping everything else identical.

### T5 — Multi-rate, multi-window sensing is the ML-trainability design pattern (A-S10, A-S12)
Consensus: MODERATE-HIGH. Two independent lines agree: (i) the 2026 TCN predictive-maintenance system (A-S10) segments every window at 0.5/1.0/2.0 s scales across vibration + smcAC/smcDC current + AE channels, with low-pass denoise + per-channel detrend — short windows catch transients, long windows catch wear progression; (ii) FISHER (arXiv 2026, A-S12, fetched): real SCADA sampling rates sit just above 2× bandwidth and vary widely; fixed-duration STFT with sub-band modelling handles arbitrary rates, and fault-diagnosis accuracy comes from using FULL bandwidth (downsampling to 16 kHz destroys it — all baselines lose ≥8pp to FISHER-tiny on diagnosis).
Twin-gap mapping: twin samples everything at 1/step with no sub-step structure (SIM_SPEC §8). Prescription: keep 1/step as the base tick but (a) add fast channels (current/vibration-envelope) computed at sub-step resolution then aggregated, (b) emit multi-scale window features (short/long) in the ML dataset export, (c) never downsample vibration/current channels for PCMCI/ML (mirrors SIM_SPEC §10's "never channels 6–7" rule, extended to new fast channels).

### T6 — Cascading RCA needs multi-root, multi-alarm framing with explicit root-effect modes (A-S13)
Consensus: MODERATE (young literature, convergent direction). MATERO-RCA (arXiv 2026, A-S13, fetched): industrial RCA is hard for exactly the twin's reasons — (i) unknown root-effect modes (observation-only vs physical-propagation), (ii) multi-root multi-alarm events, (iii) implicit temporal dynamics that resist explicit SCMs. Evaluates root-set hypotheses by counterfactual trajectory compatibility, not per-node scores. Complements the two-tier causal network + C-GRU propagation-path line (IEEE ETAE 2025, Q5-screened) and graph-constrained back-tracing (GBTPL, Q5-screened).
Twin-gap mapping: twin fault windows never overlap on one machine but MAY overlap across machines (SIM_SPEC §4.3) — i.e. multi-origin overlap is already generatable but unexploited. A-S13's observation-only vs physical-propagation distinction is the exact formalism for the sensor-vs-process split (T7): spike/bias/drift faultdev is observation-only; delay/loss/breakdown/quality propagate physically via WIP/buffers.

### T7 — Sensor-vs-process disambiguation is a solved-structure problem (A-S8a, A-S8b)
Consensus: HIGH on problem structure, MODERATE on method choice. Definition (Sensors 2021, A-S8a): process fault = process deviated, sensor truthful; sensor fault = process normal, reading deviated; non-control-loop sensor faults corrupt operator/diagnostic judgment without physical effect. Method family: parity-space/bank-of-observers residuals + reconstruction-based contribution plots on DKPCA (A-S8a, Tennessee Eastman validation); redundancy + fault-signature analysis (Taiebat 2017, A-S8b; Hertfordshire sensor-FDD review, Q6-screened — "the most significant challenge in sensor FDD").
Twin-gap mapping: twin's spike/drift/bias inject `faultdev` at origin obs ONLY — these are literally sensor-side effects with no physical propagation by construction (SIM_SPEC §4.1), while delay/breakdown propagate via flow. The twin therefore already implements the A-S13/A-S8 mode split structurally; what is missing is (a) a labelled sensor-vs-process fault taxonomy in §5, (b) redundant/corroborating channels (current vs vibration vs throughput) that make parity checks possible — currently a single `obs` channel per machine cannot support disambiguation at all.

### T8 — DES + continuous physics co-simulation stays cheap and deterministic under the event-stepped pattern (A-S14, A-S15)
Consensus: HIGH. The mixed discrete-continuous SimPy framework (arXiv:2208.01408, A-S14): continuous trajectories advance under the event-stepped engine; threshold crossings raise events, external events mutate continuous state — exactly the pattern for per-step lumped ODEs inside a SimPy process. SimPy's own scheduling docs (A-S15, fetched): single-threaded, heap-queue, FIFO-per-timestamp deterministic replay ("always the same results" without unseeded `random`) — the documented basis for the twin's 0-diverge seeded-replay gate (SIM_SPEC §6/§9.4, 36-stream SeedSequence). Cost evidence: SerializableSimpy/JIT + GPU-TensorFlow SimPy acceleration lines (Q7-screened, 1.4–3.2× speedups) show headroom if needed, but per-step scalar ODEs (T1/T3/T4 terms) are O(1) per machine-step and need no such machinery.
Twin-gap mapping: licenses adding lumped thermal + wear + impulsive-vibration ODEs INSIDE the SimPy step loop with zero architectural risk to T=300 / <600s / 0-diverge.

## 3. Consensus levels

| Theme | Consensus | Basis |
|---|---|---|
| Lumped thermal as cheap-fidelity tier | HIGH | Convergent FE→MOR→LPTN ladder, replicated across groups |
| Drive current/power channels for ML twins | MODERATE-HIGH | IEEE TII validation + spindle-DT line; single-plant scope caps it |
| Two-phase wear + no-geometry-update Archard | HIGH (phenomenology) / MODERATE (trick) | Direct measurement + FE validation; coating-specific calibration |
| Impulsive/envelope vibration content | HIGH | Review-level + CWRU replication |
| Multi-rate, multi-window sensing | MODERATE-HIGH | Deployed TCN system + 19-dataset benchmark; preprint half |
| Multi-root RCA with effect modes | MODERATE | Convergent direction, heavyweight methods, preprint core |
| Sensor-vs-process structure | HIGH (structure) / MODERATE (method) | Textbook parity theory + TE-process validation |
| DES+continuous cheap + deterministic | HIGH | Framework paper + official scheduler docs |

## 4. Mapping: every theme → current twin gap

| Twin gap (CONTEXT/SIM_SPEC/config.py) | Theme(s) | Literature prescription |
|---|---|---|
| sine/AR1 vibration, no impulses | T4 (A-S2) | shot-noise impulse term coupled to wear state; envelope-domain features |
| uniform temp, no inertia | T1 (A-S9) | per-machine first-order lag, class τ, load-driven setpoint |
| missing current/power/pressure channels | T2 (A-S11, A-S4) | drive ODE → current/power channels at sub-step aggregation |
| missing tool wear | T3 (A-S6) | cumulative wear scalar + knee function → drift/current coupling |
| 1/step single-scale sampling | T5 (A-S10, A-S12) | fast-channel sub-step aggregation + multi-scale window export; never downsample fast channels |
| cross-machine overlap unexploited | T6 (A-S13) | multi-root alarm labelling; effect-mode tags per fault class |
| sensor-vs-process untestable (single obs) | T7 (A-S8a/b) | redundant channels enabling parity checks; labelled fault-mode taxonomy |
| CPU/determinism risk of added physics | T8 (A-S14, A-S15) + CONTRA A-S5 | O(1) lumped ODEs in-step; fidelity only where detection-relevant |

## 5. Gaps & Contradictions (nothing suppressed)

1. **Fidelity ≠ detection (CONTRA, A-S5).** Su et al., J. Intell. Manuf. 2024: across six fidelity attributes (resolution, mesh, shadows, illumination, light, threshold), LOWER-fidelity DTs matched high/ultra-high fidelity on two extrusion-failure detection tasks, with wins on config cost, storage, sim time and detection time. Directly pressures "as deep as possible": every added physics term must earn its place against a detection/traceback ablation. Twin consequence: the upgrade spec must pair each new term (thermal lag, impulses, wear) with an M0b-style battery check, or A-S5 predicts wasted CPU.
2. **High-order signal moments can HURT when the twin misses short timescales (A-S4, §VII).** Castellani's grid search: adding skewness/kurtosis DEGRADED detection because "short time-scales, switching and state transitions are not captured well by the DT simulation." Warning for T4/T5: impulsive content must be modelled well enough that its statistics help — half-modelled impulses could repeat this failure. Mitigation: calibrate impulse statistics against envelope-domain acceptance tests, not raw-waveform matching.
3. **PCMCI-family limits in gradual-degradation regimes (CONTRA, A-S7 + A-S1).** Choudhary et al. (Sensors 2024, fetched): PCMCI "not suitable for highly predictable systems with minimal new information at each time step" and gradual degradation "may not be substantial" step-to-step — they chose adapted FCI instead, tracking Jaccard distance of causal graphs vs a new-component reference. Runge (A-S1) assumes causal sufficiency + stationarity per segment. Twin consequence, compounding parent F4's prior-fragility verdict: wear-driven slow drift (T3) is precisely the regime where PCMCI+ is weakest — the causal layer needs a graph-change-distance complement (Jaccard-style) for degradation chains, not just lagged discovery.
4. **Prior-knowledge integration is double-edged (A-S3, reinforces parent F4).** The manufacturing causal-discovery review (JMMP 2022): priors improve accuracy AND serve as expert contact point, but adoption barriers persist. No new overturn; treat as corroborating F4's "conditional on mask correctness."
5. **Evidence-grade limits.** MDPI full texts were 403-blocked (3 attempts); A-S2/A-S8–A-S11 rest on search-result snippets + abstracts — flagged `snippet-only` in the source table. A-S12/A-S13 are 2026 preprints (unrefereed). Thermal quantitative claims lean on one fetched-abstract hybrid paper plus screened ROM snippets. The 02-hypotheses step should convert rows 1–3 above into falsifiable hypotheses rather than treating any prescription as established.
