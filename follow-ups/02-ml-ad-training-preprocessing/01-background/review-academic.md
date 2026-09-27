# Academic systematic review — ML AD training + preprocessing for the Verdandi twin (follow-up 02)

Date: 2026-09-13 · Scope: PEER-REVIEWED evidence only (industry-vendor lane excluded — teammate owns it).
Twin context: 32-machine SimPy twin, T=300, 1 sample/step, 7→15 channels, FREE sim labels, CPU-only.
Parent locks honored: F1 raw point-wise F1 only, PA-off, fixed-percentile calibration (Finding F1);
M0b battery (quantile vs GDN-light vs MP-discord); contested bars flagged, never endorsed (Finding F5).

## 0. Retrieval log (verbatim queries, retrieved → screened → kept)

All searches via academic search (Exa primary). 11 distinct queries, ~86 snippets retrieved.

| # | Mandate slot | Verbatim query | Retr. | Screened → kept |
|---|---|---|---|---|
| 1 | (a) architectures | `deep time-series anomaly detection USAD TranAD Anomaly-Transformer GDN MTAD-GAT compared SWaT WADI SMD benchmark` | 8 | TranAD VLDB22 (full text), MTAD-GAT orig. (snippet), TiTAD replication note (snippet) |
| 2 | (a+f) PA critique | `Kim et al point adjustment invalid anomaly detection time series random scores SOTA critique` | 8 | Kim AAAI22 (full text), PA-manipulability proof 2308.13068 (snippet), adversarial stress-test 2607.11969 (snippet) |
| 3 | (b) one-class vs supervised | `one-class normal-only training vs supervised synthetic anomalies time series anomaly detection` | 8 | Lau et al. 2506.13955 (full text), RoCA/contamination (snippet, excluded — off-twin: twin labels are clean), HAI+SGAN study (snippet) |
| 4 | (c) preprocessing | `preprocessing industrial time series normalization per-sensor detrending deseasonalizing missing data imputation operating regime conditioning` | 8 | Thibault framework MDPI (snippet, 403 on fetch), Fleck 2109.03469 (full text), EUSIPCO z-score note (snippet) |
| 5 | (d) windows/features | `sliding window length selection FFT wavelet envelope features multivariate time series anomaly detection manufacturing` | 8 | FFT multi-window USAD Energies25 (snippet), Dual-TF WWW24 (snippet), AALTD22 window-size (snippet), PHM FFT+envelope+kNN (snippet) |
| 6 | (e) thresholding | `anomaly threshold calibration extreme value theory POT M2AD GMM Gamma rarity score time series` | 8 | M2AD AISTATS25 (PDF→text p1–6), SPOT KDD17 (PDF→text p1–4), γGMM contamination (snippet) |
| 7 | (f) evaluation | `time series anomaly detection evaluation point-wise event-wise affiliation metric delay-aware Schmidl benchmark` | 8 | Schmidl VLDB22 (PDF→text p1–3), Affiliation KDD22 (PDF→text p1–4), Elephant NeurIPS24 (PDF→text p1–4), TimeEval (snippet) |
| 8 | CONTRA-1 deep-vs-classical | `KNN beats deep learning time series anomaly detection reversal simple baselines MTAD benchmark` | 8 | MTAD bench 2401.06175 (full text), OLS-beats-SOTA (snippet, OpenReview blocked), Sign-of-the-Times thesis (snippet), Do-DNN-contribute PR22 (snippet) |
| 9 | CONTRA-2 transformer/synthetic skepticism | `transformer skepticism time series anomaly detection synthetic anomalies hurt generalization shortcomings` | 8 | PHM-AP Trans-vs-CNN (snippet), vanilla-encoder limits Sensors25 (snippet), foundation-model zero-shot synth-data (snippet) |
| 10 | (a) GDN/USAD grounding | `USAD GDN graph deviation network multivariate time series anomaly detection SWaT WADI F1 score` | 8 | GDN AAAI21 (full text), USAD-unofficial/WADI (snippet, excluded — unofficial impl.) |
| 11 | (Q-E) leakage/imbalance | `data leakage time series cross-validation grouped episode splits SMOTE inside folds oversampling fault diagnosis` | 6 | MSSP26 bearing-leakage (snippet), PHM-EU leakage-safe bench (snippet), JFSC26 imbalance (snippet), TEP GroupShuffleSplit (snippet) |

Fetch accounting: 10 webfetch + 5 PDF→text conversions (pdftotext, pages noted per row) = 15 ≤ 25 cap.
PDF rule honored: no PDF opened as image; all PDFs converted to text first; MDPI-403 and
OpenReview-blocked rows fall back to abstract/snippet and are flagged SNIPPET-ONLY in the source table.
Snowballing: Kim refs → SPOT/Hundman/OmniAnomaly/POT lineage; TranAD refs → USAD/GDN/MTAD-GAT/LSTM-NDT/POT;
M2AD refs → USAD/OmniAnomaly/AnomalyTransformer/TimesNet/FITS; Schmidl refs → Wu&Keogh flaw lineage.

## TWIN TRAINING QUESTIONS (§4 of task) — the mapping target

- Q-A formulation per fault family (abrupt vs wear-drift vs sensor-vs-process vs cascade — one model or per-regime?)
- Q-B architecture shortlist + selection criteria for 32-machine 1 Hz multivariate data
- Q-C preprocessing pipeline order (state handling before/after normalization? imputation before features?)
- Q-D window/feature prescription incl. multi-scale
- Q-E imbalance + leakage rules (episode splits, grouped CV, resampling-inside-folds)
- Q-F thresholding + evaluation protocol consistent with raw-F1 + precision-side range

## T1. Architecture landscape on manufacturing benchmarks — raw-protocol numbers only

**Raw point-wise F1 (Kim et al. repro, Table 2 — PA-off column is the only admissible one):**
SWaT raw F1: USAD 0.791, LSTM-VAE 0.775, OmniAnomaly 0.782, GDN 0.81, Case-2 (‖input‖ baseline) 0.781,
Case-3 (untrained LSTM-AE) 0.789. WADI raw F1: GDN 0.57 (best of the seven), Case-2 0.353, USAD 0.232.
MSL/SMAP/SMD raw F1: ALL methods 0.19–0.53; best SMD raw is GDN 0.529 vs Case-2 0.494.
Two readings follow: (i) on SWaT/WADI the deep field barely separates from untrained/input-norm baselines —
Kim's "no significant improvement over baseline" verdict; (ii) GDN is the only method that is top-or-near-top
raw on SWaT (0.81), WADI (0.57), and SMD (0.529) simultaneously, and its numbers are computed point-wise
over test GT (Deng & Hooi Table 2), NOT PA-inflated — this is exactly the parent F5a refutation of "GNN ≤0.50 raw".
**Flagged PA-inflated rows (inadmissible as rankings):** MTAD-GAT 0.90/0.91/0.80, TranAD up-to-+17%,
Anomaly-Transformer, GTAD/MTAD rows cited in §4 of task — all reported under PA; TranAD's SOTA2 raw-WADI
row (49.51 vs MTAD-GAT 41.69 vs USAD 30.56, units ambiguous) is protocol-sensitive and kept as directional only.
**Consensus: MODERATE** — no universal winner; GDN has the broadest raw evidence; every PA-reported ranking
is inadmissible until re-scored raw. **→ Q-B** (shortlist must be re-scored raw on M0b; PA rows excluded).

## T2. Deep-vs-classical reversals (CONTRA-1 — nothing suppressed)

- MTAD benchmark (12 methods, 5 datasets, unified protocol): **KNN achieves best search-F1 on SMD, SMAP, MSL**
  WITHOUT PA; deep methods take the lead only AFTER PA — metric-dependence is the finding, and the authors add
  salience + delay + efficiency as deployment axes (classical methods report anomalies EARLIER, train faster).
- OLS closed-form regression beats deep SOTA on uni+multivariate TSAD benches (OpenReview 2026, SNIPPET-ONLY).
- "Sign of the Times" (KTH thesis, SNIPPET-ONLY): SMD/SMAP/MSL too simple/mislabeled to justify deep models,
  pre- and post-PA; holds on a Scania dataset too.
- Pattern Recognition 2022 (16 algos, 5 benches, SNIPPET-ONLY): no clear consistent superiority of DNN over
  conventional/ML; DNN edge only on some sets.
- Elephant/TSB-AD (NeurIPS24, full p1–4): "simpler architectures and statistical methods often yield better
  performance" across 40 algos/1070 series with tuned hyperparameters.
**Consensus: MODERATE** — classical baselines (KNN/PCA/iForest/LOF, OLS/linear) MUST be in the M0b battery as
tripwires; CPU-only twin makes their efficiency/delay advantage load-bearing, not cosmetic.
**→ Q-B** (selection criterion #1: beat KNN/PCA raw before any deep claim; criterion #2: delay + train-cost
reported alongside F1). Contradiction to T1 preserved: GDN-raw-strength and KNN-raw-wins coexist because
rankings are dataset×protocol-specific — the M0b battery on TWIN data is the only arbiter.

## T3. Transformer skepticism (CONTRA-2 — nothing suppressed)

- PHM-AP 2023 (SNIPPET-ONLY): Dilated CNN beats Transformer by ~25% (UCR, random masking) and ~60%
  (middle masking) at reconstruction; Transformer "not as well as expected".
- TranAD itself reports simple transformers underperform TranAD by >11% — the gains come from
  self-conditioning + adversarial training + MAML, not from attention per se.
- Sensors 2025 vanilla-encoder study (SNIPPET-ONLY): normal-only training restricts generalization to diverse
  anomaly patterns; authors call for semi-/self-supervised extensions.
- Counterweight (kept, not suppressed): foundation models show promise on POINT anomalies (Elephant);
  TranAD's MAML few-shot angle matters for data-scarce regimes — but the twin is data-RICH (free labels),
  so MAML's premise does not transfer.
**Consensus: CONTESTED** — transformers are not default-winners for 1 Hz sensor AD; CNN/LSTM forecasters stay
first-class. **→ Q-B** (Transformer enters shortlist only with a compute/accuracy justification vs TCN/LSTM;
default M0b deep arms: GDN-light + TCN/LSTM forecaster + USAD-style AE).

## T4. One-class/normal-only vs supervised-with-synthetic-labels (twin has FREE labels)

- Lau et al. 2025 (full text, L3 preprint): first formal semi-supervised AD formulation; adding uniform synthetic
  anomalies fixes (i) false-negative modeling (low-normal-density regions misclassified normal) and (ii) ragged
  regression functions (Prop. 1 zero-margin discontinuity → Prop. 2 continuity + minimax-optimal excess risk).
  Empirics (tabular/image/language, NOT time series): VC-SA beats VC on unknown AND known anomalies;
  prescribed dose n′ = n + n⁻. Caveats the authors state: dilution of known-signal + contamination of normal
  regions if overdosed — i.e., synthetic-labels CAN hurt generalization when misdosed or off-manifold.
- Han et al. 2022 via Lau: even 1% labeled anomalies → supervised methods empirically beat unsupervised.
- Schmidl (full p1–3, L1): supervised TSAD "rarely used" because it cannot detect UNSEEN anomalies — the
  standing objection; Lau's unknown-anomaly gains are the direct counter, but on non-temporal data.
- Twin reading: free sim labels put training in the STAD regime (Fig.1c of STAD survey snippet: "Labels Matter
  More Than Models"), yet the twin's 4–7σ rectangular faults risk a supervision trap — a classifier that keys on
  caricature magnitudes and misses 1–3σ incipient/wear-drift. Required hedge (from T4 + T8): held-out
  unknown-fault-family evaluation (wear-drift + SENSOR_VS_PROCESS pairs absent from training).
**Consensus: MODERATE for "use the free labels", CONTESTED for "synthetic augmentation dose/shape"**
(uniform-noise theory does not license uniform-noise injection on sensor manifolds).
**→ Q-A** (per-family formulation: abrupt → supervised detector; wear-drift → normal-only/drift-chain complement
per T8 of upgrade-spec; sensor-vs-process → parity-supervised mode head) **+ Q-E** (known-vs-unknown family
splits as a required eval slice).

## T5. Preprocessing for industrial time series

- Normalization scope is UNSETTLED academically: TranAD min-max per mode on TRAIN stats (global-per-channel);
  GDN robust per-sensor median/IQR error normalization (per-sensor, anomaly-robust); EUSIPCO19 (SNIPPET-ONLY):
  z-score preferred over min-max because rare events bias min-max. No paper retrieved prescribes per-machine vs
  global for multi-machine plants — genuine gap (see Gaps).
- Operating-state conditioning has the strongest support: Thibault framework (SNIPPET-ONLY, L3): only ~32% of
  time in steady state, 20 distinct regimes in 5 months of pulp-mill data; pipeline = sync → signal processing →
  steady-state detection → reconciliation → REGIME identification. M2AD (L1): asset STATUS as categorical
  covariates C into the forecaster (state-as-input, proven at 130-asset scale). GDN (L1): sensor embeddings
  self-organize by behavior class (t-SNE evidence) — i.e., conditioning can be LEARNED, not hard-split.
- Missing/stale handling: Fleck 2109.03469 (full text, L3): systematic missingness (nonlinear routes) →
  do NOT impute, do NOT drop; split into missing-free subsets + boosted residual ensemble (base on
  always-available signals). Twin map: S-DROPOUT/stale-hold is systematic (SHF=DROPOUT), so Fleck licenses the
  upgrade-spec rule "dropout windows EXCLUDED from scoring + SHF channel", and warns that imputing
  stale-holds would fabricate normal-looking segments. Random-occasional gaps → linear/last-value imputation
  per Thibault (SNIPPET-ONLY).
- Detrending/deseasonalizing: no retrieved paper isolates its AD effect; RobustPeriod/HP-filter (SNIPPET-ONLY)
  detrends before period detection; twin's AR1(0.6)+sinusoid base makes a seasonal residual the natural
  forecaster input rather than raw obs.
**Consensus: MODERATE** for state-conditioning + systematic-missing exclusion; **CONTESTED/OPEN** for
per-machine vs global normalization. **→ Q-C** (order: sync → SHF/dropout mask → state split/covariate →
per-sensor robust norm fit on RUN-normal-only → detrend/residualize → window → features; imputation ONLY for
random gaps, never for stale-hold; normalization stats fit inside episode folds).

## T6. Windowing + feature design

- Window lengths in the literature span 5 (GDN, forecasting step) to 100–120 (Kim τ=120; USAD/OmniAnomaly
  convention): GDN's w=5 works because forecasting needs short memory; reconstruction/discord methods need
  100+. Kim §3.3: LONGER windows INCREASE the input-norm baseline's F1 — window choice moves the baseline,
  so windows must be fixed before any architecture claim (M0b design rule).
- Principled selection exists: AALTD22 (SNIPPET-ONLY): autocorrelation-hill/RobustPeriod dominant-period
  detection (detrend → decouple periodicities → DFT+AC); FFT-guided multi-window USAD (SNIPPET-ONLY):
  windows {11,16,24,32,96} covering dominant cycles + DTW + Isolation Forest on errors, detects spikes AND slow
  drifts — closest published analog to the twin's Q-D multi-scale need.
- Time-frequency uncertainty is formal: Dual-TF WWW24 (SNIPPET-ONLY): Theorem 3.1 optimal inner-window
  condition trading time vs frequency uncertainty — licenses multi-scale over single-window.
- Feature side: PHM multimodal (SNIPPET-ONLY): envelope + FFT → Transformer + kNN/OOD, ~99% fault
  classification; MSCRED/MTAD-GAT signature-matrix + ConvLSTM/GRU for inter-sensor correlation; M2AD area
  error (±l integration) beats point error on contextual/noisy anomalies — directly relevant to wear-drift.
**Consensus: MODERATE** for multi-scale windows + spectral/envelope features; **OPEN** for exact lengths
(they must be derived from twin dominant periods, not imported). **→ Q-D** (multi-scale export 0.5/1/2× base
per upgrade-spec §5; base length from autocorrelation/RobustPeriod on clean RUN; features: time stats + FFT
band energy + envelope + area-error-style integrals; ablation per feature group).

## T7. Thresholding / calibration theory

- SPOT/DSPOT KDD17 (full p1–4, L1): POT/GPD threshold z_q from excesses over high threshold t via MLE
  (Grimshaw), single risk parameter q = target FP rate; DSPOT variant for drift; streaming, distribution-free.
  Assumption to log: exceedances follow GPD; needs enough peaks N_t (thin-data warning for T=300 episodes).
- OmniAnomaly adjusted-POT + TranAD POT head: POT is the de-facto deep-TSAD threshold, but both papers tune
  for best-F1 — the "searching for best F1 on TEST" practice MTAD explicitly bans (use EVT-selected threshold
  for F1, search-threshold only for F̂1, never optimize PA-F1).
- GDN (L1): threshold = max validation SMA score — simplest fixed rule, no distribution assumption, but
  brittle to validation contamination (twin: clean-episode validation is available — advantage).
- M2AD (L1, full mechanics): per-sensor GMM on TRAIN errors → two-sided p-values → Fisher aggregation with
  sensor weights → Gamma(α̂,θ̂) moment-matched calibration for DEPENDENT p-values (Prop. 2: χ² assumption
  miscalibrates both directions) → threshold at significance γ; top-k sensor contributions for interpretability;
  point-vs-area error switch (area for contextual/drift). This is the complete template for the upgrade-spec
  "M2AD-style GMM+Gamma layer" — with one flag: M2AD's TP counts ANY overlap (event-wise), so its +21%
  is NOT raw-F1 evidence (see T8).
- γGMM (SNIPPET-ONLY, L2): Bayesian contamination-factor estimation — relevant if calibration normals are
  imperfect; twin's clean episodes make this a fallback, not the default.
**Consensus: STRONG** that threshold must be data-driven with a pre-registered FP target (POT-risk-q or
Gamma-significance or fixed-percentile), never test-searched; **MODERATE** for GMM+Gamma as the
multi-sensor template. **→ Q-F** (three calibration arms per upgrade-spec T7: fixed-percentile baseline vs
POT/SPOT vs GMM+Gamma; threshold fit on clean-episode validation ONLY; report alerts/1000 at 20/150/5).

## T8. Evaluation methodology (locks Q-F)

- PA is INVALID (Kim, STRONG consensus + proof + random-beats-SOTA + F1PA–F1 correlation ≈ 0 on SWaT):
  parent F1 lock stands. PA%K (Kim §4.2) is a PA-repair, NOT adopted — raw F1 + companions instead.
- Schmidl (L1): threshold-agnostic AUC-ROC/PR/PTRT for SCORING quality; thresholding is "orthogonal,
  algorithm-independent" — i.e., evaluate scores and operating points separately; supervised methods "restricted
  … rarely used" (countered by T4 for the twin's labeled regime).
- Affiliation (L1, full p1–4): parameter-free, adversary-robust, local per-event precision/recall in time units;
  affiliation explicitly shows range-based predecessors are gameable — use as DIAGNOSTIC companion, not the
  locked bar (random ≈ lower bound by construction).
- Elephant (L1): VUS-PR most reliable single measure under lag/label-noise; lag-robustness matters for the
  twin's delay faults (+d cycle) and incipient drift onsets.
- MTAD (L2): salience (score separability → threshold-choice robustness) + delay (points-to-first-detection
  per segment) + efficiency — delay maps directly to twin delay faults and cascade hop latency; salience maps
  to the precision-side healthy-window bar.
- M2AD overlap-TP + F0.5 (precision-favoring): directionally aligned with the twin's precision-side bar (T5)
  but protocol-incompatible with raw F1 — cite as supporting philosophy, never as numeric evidence.
**Consensus: STRONG** (raw point-wise F1 + fixed calibration as the admissibility bar; delay-aware + range-side
companions allowed as SECONDARY). **→ Q-F** (primary: raw-F1 PA-off fixed-percentile; secondary:
delay-to-detection per fault family + affiliation/VUS-PR diagnostic + healthy-window alert rate; NO overlap-TP,
NO PA%K, NO test-searched thresholds).

## T9. Imbalance + leakage discipline (Q-E rules)

- Leakage-safe grouping is CONVERGENT across 2026 literature (SNIPPET-ONLY, L2–L3): MSSP26 bearing study —
  group dependent samples (bearing-level) in same fold + blocked CV, "look-ahead bias" framing; PHM-EU26 —
  recording-level train/test separation, six fixed scenarios, repeated-seed follow-up; TEP practice —
  GroupShuffleSplit by simulation RUN. Twin map 1:1 → EPISODE-SEEDED splits (upgrade-spec §5 already requires
  this; evidence upgrades it from convention to multi-study consensus).
- Resampling INSIDE folds only (I15 convention, retained): JFSC26 (SNIPPET-ONLY, L4): XGBoost-no-handling
  F1 0.8952 binary beats SMOTE/ADASYN (p=0.0312); class-weighting lifts macro-recall 0.8448→0.8908 without F1
  loss — i.e., default to no-resampling + class weights, add SMOTE only via ablation. Purged/embargo splits
  (SNIPPET-ONLY) for 50%-overlap windows: contamination half-life = window length.
- Normalization/threshold statistics fit inside folds (from T5+T7): per-sensor medians/IQRs, min-max ranges,
  GMMs, POT fits never see held-out episodes.
**Consensus: MODERATE-STRONG** for episode-grouped splits + no test-peeking stats; **CONTESTED** for SMOTE on
sensor windows (default OFF). **→ Q-E** (rules: episode-seeded grouped CV; wear/maint/family stratification;
overlap-window purge/embargo; resampling-inside-folds-only; class-weight-first).

## T10. State/regime handling: one model or per-regime? (Q-A core)

- Evidence for state-AS-INPUT: M2AD covariates (130-asset production proof); TranAD global min-max (implicit
  single-model); GDN learned embeddings (single model, behavior clusters emerge).
- Evidence for state-SPLIT models: Thibault (steady-state 32% + 20 regimes — a single model must span wildly
  different dynamics); twin's own measured mix (RUN 68.8% vs STARVED 29.5% vs DOWN 1.7% — DOWN has ~160
  machine-steps TOTAL, unlearnable as its own regime).
- Synthesis: neither extreme is licensed. Single model with state covariate + per-sensor robust norms covers
  RUN/STARVED/BLOCKED; DOWN is too thin to model — treat as masked/excluded context (with SHF-like flag),
  not a training regime. Wear-drift (WSTATE>0.8) is NOT a switchable regime but a continuum → drift-chain
  complement (upgrade-spec T8), not a separate classifier.
**Consensus: MODERATE** for single-model-with-covariates + DOWN-masking. **→ Q-A + Q-C**.

## Gaps & Contradictions (nothing suppressed)

1. **GDN-raw-0.81 vs KNN-raw-wins vs untrained-baseline-parity**: all three are true on different datasets.
   No theory predicts which regime the twin's 32-machine 1 Hz data falls in → M0b battery decides; any
   pre-battery ranking is speculation. (Maps Q-B.)
2. **M2AD +21% uses overlap-TP**: its headline number is inadmissible under the F1 lock; only its
   MECHANICS (GMM+Gamma, area error, covariates) transfer. Same flag on TranAD +17% and MTAD-GAT rows. (Q-F.)
3. **Synthetic-help vs synthetic-hurt**: Lau theory + Han 1% vs dilution/contamination caveats + Schmidl
   unseen-objection. Twin resolution requires the known-vs-unknown-family eval slice — currently DESIGNED
   (upgrade-spec §4 labels) but unrun. (Q-A, Q-E.)
4. **Per-machine vs global normalization**: zero retrieved papers decide it for multi-machine plants; GDN
   per-sensor robust vs TranAD global min-max coexist without comparison. M0b ablation owed. (Q-C.)
5. **Window 5 vs 120**: forecasting vs reconstruction optima differ by 24×; no joint theory except Dual-TF
   uncertainty tradeoff (SNIPPET-ONLY). Twin must fix windows from its own dominant periods first. (Q-D.)
6. **EVT stationarity vs wear-drift**: SPOT assumes stationary stream; DSPOT handles abrupt drift, not slow
   knee-drift — POT thresholds on drifting residuals need re-validation (ties upgrade-spec T7 tripwire). (Q-F.)
7. **SMOTE on time windows**: JFSC no-handling-wins vs I15 SMOTE-inside-folds convention — ablation owed,
   default OFF. (Q-E.)
8. **Affiliation/VUS vs raw-F1 lock**: companions are diagnostic-only; promoting them to primary would
   re-introduce protocol shopping through the back door. Boundary logged, not crossed. (Q-F.)
9. **Missing-data theory gap**: Fleck covers systematic-route missingness; no retrieved work covers
   twin-style stale-hold + parity-flag joint modeling — SHF design is precedent-informed, not evidence-backed.
   (Q-C.)
10. **Transfer numerics**: all cross-dataset magnitudes (GDN 0.81, KNN wins, +21%) are BENCHMARK-scoped;
    twin-scoped values await M0b. No number in this review transfers to the twin. (All Qs.)
