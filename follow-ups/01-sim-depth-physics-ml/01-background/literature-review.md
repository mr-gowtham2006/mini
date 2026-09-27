# Literature review (canonical merged) — sim-depth-physics-ml (follow-up 01)

Date: 2026-09-13 · Scope: 32-machine SimPy plant twin upgrade (T=300, CAL_WIN=120, CPU <600s wall, 0-diverge seeded replay, SciPy allowed).
Method: TEXT-ONLY merge of `review-academic.md` + `review-industry.md`. No sources invented — every claim below traces to a row in `source-table.md`. No images/PDFs/diagrams opened.

Flow: 176 retrieved → 176 screened → ~36 eligible → ~36 unique sources included (academic 80 → 80 → 16; industry 96 → 96 → ~20 unique after cross-query dedupe).

## 1. Search strategy (PRISMA-S Item 8, per angle, verbatim)

### 1a. Academic angle (peer-reviewed only; 10 queries)

| QID | Verbatim query | Covers | Retrieved → screened → kept |
|---|---|---|---|
| Q1 | `lumped thermal model machine tool manufacturing simulation fidelity` | (a) thermal | 8 → 4 → 1 (A-S9) |
| Q2 | `motor drive dynamics spindle current power digital twin manufacturing simulation` | (a) drive/current | 8 → 5 → 1 (A-S11) |
| Q3 | `tool wear model Archard Taylor manufacturing simulation degradation propagation` | (a) wear | 8 → 4 → 1 (A-S6) |
| Q4 | `multi-sensor sampling rate design predictive maintenance machine learning manufacturing` | (b) channels/sampling | 8 → 4 → 2 (A-S10, A-S12) |
| Q5 | `cascading anomaly detection root cause manufacturing multi-origin fault propagation` | (c) cascades/traceback | 8 → 5 → 1 (A-S13) |
| Q6 | `sensor fault versus process fault disambiguation manufacturing diagnosis` | (c) sensor-vs-process | 8 → 4 → 2 (A-S8a, A-S8b) |
| Q7 | `discrete event continuous co-simulation manufacturing CPU efficient deterministic SimPy` | (d) DES+physics cheap | 8 → 4 → 2 (A-S14, A-S15) |
| Q8 (CONTRA) | `high-fidelity digital twin failed improve anomaly detection negative result manufacturing` | contradiction-seeking | 8 → 3 → 2 (A-S5, plus A-S4 §VII note) |
| Q9 (CONTRA) | `causal discovery fails time series manufacturing prior knowledge wrong PCMCI limitation` | contradiction-seeking | 8 → 4 → 2 (A-S7, A-S1-limits) |
| Q10 | `bearing vibration model spectral kurtosis envelope analysis fault frequency rotating machinery review` | (a) vibration realism | 8 → 4 → 1 (A-S2) |

Level rubric: L1 systematic review/foundational · L2 journal · L3 conference · L4 preprint · L5 official docs. Academic: 16 rows, 10 at L1–L2. Evidence-grade limits: MDPI full texts 403-blocked (3 attempts, `snippet-only`); A-S8b landing-page only (PDF never opened); A-S12/A-S13 2026 preprints (unrefereed).

### 1b. Industry angle (official docs/standards/practitioner/vendor-tech; peer-reviewed theory excluded)

| # | Verbatim query | Angle | Retrieved → screened → kept |
|---|---|---|---|
| Q1 | factory historian sampling rates vibration temperature motor current OSIsoft PI AVEVA Siemens channels logged | (a) historian channels/rates | 8 → 8 → 5 |
| Q2 | Siemens Insights Hub AVEVA digital twin production physics modeling capabilities limitations | (b) twin platforms | 8 → 8 → 4 |
| Q3 | PTC ThingWorx GE Vernova digital twin manufacturing production deployment machine monitoring | (b) twin platforms | 8 → 8 → 3 |
| Q4 | OPC UA companion specification machinery monitoring sampling rate ISA-95 IEC 62264 historian MES | (a)+(standards) | 8 → 8 → 3 |
| Q5 | ML anomaly prediction factory training data labeling class imbalance windowing LSTM transformer vibration | (c) ML data design | 8 → 8 → 4 |
| Q6 | sim-to-real transfer manufacturing synthetic data lessons learned factory simulation | (c) Sim2Real | 8 → 8 → 4 |
| Q7 | alarm flood management EEMUA 191 cause effect matrix operator root cause cascade manufacturing | (d) RCA practice | 8 → 8 → 4 |
| Q8 | PyRCA root cause analysis production deployment topology causal graph microservice factory | (d) RCA tooling | 8 → 8 → 3 |
| Q9 | ISO 13374 MIMOSA OSA-CBM condition monitoring data processing blocks vibration standard | (a)+(standards) | 8 → 8 → 3 |
| Q10 | motor current energy meter compressed air pressure OEE downtime codes factory data collection practice | (a) missing channels | 8 → 8 → 5 |
| Q11 (contradiction) | digital twin ROI failure manufacturing disappointing results why twins fail plant | contradiction | 8 → 8 → 5 |
| Q12 (contradiction) | predictive maintenance false positives plant alert fatigue failed deployment lessons | contradiction | 8 → 8 → 5 |

Levels: L3 official docs/standards · L4 practitioner/vendor-technical/engineering · L5 opinion/blog. Industry: ~20 unique sources after cross-query dedupe. Paywalled standards (ISO 13374, EEMUA 191) rest on publisher landing pages + secondary guides, marked accordingly; MathWorks page 403 → snippet-only (I11); vendor outcome numbers (I09/I10) marketing-grade, never used for effect sizes.

### Inclusion / exclusion criteria (both angles)

Include: (1) factory/manufacturing sensing, twin physics, ML-trainability, cascade/RCA relevance to the 32-machine twin; (2) retrievable as text (HTML/markdown/docs/abstract/snippet); (3) dated or versioned. Exclude: (a) image/PDF-only evidence that cannot be converted to text (reason: TEXT-ONLY constraint — e.g. A-S8b PDF kept as landing-page provenance only); (b) peer-reviewed theory in the industry angle and vendor marketing in the academic angle (reason: lane separation, avoid double-counting); (c) waveform-level bearing-physics claims for the twin's 1/step channels (reason: out of scope — twin claims RMS-grade only). No language restriction applied; all included sources are English.

## 2. Thematic synthesis (across BOTH angles)

### S1 — Cheap-fidelity consensus: lumped first-order physics, not FE (academic T1+T8 × industry T3)
Both angles converge. Academic: FE→MOR→lumped ladder, lumped thermal networks as the accepted cheap tier (A-S9); ~78% thermal-compute reduction at 0.38% model order in screened ROM literature; DES+continuous co-simulation stays cheap and deterministic under the event-stepped pattern (A-S14) with SimPy's heap-queue FIFO replay (A-S15). Industry: no vendor ships per-machine transient multi-physics at production scale — Siemens Factory Twin is Plant Simulation DES + IIoT data (I06), deep physics lives in Simcenter Executable Twin as ROM compressed offline and never run full-fidelity online (I07); AVEVA is historian + AF models + Event Frames (I05), PTC is connectivity + OEE dashboards (I09), GE is statistical/ML process models (I10). Joint prescription: per-machine first-order lags (thermal τ, wear scalar, current from drive ODE) inside the SimPy step loop — O(1) per machine-step, zero architecture risk to T=300/<600s/0-diverge.

### S2 — Current/power are the cheapest high-value new channels (academic T2 × industry T2/G1-G2)
Both angles name motor current first. Academic: spindle/drive DTs model FOC→current/torque→force→vibration as coupled ODEs and use the twin to generate ML datasets (A-S11); DT-simulated normal + few real anomalies trains a Siamese AE beating SOTA unsupervised AD at FPR ≈9%/FNR ≈12% (A-S4). Industry: clip-on current is the default legacy retrofit (I19), current-intervals feed OEE (I18), energy-per-unit from master meters + motor currents is the ISO 50001 standard calc, and idle-but-powered draw is the exact signal the twin's DOWN/BLOCKED/STARVED offsets cannot produce (I16). Joint prescription: CH8 current (`I_idle + k·load + faultdev`, idle>0) as P0, CH9 energy derived by integration as P0, header-level air pressure CH10 as P1 second shared-resource contention.

### S3 — Wear is two-phase and CPU-cheap; horizons need episode chaining (academic T3 × industry T4/G7)
Academic: wear ≈0 until initiation then rapid nonlinear growth; stroke-dependent-coefficient modified Archard reproduces the knee with NO geometry updates (A-S6) — one scalar ODE per machine. Industry: normal-only/one-class training is the norm with pre-/post-maintenance window labeling (I11); production horizons are days (14-day window, I12), so T=300 episodes are micro-windows for prognostics; SMOTE-family oversampling on 50%-overlap FFT windows (I13) with resampling INSIDE CV folds (I15). Joint prescription: cumulative wear scalar + knee → drift/current coupling (the "slow subtle" mechanism), plus MAINT_EVENT reset flags and episode-seeded splits for the ML dataset export.

### S4 — Vibration: impulsive content for diagnosis, RMS-grade for historians (academic T4+T5 × industry T1/G6)
Agreement on facts, split on implication (see contradiction R2). Academic: bearing faults are quasi-periodic impulses via SK band selection + envelope at characteristic frequencies (A-S2); twin sine+AR1 learns the wrong signature; multi-scale 0.5/1.0/2.0 s windows catch transients + wear (A-S10); full-bandwidth use beats downsampled baselines by ≥8pp (A-S12). Industry: plants burst-collect ≥10k values/s waveforms for seconds, FFT, historize only RMS/envelope/bands; temperature on RoC deadbands (I03); PI scan classes 1 s/5 s/20 s/10 min (I04); OPC UA Machine Tools caps MES/analytics at 1 Hz (I08). Joint prescription: keep 1/step obs as documented RMS-grade (claim boundary: no bearing-frequency resolution), add wear-coupled impulse term + multi-scale window export, never downsample fast channels.

### S5 — Cascades: multi-root effect-mode formalism × alarm-flood operations (academic T6+T7 × industry T5)
Academic: MATERO-RCA frames multi-root multi-alarm with observation-only vs physical-propagation modes via counterfactual trajectory compatibility (A-S13); sensor-vs-process is solved-structure (parity-space/bank-of-observers + reconstruction contributions, A-S8a/A-S8b) — twin spike/drift/bias are literally observation-only by construction while delay/breakdown propagate physically. Industry: operators do NOT walk graphs — EEMUA 191 targets ≈1 alarm/10 min, floods (10+/10 min) contributed to TMI/Bhopal-class incidents (I23/I24); flood alarms ARE causally related along flow from one root (I25); PyRCA's topology-mask + walk + domain-constraint pattern (ε-diagnosis + Bayesian/random-walk scoring, PC with forbidden/required links) structurally mirrors our veto-mask + depth-≤3 walk (I28). Joint prescription: effect-mode tags per fault class, redundant channels enabling parity checks, multi-root alarm labelling; flood-grouping narration deferred to the narration layer.

### S6 — Sim2Real and trust curves bound every claim (academic A-S4/A-S12/A-S13 limits × industry T4/C1-C3)
Academic: 1/min sampling + 2.18% energy discrepancy (A-S4); preprints unrefereed (A-S12/A-S13); snippet-only rows. Industry: synthetic-only collapses without paired-real data (0.2516→0.8853 mAP with 50 paired-real + 500 synthetic, I14); domain randomization beats photorealism (I14b); transfer is a 4-stage pipeline (I14c); 83–99% assembly transfer only with careful real-system design (I14d). Joint rule: keep FAULT_RANGES domain randomization, document transfer-claim boundary (no Sim2Real claim without paired-real validation), ship every new channel with Q_DET calibration + noise spec.

## 3. Gaps & Contradictions (nothing suppressed; Phase-2 hypotheses seed from these, not absences)

G1. **Fidelity ≠ detection (A-S5 + C1).** Lower-fidelity DTs matched high/ultra-high fidelity on 2 detection tasks across 6 fidelity attributes with cost/time wins (A-S5); twins failed modeling assets instead of decision/data/incentive webs (C01), accurate twins get ignored without decision authority (C02), 75% stall past pilot / ~15% escape (C03/C04). Every new physics term must pass a detection/traceback ablation or it is wasted CPU. → contradiction-map R1.
G2. **Half-modelled impulses can HURT (A-S4 §VII + industry 1 Hz rule).** Adding skew/kurtosis degraded detection because short timescales were missed by the DT; industry historicz RMS-grade at ≤1 Hz and never claims bearing frequencies. Impulses must clear envelope-domain acceptance tests, not raw-waveform matching. → R2.
G3. **PCMCI+ weakest exactly in wear-drift regimes (A-S7 + A-S1, compounding parent F4).** PCMCI "not suitable for highly predictable systems with minimal new info per step" — adapted FCI + Jaccard-vs-reference-graph instead; Runge assumes sufficiency + per-segment stationarity. Wear-driven slow drift is precisely that regime: the causal layer needs a graph-change-distance complement. → R4.
G4. **Alert fatigue vs sensitivity (C2 + A-S4 rates).** "85% accurate" models get muted; zero-value-as-fault FP spikes, start-up/changeover filtering required (C05/C06/I12). Twin 4–7σ fault mags may be sensitivity-tuned in a way plants reject; A-S4's 9% FPR/12% FNR is the realistic ceiling reference. Needs a precision-side lens (alerts/1000 healthy windows). → R3.
G5. **Physics cheap to run, expensive to trust (A-S14/A-S15 vs C3).** In-step ODEs are O(1) and replay-safe, but twin value is back-loaded: first months are pure integration/validation cost (C09), vendor 18-month paybacks don't survive year-2 accounting (C10), each channel needs calibration + threshold re-tuning. → R5.
G6. **Generic models fail; vendor numbers unaudited (C4 + C5 × A-S3).** Failure mode is generic modeling + CMMS-disconnected alerts (C06b/C06c); priors help only when correct and adoption barriers persist (A-S3, reinforcing parent F4 mask-conditionality). GE/PTC outcome numbers (waste −75%, OEE +10%) are marketing-grade, usable for architecture patterns only. → R6.

## 4. Parent-findings consistency (F1–F3 Locked; tension flagged, none created)

- F1 (raw point-wise F1 only): no tension. This review makes no detector ranking; industry windowing/imbalance guidance (I13/I15) explicitly requires in-pipeline resampling and episode-seeded splits, consistent with ungameable evaluation. M0b battery still owed per F5.
- F2 (no platform framing): no tension — reinforced. C1 (C01–C04) independently corroborates the Locked platform kill; the upgrade spec must weight trace-explainability ≥ physics depth (F6 wedge is a decision/authority interface, not more physics).
- F3 (0/14 joint T+P+R): no tension — reinforced. PyRCA covers ≤2 legs with a structurally convergent topology+walk pattern (I28), i.e. independent validation from the opposite (microservice) direction, not a full-row hit.
- F4 (masked PCMCI+ conditional on mask correctness): no tension — reinforced and pressured as documented. A-S3 corroborates prior-conditionality; A-S7/A-S1 pressure the wear-drift regime (G3/R4) without overturning the posture.
- No new claim in this review contradicts Locked F1–F3. Numeric citations carried over (A-S4 9%/12%, ROM 78%/0.38%, FISHER ≥8pp, RF-DETR 0.2516→0.8853, GE percentages) keep their original evidence grades and are not upgraded.

## 5. Handoff to 02-hypotheses (candidate seeds, ≤5 to be selected there)

H-seed-a (from G1): each added physics term (thermal lag, impulses, wear) changes AC@1/detection-F1 by a measurable margin vs ablated twin, else removed. H-seed-b (from G2): wear-coupled impulses improve envelope-domain features without degrading skew/kurtosis-conditioned detection. H-seed-c (from G3): PCMCI+ alone misses wear-degradation chains that a Jaccard-style graph-distance complement catches. H-seed-d (from G4): a precision-side bar (alerts/1000 healthy windows) demotes at least one sensitivity-tuned detector setting plants would mute. H-seed-e (from G5): each new channel ships calibration + noise spec; without it, thresholds from the old 7-channel twin mis-fire on the new channels.
