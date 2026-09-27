# Canonical literature review — ML AD training + preprocessing for the Verdandi twin (follow-up 02)

Date: 2026-09-13 · Merger of `review-academic.md` (peer-reviewed lane, T1–T10, Q-A..Q-F) + `review-industry.md` (production lane, T-P1–T-P8, P-A..P-F).
Twin context: 32-machine SimPy twin, T=300, 1 sample/step, 7→15 channels, FREE sim labels, CPU-only, team 2–4 students.
Ordering principle (CONTEXT.md): training + preprocessing requirements FIRST; data-generation changes are downstream of what training needs.

PRISMA flow: academic 11 queries → ~86 snippets retrieved → 96 screened → 25 kept (11 full-content + 14 SNIPPET-ONLY); industry 12 queries → 96 retrieved → 96 screened → 18 fetched-and-kept + ~8 snippet-level (21/25 fetch cap); merged canon = 51 rows (25 A + 26 J) after dedup (§8) → themes below → hypotheses in `02-hypotheses/`.

## Locks honored (non-negotiable)

- Parent F1 (Locked): raw point-wise F1 only, PA-off, fixed-percentile calibration. All PA-reported rankings (MTAD-GAT 0.90/0.91/0.80, TranAD +17%, Anomaly-Transformer rows, M2AD +21%, Merlion tutorial F1) are INADMISSIBLE as numeric evidence — mechanics may transfer, numbers never do.
- Parent F2 (Locked): no platform/ROI framing; verdicts battery-scoped only. Vendor outcome magnitudes ($96M, 94% precision, −75% waste, 30%-faster UPW) quarantined to pattern use (source-table §Q).
- Parent F3 (Locked): no competitor wedge claims in this review.
- Sibling 01 compatibility (upgrade-spec §5 + locks L1–L4/F12–F13): base tick 1 sample/step; multi-scale export 0.5/1/2×; episode-seeded + wear-stratified splits; pre-/post-maintenance window labels; 50%-overlap FFT windows; resampling INSIDE folds only; calibration-normals ~300/episode guidance DIRECTIONAL only (§7 tensions T-C2, T-C5 recorded, spec unchanged).
- Sibling contested F7–F11 respected: no per-term physics verdict, no impulse ≥15% claim, no fixed budget constant, no complement win, no manual-spec blocker imported here.

## 1. Search strategy — academic angle (verbatim queries, Q-A..Q-F coverage)

| # | Verbatim query | Retrieved → kept |
|---|---|---|
| 1 | `deep time-series anomaly detection USAD TranAD Anomaly-Transformer GDN MTAD-GAT compared SWaT WADI SMD benchmark` | 8 → TranAD VLDB22 (full), MTAD-GAT (snippet), TiTAD note (snippet) |
| 2 | `Kim et al point adjustment invalid anomaly detection time series random scores SOTA critique` | 8 → Kim AAAI22 (full), PA-manipulability proof + adversarial stress-test (snippets) |
| 3 | `one-class normal-only training vs supervised synthetic anomalies time series anomaly detection` | 8 → Lau 2506.13955 (full), RoCA (excluded, off-twin), HAI+SGAN (snippet) |
| 4 | `preprocessing industrial time series normalization per-sensor detrending deseasonalizing missing data imputation operating regime conditioning` | 8 → Fleck 2109.03469 (full), Thibault framework (snippet, 403), EUSIPCO z-score (snippet) |
| 5 | `sliding window length selection FFT wavelet envelope features multivariate time series anomaly detection manufacturing` | 8 → FFT multi-window USAD, Dual-TF, AALTD22, PHM envelope+kNN (snippets) |
| 6 | `anomaly threshold calibration extreme value theory POT M2AD GMM Gamma rarity score time series` | 8 → M2AD AISTATS25 (PDF→text p1–6), SPOT KDD17 (PDF→text p1–4), γGMM (snippet) |
| 7 | `time series anomaly detection evaluation point-wise event-wise affiliation metric delay-aware Schmidl benchmark` | 8 → Schmidl VLDB22 (p1–3), Affiliation KDD22 (p1–4), Elephant NeurIPS24 (p1–4), TimeEval (snippet) |
| 8 | `KNN beats deep learning time series anomaly detection reversal simple baselines MTAD benchmark` | 8 → MTAD bench 2401.06175 (full), OLS-beats-SOTA + Sign-of-the-Times + Do-DNN-contribute (snippets) |
| 9 | `transformer skepticism time series anomaly detection synthetic anomalies hurt generalization shortcomings` | 8 → PHM-AP Trans-vs-CNN, vanilla-encoder Sensors25, foundation-model synth-data (snippets) |
| 10 | `USAD GDN graph deviation network multivariate time series anomaly detection SWaT WADI F1 score` | 8 → GDN AAAI21 (full), USAD-unofficial (excluded, unofficial impl.) |
| 11 | `data leakage time series cross-validation grouped episode splits SMOTE inside folds oversampling fault diagnosis` | 6 → MSSP26 leakage, PHM-EU leakage-safe bench, JFSC26 imbalance, TEP GroupShuffleSplit (snippets) |

Fetch accounting: 10 webfetch + 5 PDF→text (pages noted) = 15 ≤ 25 cap. TEXT ONLY honored (no PDF-as-image; MDPI-403/OpenReview-blocked → SNIPPET-ONLY).

## 2. Search strategy — industry angle (verbatim queries, P-A..P-F coverage)

| # | Verbatim query | Retrieved → kept |
|---|---|---|
| Q1 | `production anomaly detection training pipeline data cleaning normalization windowing MLflow model registry manufacturing` | 8 → 5 (J12, J15, J16, J09 + 1) |
| Q2 | `feature store manufacturing retraining triggers model registry MLflow ClearML time series` | 8 → 4 (J04, J07 + 2) |
| Q3 | `weak supervision Snorkel active learning labeling rare faults predictive maintenance manufacturing` | 8 → 4 (J21, J22 + window-label rows) |
| Q4 | `imbalanced anomaly detection production normal-only training cost-sensitive loss threshold tuning per asset` | 8 → 4 (J02, J17 + cost rows) |
| Q5 | `startup shutdown changeover filtering operating regime gating preprocessing manufacturing sensor data downtime codes` | 8 → 4 (J12, J18 + snippets) |
| Q6 | `sim2real fine-tuning pretrain synthetic data fine-tune real frozen backbone industrial defect detection paired data` | 8 → 4 (J19, J23–J25) |
| Q7 | `anomaly detection deployment monitoring alert budget shadow mode per-asset thresholds suppression gates production` | 8 → 5 (J05, J10, J13 + 2) |
| Q8 | `imbalanced-learn pipeline resampling leakage SMOTE inside cross-validation official docs` | 8 → 4 (J01, J03 + 2) |
| Q9 | `predictive maintenance value failure false positives alert fatigue plant deployment lessons learned` | 8 → 6 (J10, J11, J13 + 3) |
| Q10 | `feature store overhead postmortem sim pretraining negative transfer manufacturing AutoML overfitting plant failure` | 8 → 5 (J15, J23 + 3) |
| Q11 | `pre-maintenance window labeling post-maintenance normal anomaly detection manufacturing operator codes audit` | 8 → 4 (J10, J18 + 2) |
| Q12 | `PyTorch ONNX export time series anomaly detection CPU inference Merlion PyRCA documentation` | 8 → 4 (J06, J08, J14 + 1) |

Fetch accounting: 21/25 used. TEXT ONLY honored (PDF-only hits held at snippet/UNVERIFIED or excluded from content claims).

## 3. Cross-angle themes (7; each cites both lanes)

### T-C1. Rankings are dataset×protocol-specific; the M0b battery on twin data is the only arbiter
Academic: GDN raw top-or-near-top on SWaT (0.81)/WADI (0.57)/SMD (0.529) [A02] coexists with KNN best raw search-F1 on SMD/SMAP/MSL [A04], OLS-beats-deep [A13], and untrained/input-norm baselines matching SOTA raw on SWaT [A01]. Industry: equipment-type-specific models beat generalized by ~18pp (pattern-grade) [J10]; generic modeling is the named failure mode [01-C4]. Merged rule: no pre-battery ranking; classical tripwires (KNN/PCA/iForest/LOF, OLS/linear) + graph-free control stay in the battery; delay + train-cost reported alongside F1. → Q-B.

### T-C2. Supervision: use the free labels, hedge the caricature trap (both lanes converge on unknown-family evaluation)
Academic: Lau formal semi-supervised gains on known AND unknown anomalies (non-temporal data) + Han 1%-labels-beat-unsupervised [A10] vs Schmidl unseen-objection [A06] + dilution/contamination caveats [A10] + 4–7σ rectangular-trap reading (T4). Industry: window-labels + back-labeling (failure→earliest precursor) + disposition feedback loop converting unsupervised→supervised over time [J10, J12, J17]; never start with hand-labeling [T-P2]. Merged rule: per-family formulation (abrupt→supervised; wear-drift→normal-only/drift-chain complement per upgrade-spec T8; sensor-vs-process→parity-supervised head) + mandatory held-out unknown-fault-family slice (wear-drift + SENSOR_VS_PROCESS absent from training). → Q-A, Q-E.

### T-C3. State handling ladder: filter-from-baseline → state-as-feature → per-class models (full cross-angle agreement)
Academic: state-as-covariate (M2AD 130-asset proof [A05]) + learned embeddings [A02] vs steady-state-32%/20-regimes split pressure [A19]; synthesis = single-model-with-covariates + DOWN-masking (DOWN ~160 machine-steps unlearnable) [T10]. Industry: identical ladder — filter transients [J12], state-as-feature ("normal at full load, anomalous at idle" [J13]), per-type models (+18pp pattern [J10]); MAINT_EVENT reset + suppression [J09, J11]. Merged pipeline order: sync → SHF/dropout mask → state split/covariate → per-sensor robust norm (RUN-normal-only) → detrend/residualize → window → features; imputation ONLY for random gaps, never stale-hold [A11]; twin exports state/reason/event streams as stratification keys. → Q-A, Q-C, Q-D.

### T-C4. Leakage discipline is settled across all 2026 evidence: episode-grouped splits + inside-fold stats (Resolved)
Academic: bearing-level grouping + blocked CV [A20], recording-level separation + repeated seeds [A21], GroupShuffleSplit by RUN [A11-row]. Industry (official): imblearn.pipeline inside-folds [J01], fit-train/transform-test [J03]; purge/embargo for 50%-overlap windows. Upgrade-spec §5 already requires episode-seeded + wear-stratified splits — evidence upgrades it from convention to multi-study consensus. Merged rule: episode-seeded grouped CV; wear/maint/family stratification; overlap purge/embargo; ALL stats (norms, GMMs, POT fits) fit inside folds. → Q-E.

### T-C5. Imbalance: default OFF resampling, class-weight/cost-first, inside-pipeline if ever used (tension with spec wording logged, spec unchanged)
Academic: no-handling XGBoost beats SMOTE/ADASYN (p=0.0312); class weights lift macro-recall without F1 loss [A22]. Industry: normal-only default [J17, J12]; cost-sensitive post-tuning (`TunedThresholdClassifierCV` [J02], pos_weight 11.5 + negotiated tiers [J15]); resampling MANDATED inside-pipeline WHEN used [J01]. Upgrade-spec §5 permits "SMOTE-family … INSIDE CV folds only" — compatible: permission ≠ default. Merged rule: default no-resampling + class weights; SMOTE only via ablation, always inside `imblearn.pipeline`; per-class thresholds, never one global cut-off. → Q-C, Q-E.

### T-C6. Thresholding: three calibration arms, pre-registered FP target, never test-searched (mechanics from academic, budget discipline from industry)
Academic mechanics: fixed-percentile baseline [A02: max-validation-SMA] vs POT/SPOT risk-q [A09] vs GMM+Gamma dependence-correct calibration [A05]; threshold-agnostic scoring vs operating points split [A06]; companions (affiliation [A08], VUS-PR [A07], salience/delay/efficiency [A04]) diagnostic-only under the F1 lock. Industry discipline: threshold-to-budget-then-read-recall [J13]; per-asset thresholds [J06, J10]; shadow 2–6 wks + backtesting before live [J05, J12]; suppression outside the model [J13, J05]. Merged rule (upgrade-spec T7): three arms (fixed-percentile vs POT/SPOT vs GMM+Gamma), fit on clean-episode validation ONLY; report alerts/1000 at 20/150/5; delay-to-detection per family + healthy-window rate as companions; NO overlap-TP, NO PA%K, NO test-searched thresholds. → Q-F.

### T-C7. Deployment reality constrains training: precision-side gates ship with every detector; retrain on evidence; Sim2Real needs paired real
Academic: precision-side bar philosophy (M2AD F0.5 direction, not its numbers) [A05]; EVT stationarity vs wear-drift tripwire (DSPOT covers abrupt drift, not knee-drift) [A09]. Industry: alert budget as design input (handful/shift [J13]; 89%-FP abandonment [J10]); evidence-triggered retrain (metric + drift + MAINT_EVENT, never calendar-only [J06, J09, J11, J12, J15]); sim-pretrain→fine-tune on ~5–10% paired real (0.25→0.89 mAP case [J19], vision-modality caveat) with negative-transfer control [J23]; full (not frozen) fine-tuning budgeted for sensory signals. Merged: every detector ships with (budget, shadow log, suppression rules, tier routing, precision-side metrics); twin keeps FAULT_RANGES randomization + seeded episode IDs for future pairing; no sim-pretrained promotion without paired-real validation. → P-E, P-F, generation back-propagation.

## 4. Tension check (explicit)

- Vs parent F1: clean — all rankings cited PA-off/raw; PA rows flagged inadmissible, never ranked. Companions (affiliation/VUS-PR/delay) diagnostic-only; promoting them to primary would re-introduce protocol shopping (academic Gap 8 carried).
- Vs parent F2: clean — no ROI/value prose; vendor magnitudes quarantined (§Q of source table); verdicts battery-scoped.
- Vs parent F3: clean — no competitor coverage claims.
- Vs sibling F12 (Locked L1+L2: physics-residual gain + envelope-metric validity): reinforced — T-C3 state-covariate + T-C6 area-error/envelope features reuse the licensed instrument; no twin-transfer number asserted.
- Vs sibling F13 (Locked L3+L4: drift-collapse fact + rate-budget practice): reinforced — T-C6 budget reporting + T-C2 drift-chain complement assume the split; the 20/150/5 reporting triple is the F9 surviving-range shape, not the refuted 10/1000 constant.
- Vs sibling contested F7–F11: no position taken — per-term physics (F7), impulse ≥15% (F8), budget-constant demotion (F9), complement win (F10), manual-spec blocker (F11) all stay open; T-C5's default-OFF does NOT revoke upgrade-spec §5's inside-folds SMOTE permission.

## 5. Gaps (merged, nothing suppressed; ≤10 provenance gap-fills used: 0 — merge only, no new claims)

1. Per-machine vs global normalization undecided for multi-machine plants (GDN per-sensor robust vs TranAD global min-max coexist; industry per-asset thresholds are inference-side, not training-norm evidence) → M0b ablation owed. (Q-C.)
2. Window 5 vs 120 (24× forecasting-vs-reconstruction gap); base length must come from twin dominant periods (autocorrelation/RobustPeriod), not imports. (Q-D.)
3. Synthetic dose/shape on sensor manifolds: uniform-noise theory ≠ injection license; known-vs-unknown-family slice designed but unrun. (Q-A, Q-E.)
4. EVT stationarity vs wear-drift: POT on drifting residuals needs re-validation (upgrade-spec T7 tripwire). (Q-F.)
5. SMOTE-on-windows ablation owed, default OFF (T-C5). (Q-E.)
6. Twin-style stale-hold + parity-flag joint modeling: precedent-informed (Fleck), not evidence-backed. (Q-C.)
7. No paired-budget number for multivariate factory time-series (all Sim2Real budgets are vision-modality) → 5–10% rule is a hypothesis with validation tripwire. (Generation.)
8. Window lengths, feature sets, normalization scope, threshold rules → the ≤5 falsifiable hypotheses in `02-hypotheses/` (Phase 2 seeds come from contradictions-map rows).
9. All cross-dataset magnitudes bench-scoped; twin-scoped values await M0b. No number here transfers to the twin.
10. Sibling-01 open legs reused, not re-litigated: U10/U11 (calibration-normals constants), U14 (worst-zone recording), mask-correctness sensitivity.

*End — canonical merged review: PRISMA line + 2 search tables + 7 cross-angle themes + explicit tension check + 10 gaps. Sources: `source-table.md`. Contradictions: `contradictions-map.md`.*
