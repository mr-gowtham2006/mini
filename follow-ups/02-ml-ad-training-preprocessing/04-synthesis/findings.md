# Findings — follow-up 02: ML AD training + preprocessing (training-first)

Date: 2026-09-13 · Scope: `01-background/` (51-row canon, 7 cross-angle themes, 9-row contradictions map) + `02-hypotheses/` (H1–H5 registry) + `03-evidence/` (5 supporting + 5 contra + evidence-log + claim-lock annex). No new retrieval. No twin-transfer numbers.

Rule honored: synthesis cites ONLY LOCK-1..6 as established (+ parent F1–F3, sibling L1–L4). U-1..U-6 appear solely as open legs. Removed-H citations (Han-1%, OSAD, DCD-VAE, A19-half/MDPI-Processes, FluxEV) never appear as support. §Q quarantine honored: vendor magnitudes + PA rows never rank. All PA-numbered rows flagged inadmissible per parent F1.

Confidence rubric (strictly enforced, no straddling ranges): High ≥85% = L1–L2 base · Moderate 70–85% = L3 + survived falsification · else Contested with no number.

---

## TF1 [ESTABLISHED] Episode-grouped splits + inside-fold statistics is the pipeline rule

**Claim:** Train/test splits are episode-seeded and grouped (no window leakage across episodes); ALL statistics (norms, GMMs, POT fits, resampling) are fit inside folds / fit-train-transform-test. 50%-overlap windows require purge/embargo of one window length. Upgrade-spec §5's episode-seeded + wear-stratified rule is thereby upgraded from convention to multi-study consensus.

**Confidence:** High, 85–88% (L2 peer-reviewed method + L3 official-docs convergence + contradictions-map Row 7 Resolved-agreement; two independent lanes, no counter-position found).

**Evidence strength:** GRADE High — 5 sources: MSSP26 bearing-grouped + blocked CV (L2 snippet); PHM-EU26 recording-level separation + repeated seeds (L3 snippet); TEP GroupShuffleSplit-by-RUN row (L3); imbalanced-learn `imblearn.pipeline` inside-folds official docs (L3); scikit-learn fit-train/transform-test + Pipeline-enforcement official docs (L3).

**Key sources:** doi 10.1016/j.ymssp.2026.114640 · phmsociety.org PHM-EU26 4924 · imbalanced-learn.org/stable/common_pitfalls.html · scikit-learn.org/stable/common_pitfalls.html · ar5iv 2109.03469 (Fleck/Gap grouping row).

**Remaining uncertainty:** Exact purge/embargo length for 50%-overlap FFT windows (one window length is the rule-of-thumb, not a measured optimum); wear/maint/family stratification weights for the 13% kit funnel are unrun.

**Alternative explanations:** (1) Random window splits with overlap-purge suffice and episode grouping wastes data — rebutted: purge fixes overlap leakage but not episode-trajectory leakage (same-seed windows share dynamics); grouping is the strictly safer rule at negligible cost given free sim episodes.

**Would-be-overturned-by:** An episode-grouped vs window-random ablation on twin data showing |ΔF1_raw| < 0.01 AND zero episode-identity leakage (nearest-neighbour episode-ID recovery ≤ chance +2pp). Numeric tripwire: leakage-unit-test failure rate >5% on the random-split arm while grouped arm holds — until then TF1 stands.

---

## TF2 [ESTABLISHED] Imbalance default: no-resampling + class-weight/cost-first; SMOTE only via ablation, always inside `imblearn.pipeline`

**Claim:** The pipeline DEFAULT is no resampling with class weights / cost-sensitive threshold post-tuning. SMOTE-family is permitted (upgrade-spec §5 inside-folds permission retained, unchanged) but default-OFF and ablation-gated. Per-class (not one global) thresholds. This is permission-vs-default reconciliation, not a spec revision.

**Confidence:** Moderate, 72–80% (L3 official-docs + L4 snippet industrial convergence + contradictions-map Row 5 Leaning-B; no L1/L2 anchor — capped below High by rubric).

**Evidence strength:** GRADE Moderate — 5 sources: JFSC26 no-handling-XGBoost-beats-SMOTE/ADASYN p=0.0312 + class-weights-lift-macro-recall (L4 snippet); sklearn `TunedThresholdClassifierCV` cost-matrix post-tuning (L3 official); imblearn inside-pipeline mandate WHEN used (L3 official); normal-only-first production pattern J17/J12 (L4); pos_weight-11.5 + negotiated tiers case (L4 pattern).

**Key sources:** ejournal.ptti JFSC26-399 · scikit-learn.org cost-sensitive guide · imbalanced-learn.org pitfalls · averroes.ai normal-only guide.

**Remaining uncertainty:** SMOTE-on-windows ablation unrun on twin data (Gap 5 of review); whether class weights suffice on the 13%-funnel kit classes (assembly/rework starved of examples) vs per-class threshold personalization absorbing the gap (H4-A3 interaction).

**Alternative explanations:** (1) SMOTE-inside-folds on window embeddings helps minority fault families (post-knee wear, MULTI_ROOT, SENSOR_VS_PROCESS) beyond what weights achieve — live via the retained ablation permission; (2) normal-only training makes the whole imbalance question moot for drift families — live via H1's per-family formulation (drift-chain complement).

**Would-be-overturned-by:** SMOTE-inside-pipeline ablation on twin M0b data with F1_raw(SMOTE) − F1_raw(weights-only) ≥ +0.02 on minority-family slices AND leakage unit test passing. Numeric tripwire: +2pp minority-slice gain — below it the default-OFF stands.

---

## TF3 [ESTABLISHED] No deep claim without beating max(KNN, PCA) raw on the same battery (classical-tripwire gate)

**Claim:** LOCK-1 restated as pipeline gate: under shared splits/features/windows and fixed validation-max calibration, a deep-vs-classical raw-F1 delta is a well-defined published quantity; therefore the H2 wire (GDN-light − max(KNN,PCA) ≥ +3pp) is a load-bearing instrument. Every deep reading of H1/H3/H4/H5 reports its classical delta. Controls iForest/LOF/OLS reported, never wire-moving. (LOCK-1 + LOCK-6 jointly.)

**Confidence:** High, 86–90% (L1 primary GDN raw table + L3 STAND common-harness method fact; 5-query counter-search failed to refute — all deep-clears-wire hits voided as PA-on).

**Evidence strength:** GRADE High — 6 sources: Deng & Hooi AAAI21 Table 2 SWaT 0.81-vs-0.23/0.08, WADI 0.57-vs-0.10/0.08 (L1 full-text R); STAND TSB-AD common harness running classicals under one protocol (L3 full-text R); Schmidl VLDB22 threshold-agnostic AUC General Finding 2 (R); Wu & Keogh flaw taxonomy (P); TranAD PA-on/PA-off dual table as protocol-duality exhibit (R); KTH-thesis + Sign-of-the-Times no-DNN-superiority direction (snippet).

**Key sources:** ar5iv 2106.06947 §4.3+Eq.12 · arxiv 2511.16145v1 §V-B2 · timeeval.github.io/evaluation-paper · arxiv 2009.13807.

**Remaining uncertainty:** The TWIN wire outcome itself (U-2): bench precedent shows clearable elsewhere (+58pp SWaT); twin caricature-rectangular mix predicts compression/reversal. No literature can close it — M0b read-off only.

**Alternative explanations:** (1) Twin is WADI-easy for GNNs and GDN-light clears by ≥+3pp (GDN 0.81-SWaT precedent transfers) — exactly H2-false; the wire adjudicates it. (2) ALL arms <0.60 raw-F1 → battery uninformative, gate untested, magnitude ladder (1–3σ incipient) is the back-propagated fix — H2-A2 branch.

**Would-be-overturned-by:** M0b battery read-off: F1_raw(GDN-light) − max(F1_raw(KNN), F1_raw(PCA)) ≥ 0.03 clears the gate (H2 falsified, graph-ablation ≥10pp becomes next hurdle); max(raw-F1) < 0.60 declares battery-inconclusive instead. Numeric tripwire: +3pp / 0.60 floor.

---

## TF4 [ESTABLISHED] Regime/mode conditioning beats mode-agnostic scoring — direction only, no magnitude, no ordering

**Claim:** LOCK-5 restated: ignoring operating mode creates blind-spot failures; conditioning on regime (covariate, per-regime model, or conditioned thresholds) recovers detection. DIRECTION ONLY. Says nothing about single-covariate-vs-per-state ordering (TF5 contested) and carries no transfer number.

**Confidence:** High, 85–88% (L1 mechanics + L3 full-text controlled benchmark + contra-corroborated conditioning witnesses; counter-search threatened ordering only, never the principle).

**Evidence strength:** GRADE High — 6 sources: TEP product-aware bank 100%-vs-22.2% blind-spot stress (L3 full-text R); M2AD STATUS covariates + χ²-miscalibration Prop. 2, 130-asset case (L1 P mechanics-only); HSMM+PCA per-mode 100%/98.3% vs 2–5% single-model (P); SCAL state-conditioned 95.6% (P); ABB zero-FP/FN contextual (P); SCDT context-conditioned envelopes (P).

**Key sources:** arxiv 2606.00052 (Islam & Carden) · PMLR v258 M2AD · HAL 03875921 · IOP 10.1088/1361-6501/aea242.

**Remaining uncertainty:** Which conditioning rung the twin needs (ordering = TF5); DOWN-masking isolated contribution (U-6 secondary); whether STARVED −2σ offsets behave like TEP thermodynamic modes or like label-noise regimes.

**Alternative explanations:** (1) Filtering (RUN-only baseline + regime-conditioned thresholds, no covariate) captures the whole gain — the covariate is decoration; adjudicated by H3 tripwire-1 (C−F ≥+2pp) + masking/threshold ablations. See TF5.

**Would-be-overturned-by:** M0b STARVED-heavy subset where covariate + per-state + filter-only arms all land within ±1pp raw-F1 AND within ±2/1000 healthy-window alert rate → conditioning irrelevant on twin data. Numeric tripwire: <1pp spread across all three arms.

---

## TF5 [CONTESTED — no number] Single-covariate vs per-state vs per-class ordering on STARVED-heavy data

**Claim status:** Contested. Covariate-vs-filter leg survives (TF4); C-vs-P ordering leg provisionally falsified by per-regime precedents (C1+C2: product-aware 100%-vs-22.2%, HSMM+PCA 98–100%). Pipeline consequence (locked by this synthesis): single-covariate (Arm C) AND per-state bank (Arm P) BOTH ship to M0b; no default winner. Per-class (A/B/C/ASM/RWK) arm rides alongside as the A1 escalation.

**Confidence:** Contested, no number (L3 full-text per-regime precedents cut against C-vs-P; covariate witnesses S1–S3 support conditioning-in-general, not the ordering; missing leg U-3: no study runs single-with-covariate vs per-state head-to-head on STARVED-heavy data).

**Evidence strength:** GRADE Moderate — 5 sources split 3-vs-2: against ordering: product-aware TEP (L3 R), HSMM+PCA multimode TEP (P), Eusipco-2012 interference effects (snippet); for covariate viability: SCAL (P), ABB contextual (P). No head-to-head exists anywhere found.

**Key sources:** arxiv 2606.00052 · HAL 03875921 · Eusipco2012 1569587469 · IOP aea242 · ZHAW ABB collection.

**Remaining uncertainty:** Everything ordering-shaped: whether cross-state context + data-thin STARVED pools (H3 prediction 2) outweigh per-regime specialization; per-class-vs-covariate gap (A1); DOWN-masking vs covariate attribution (A3); regime-label Sim2Real fragility (C4 label-noise analog).

**Alternative explanations:** (1) A1 per-class wins (per-class − C ≥ +2pp) → heterogeneity defeats covariate capacity, ladder moves one rung up. (2) A2 filtering suffices (C−F < +2pp) → STARVED offsets harmless under robust norms, adopt simpler filter rule. (3) A3 DOWN-masking does the work → mask, not covariate, is the active ingredient.

**Would-be-overturned-by:** M0b STARVED-heavy (≥40% STARVED steps) subset: H3 survives iff F1_raw(C) − F1_raw(F) ≥ 0.02 AND F1_raw(C) − F1_raw(P) > 0.00; A1 fires iff per-class − C ≥ +2pp. Numeric tripwire: +2pp vs filter / >0 vs per-state / +2pp per-class-escalation.

---

## TF6 [CONTESTED — no number] Supervised-with-free-labels: known-family margin leg survives, unknown-family leg provisionally falsified

**Claim status:** Contested. Per-family supervised (abrupt→supervised head; wear-drift→normal-only/drift-chain complement per sibling T8; sensor-vs-process→parity-supervised head) with dose/shape ablation is the M0b test design; normal-only + drift-chain is the live fallback. No supervision magnitude asserted.

**Confidence:** Contested, no number (tripwire-2 leg under multi-source threat: DRA seen-bias, bias-effect >50% TPR drop, RedLamp diversity-gap/false-anomaly, fixed-augmentation generalization drop, TimeRCD shape-bias, Sim2Real gap witnesses; tripwire-1 leg directionally supported by STAND L3 + DevNet L1-abstract + Lau L3 mechanism — neither leg closable from literature).

**Evidence strength:** GRADE Moderate — 7 sources split: for margin leg: STAND supervised-beats-unsupervised direction (L3 R), DevNet 0.005–1% deviation learning (P directional), Lau minimax-optimal known+synthetic gains incl. unknown-DoS AUPR 0.345→0.793 non-temporal (L3 R, caveat kept); against unknown leg: DRA seen-vs-unseen bias (R abstract), Schmidl General Finding 2 supervised-not-superior (R), IJCAI21 bias-effect (P), RedLamp 2025 (P), fixed-augmentation drop (P).

**Key sources:** arxiv 2511.16145v1 · arxiv 2506.13955v1 · arxiv 1911.08623 + dev-network repo · arxiv 2203.14506 · timeeval evaluation page · doi 10.24963/ijcai.2021/456 · arxiv 2505.20765v1.

**Remaining uncertainty:** Missing leg U-1: temporal open-set precedent or M0b hedge-split read-off (wear-drift + SENSOR_VS_PROCESS absent from training, recall_unknown ≥ 0.30); synthetic dose interior optimum on sensor manifolds (U-6); sim-label transfer risk (TMech-2025 CSRA / JIM-2026 review witnesses, P).

**Alternative explanations:** (1) A1 caricature-overfit: S wins known but recall_unknown < 0.30 → memorized 4–7σ rectangulars. (2) A2 normal-only suffices: |S−N| < 3pp → adopt simpler N. (3) A3 dose poison: monotone-decreasing F1 in dose → any synthetic dose hurts.

**Would-be-overturned-by:** M0b battery: H1 FALSE iff F1_raw(S) − F1_raw(N) < 0.03 OR recall_unknown(S) < 0.30 on the pre-fixed held-out slice. Numeric tripwire: +3pp margin / 0.30 unknown-recall floor.

---

## TF7 [ESTABLISHED — estimator] Per-sensor median/IQR robust error norm prevents high-variance-sensor dominance

**Claim:** LOCK-2 restated: normalizing per-sensor errors with median/IQR rather than mean/std is outlier-robust and prevents high-variance channels drowning stable ones. ESTIMATOR choice only — says NOTHING about per-machine vs global scope (TF8 contested).

**Confidence:** High, 85–90% (L1 GDN Eq.12 inside raw-winning system + L3 official-docs semantics + L3 TSFM statistics ranking; H4-contra attacked scope, never the estimator).

**Evidence strength:** GRADE High — 4 sources: GDN §3.6 Eq.12 robust norm (L1 R); sklearn RobustScaler median/IQR + fit/transform separation (L3 R official); TSFM study mean/std-family beats min-max 25–45%, Lag-Llama median/IQR for robustness (L3 R, forecasting-task caveat); Lima & Souza variance-dominance mechanism (P).

**Key sources:** ar5iv 2106.06947 §3.6 · sklearn RobustScaler docs · ar5iv 2512.02833.

**Remaining uncertainty:** Scope leg (TF8); anomaly-robust fitter details under contamination (quantile breakdown at high fault density); backbone interaction (trees scale-invariant → estimator matters less).

**Alternative explanations:** (1) On tree/scale-invariant backbones the estimator choice washes out — adjudicated by the H4 cross-backbone interaction (M-vs-G under KNN AND tree AND GDN-light).

**Would-be-overturned-by:** Twin ablation where median/IQR vs mean/std error-norms differ by <0.5pp raw-F1 across ≥2 backbones AND worst-machine alert rates within ±1/1000 → estimator immaterial on twin data. Numeric tripwire: <0.5pp spread.

---

## TF8 [INCONCLUSIVE — open gap, no default winner from literature] Per-machine vs global normalization SCOPE

**Claim status:** Inconclusive (genuine open gap — zero direct-comparison papers found by supporting gap-pass AND 5-query contra sweep). Pipeline consequence (locked by this synthesis): interim ship-first default is per-machine per-sensor robust (median/IQR, RUN-normal-only, inside folds) with an explicit reversal tripwire — the safer default under heterogeneity, not an evidence verdict. See spec §4 for the default + tripwire.

**Confidence:** Contested leaning-unresolved, no number (closest call of the devil's run; G-side threats live but no wire fired).

**Evidence strength:** GRADE Low-Moderate — 6 sources, none direct: M-side: GDN robust precedent (mechanism, not scope), TSFM scope×statistic factorization + min-max-weak (L3), variance-dominance failure mode (P); G-side: TranAD §3.2 global mode-wise min-max VERIFIED Eq.1 feeding the SOTA line (R — preprocessing scope untouched by PA discount), RevIN scale-erasure mechanism (snippet), thin-pool ~200-step estimation-noise arithmetic (analogs), GDformer/MADDoC shared-structure transfer (P, representation not norm).

**Key sources:** pvldb vol15 p1201 Eq.1 (TranAD) · arxiv 2603.11869 (RevIN role) · arxiv 2602.17568 (Be-Wary) · ar5iv 2512.02833 · GLNMP hybrid-scope (fuse direction).

**Remaining uncertainty:** Missing leg U-4: ANY per-machine-robust vs global-min-max ablation, twin or bench; whether minority-class signal (C/ASM/RWK) is preserved or erased per scope; whether per-asset thresholds substitute for training-side scope (H4-A3).

**Alternative explanations:** (1) A1 global wins (G−M ≥ +2pp) → shared operating points dominate; per-machine norms overfit thin pools. (2) A2 backbone-erased (|M−G| < 2pp on trees, ≥+2pp on distance/deep) → scope matters only where the model reads scale. (3) A3 personalization absorbs scope (gap vanishes under per-asset thresholds).

**Would-be-overturned-by:** M0b M-vs-G ablation (≥2 backbones, inside-fold fitting, leakage test passing): H4 FALSE iff F1_raw(M) − F1_raw(G) < 0.02; STRONG falsification iff G − M ≥ +2pp → adopt G. Numeric tripwire: ±2pp signed wire.

---

## TF9 [ESTABLISHED — incumbency + design constraint; OUTCOME inconclusive] Thresholding: Pct is the incumbent; SPOT has a calibration pile + stationarity spec; the 3-arm winner is unrun

**Claim:** Two locked halves + one open outcome. (a) LOCK-3: validation-fit fixed-percentile (incl. max-validation, per-regime P95) is the published operating rule in SOTA systems AND documented production practice (threshold-to-budget-then-recall) — incumbency, not optimality. (b) LOCK-4: SPOT fits GPD on an n∼1000 calibration batch and assumes stationarity; DSPOT covers abrupt drift, not knee/wear drift; twin pools (~300 normals/episode) are sub-SPOT-scale by arithmetic. (c) Unresolved U-5: no study runs Pct-vs-POT-vs-GG on shared validation-only calibration — the M0b firing test decides; live devices: POT≈Pct collapse risk on the F1 leg, GG tax-without-gain branch.

**Confidence:** High, 85–89% for (a)+(b) (L1 GDN max-validation + TEP P95 + L1 SPOT design spec + production-pattern corroboration); Inconclusive, no number for (c).

**Evidence strength:** GRADE High for (a)+(b) — 7 sources: GDN max-validation (L1 R); TEP per-mode P95 Eq.7 (L3 R); SPOT KDD17 n∼1000 + stationarity (L1 P, HAL PoW-blocked logged miss); libspot "stationary only" + SCS/MACS quasi-stationary (P); threshold-to-budget practice J13 + 99.5th-pctile streaming + replay/shadow patterns (L4 P, quarantine observed); M2AD Gamma mechanics as GG-arm existence proof (P mechanics-only, +21% inadmissible). For (c): GRADE Low — zero shared-calibration 3-arm studies found.

**Key sources:** ar5iv 2106.06947 §3.6 · arxiv 2606.00052 Eq.7 · KDD17 SPOT · asiffer libspot guides · PMLR v258 (mechanics) · J13 Technolynx + TheCodeForge/Arun Baby/MAY2704.

**Remaining uncertainty:** Missing leg U-5 (M0b firing test); POT≈Pct collapse on F1 with budget-instability (H5-ambiguous vs survived); GG worst-machine edge numbers (U-6, mechanics only); EVT threshold-uncertainty at dozens-of-peaks.

**Alternative explanations:** (1) A1 simplest-wins outright (Pct keeps joint criterion) → machinery unjustified at twin scale. (2) A2 budget-dependent winner → ship budget-conditional calibration, not a single winner. (3) A3 GG tax without gain (GG≈Pct + ≥10× calibration compute) → dependence correction is formalism on twin-shaped (univariate-dominant) faults.

**Would-be-overturned-by:** M0b firing test (identical scores, validation-only fits, 20/150/5 budgets global + worst-machine): H5 FALSE (keep Pct) iff max(F1)−min(F1) < 0.02 AND max(alert)−min(alert) < 3/1000 at BOTH 20 and 150; H5-ambiguous iff winner differs per budget (ship budget-conditional). Numeric tripwire: 2pp F1 / 3-per-1000 alert spread, ≥2-of-3-budget stability.

---

## TF10 [ESTABLISHED — protocol] PA-on / overlap-TP / test-calibrated numbers are inadmissible as raw-F1 evidence

**Claim:** LOCK-6 restated for the training pipeline: gains measured under point-adjustment / overlap-TP / test-calibrated-threshold protocols (TranAD +17.06%, GDN-WADI +54%, USAD +0.096, M2AD +21%, tutorial F1s) transfer MECHANICS ONLY; numbers void against the F1 lock. M0b reports raw point-wise F1 (PA-off) primary; affiliation/VUS-PR/delay companions diagnostic-only; NO overlap-TP, NO PA%K, NO test-searched thresholds.

**Confidence:** High, 88–92% (L1 protocol forensics + L1 benchmark-validity + the H2-contra run itself as exhaustion counter-search; restates parent F1).

**Evidence strength:** GRADE High — 5 sources: TranAD PA-on/PA-off dual table (R); AAAI22 rigorous-eval PA pitfall + Threshold-Paradox 2026 test-calibration bias (P–R); Wu & Keogh flaw taxonomy (P); Schmidl threshold-agnostic General Finding 2 (R); MTAD-bench PA-dependence (KNN-best-raw vs deep-best-post-PA, L2).

**Key sources:** pvldb vol15 TranAD table · ojs.aaai 20680 · scitepress 143926 · arxiv 2009.13807 · ar5iv 2401.06175.

**Remaining uncertainty:** Which diagnostic companions (affiliation / VUS-PR / delay-to-detection / healthy-window rate) best complement raw-F1 operationally — admissibility is locked, companion selection is T-C6/H5 business.

**Alternative explanations:** (1) PA approximates event-utility and should be a co-primary — rebutted at parent level (random-scores-reach-SOTA: PA cannot discriminate methods even on its own terms). No live alternative inside this follow-up.

**Would-be-overturned-by:** Parent-level tripwire only (F1 of parent findings): published proof/replication with PA ranking preserving raw ordering (Spearman ρ ≥ 0.90, ≥10 methods, ≥2 datasets) AND random-control PA-F1 < 0.20. Numeric tripwire: ρ ≥ 0.90 + random < 0.20 — never observed.

---

*End — 10 findings: TF1/TF2 established-pipeline (TF2 Moderate-capped) · TF3/TF4/TF7/TF9ab/TF10 established-High · TF5/TF6 contested-no-number · TF8 inconclusive-open-gap · TF9c open-outcome. Every finding carries Confidence band + GRADE + count + Key sources + Remaining uncertainty + ≥1 Alternative + numeric overturn tripwire.*
