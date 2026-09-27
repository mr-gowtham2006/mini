# Training-preprocessing spec — follow-up 02 (THE deliverable, training-first)

Date: 2026-09-13 · Status: normative for the training pipeline; constrains generation (ordering principle, CONTEXT.md). Extends sibling `upgrade-spec.md` §5 + T5, never contradicts; extends parent Locked F1–F3, never contradicts. All numeric tripwires run PA-off on episode-seeded grouped CV. Twin facts: T=300, 1 sample/step, 7→15 channels, 32 machines, RUN 68.8% / STARVED 29.5% / DOWN 1.7%, 4–7σ rectangular caricatures, ~13% kit funnel, free sim labels, CPU-only, 2–4 students.

---

## §1. Problem formulation per fault family (incl. unknown-family slice)

| Family | Faults | Formulation | Train signal | Head / loss candidate |
|---|---|---|---|---|
| Abrupt | spike / drift / bias / delay / breakdown / quality | Supervised per-family (Arm S) vs normal-only (Arm N) head-to-head (H1) | Free sim labels; dose/shape via ablation (start n′=n+n⁻, Lau), never uniform-noise default | Deviation-loss/Z-score head (DevNet direction, P) or BCE scorer (STAND direction, L3); ≥3 dose levels |
| Wear-drift (slow-subtle) | P-WEAR-KNEE, post-knee drift/current/impulse couplings | Normal-only + drift-chain complement (sibling T8), NOT supervised-abrupt head | WSTATE>0.8, MAINT_EVENT resets; pre-knee train / post-knee test stratification | Complement scored wear-drift AC@1 ≥50% + ≥10pp gain bar (sibling T8) |
| Sensor-vs-process | S-DRIFT / S-DROPOUT vs P-DRIFT pairs | Parity-supervised head (separable ONLY via CH8/CH10 cross-check + reconstruction contribution) | Labelled mode taxonomy (sibling §4); SHF flags | Mode-accuracy scored alongside AC@1 |
| Cascade / multi-root | CASCADE_CHILD (root_id+hop, depth≤3), MULTI_ROOT (root_ids[]) | Supervised with joint-root-set scoring; effect-mode tags (observation-only vs physical-propagation) | Graph-distance complement arm (sibling H4 battery) | Subset AC@1 + hop-accuracy |
| Unknown-family slice (HELD OUT) | Wear-drift + SENSOR_VS_PROCESS episodes ABSENT from training, fixed before the run | Hedge split: recall_unknown(S) ≥ 0.30 required alongside +3pp known margin (H1 tripwire) | Never trained on; dose ablation must not touch it | Unknown-recall is a REPORTED bar, not a tuned one |

Fallback: if H1 falsifies, ship normal-only + drift-chain complement; keep free labels for window-label/back-labeling discipline only (T-C2 industry pattern: back-label failure→earliest-precursor + disposition feedback loop).

## §2. Architecture shortlist + selection criteria (H2-gated)

Shortlist (all share §3–§7 pipeline; differ only in scorer):
1. **Quantile baseline** (incumbent detector; M0b battery arm).
2. **KNN / PCA tripwires** (mandatory; wire-moving: max(KNN,PCA) sets the bar) + iForest / LOF / OLS controls (reported, never wire-moving).
3. **GDN-light** (graph arm; needs ≥10pp graph-ablation vs complete-graph next if it clears the wire).
4. **MP-discord guardrail** (M0b battery arm per MINIPRO-10).
5. **Deep successor arms** (USAD/TranAD-family mechanics only — numbers PA-voided): admitted one at a time, each gated identically.

Selection criteria (M0b battery, MINIPRO-10 bars + H-wires): raw-F1 ≥ 0.85; AC@1 ≥ 70% + ≥10pp graph ablation; flip < 40%; wall < 600s; 0-diverge ×5; shadow cross-partition; H2 wire +3pp over max(KNN,PCA); H1/H3/H4/H5 tripwires per §8. Delay + train-cost companions reported. Classical cost leg (OQ-7): timed on reference CPU; <3× ratio drops the cost rationale (gate unaffected).
CPU-only constraint: `torch.onnx.export(dynamo=True)` → ONNX Runtime CPUExecutionProvider + numeric parity assert (J08 direction); no GPU dependency anywhere.

## §3. Preprocessing pipeline (normative order)

**3.1 Cleaning.** Sync → SHF/dropout mask (DROPOUT windows EXCLUDED from scoring, logged for healthy-window replay; sibling §4) → MAINT_EVENT reset handling (pre-maintenance anomalous / post-maintenance normal window labels, I11) → transient filtering (start-up/changeover kept in a SEPARATE transient-inclusive healthy pool for precision replay, never in the normal-baseline fit). Imputation ONLY for random gaps, never stale-hold (Fleck direction, T-C3); stale-hold + parity-flag joint modeling is precedent-informed, not evidence-backed (review Gap 6).

**3.2 State handling (H3 co-default, STARVED-heavy regime).** All three ship to M0b, no default winner (TF5): Arm C single-with-state-covariate (RUN/STARVED/BLOCKED/DOWN categorical + DOWN-masking + regime-conditioned thresholds); Arm F filter-only (RUN-only train, naive score); Arm P per-state bank (STARVED model on STARVED-normal) + per-class (A/B/C/ASM/RWK) escalation arm. DOWN (~1.7%, ~160 machine-steps) masked throughout as unlearnable. STARVED-heavy subset (≥40% STARVED steps) is the deciding slice. Ablations: masking-with/without (OQ-8), thresholds-conditioned/unconditioned (≥1pp attribution expected, H3 prediction 3).

**3.3 Normalization (H4 inconclusive default — explicit).** SHIPS FIRST: per-machine per-sensor robust median/IQR, fit on RUN-normal-only INSIDE folds; test transformed, never refit (LOCK-2 estimator + TF1 discipline). REVERSAL TRIPWIRE (signed ±2pp): M−G < +2pp falsifies (adopt winner); G−M ≥ +2pp STRONGLY falsifies → adopt global-per-channel min-max. Rationale for M-first (not evidence): heterogeneity safety (class-scaled A/B/C/ASM/RWK dynamics; variance-dominance failure mode) with worst-machine companions to catch thin-pool noise. Run M-vs-G under ≥2 backbones (KNN + tree + GDN-light) + per-asset-threshold interaction (OQ-9). Log per-machine norm-estimation variance.

**3.4 Windowing + features.** Base tick 1 sample/step (sibling §5 anchor; never downsample fast derivations). Base window length from TWIN autocorrelation-hill/RobustPeriod analysis (OQ-10), NOT imported (5-vs-120 literature span is untransportable); fallback if no dominant period: multi-scale default {short, base, long}. Export 0.5/1.0/2.0-step-equivalent aggregates (mean, RMS, min/max, envelope-band energy with pre-registered GES2N-style statistic). Features: window stats + FFT/STFT + envelope-band energy + area-error (M2AD mechanics) + state/SHF/RC covariates + WSTATE/THERM where admitted. 50%-overlap FFT windows with purge/embargo = 1 window length.

**3.5 Splits + leakage rules (TF1 law).** Episode-seeded grouped CV (no window leakage across same-seed episodes); wear/maint/family stratification; overlap purge/embargo; ALL stats (norms, GMMs, POT fits, resampling) fit inside folds (`imblearn.pipeline` enforced); fit-train/transform-test; `leakage_unit_test.py` failing closed (episode-recovery ≤ chance+2pp; norm-refit detection; SMOTE-outside rejection).

**3.6 Imbalance (TF2 law).** Default no-resampling + class-weight/cost-first (`TunedThresholdClassifierCV`-style post-tuning, pos_weight-style weights) + per-class thresholds, never one global cut-off. SMOTE-family default-OFF; permitted ONLY via ablation, always inside `imblearn.pipeline`. Minority classes: post-knee wear + MULTI_ROOT + SENSOR_VS_PROCESS + kit-funnel classes. Promotion tripwire: minority-slice +2pp → default-ON for those families.

**3.7 Calibration (3-arm + incumbent).** Arms on identical scores, fit on CLEAN-EPISODE VALIDATION ONLY, never test-searched (TF9): Pct fixed-percentile/max-validation (incumbent, F1-locked home turf); POT/SPOT with pre-registered risk q (runs under open n∼1000-vs-~300 handicap + stationarity caveat; pooled-POT variant logged); GG GMM+Gamma dependence-correct (cost timed). Joint criterion: raw-F1 + alerts/1000 @20/150/5 global + worst-machine; stability ≥2-of-3 budgets; H5-ambiguous → budget-conditional calibration. Collapse wire: ΔF1 <2pp AND Δalert <3/1000 at both edges → keep Pct.

## §4. Evaluation protocol

Primary: raw point-wise F1, PA-off, fixed-percentile calibration (parent F1 Locked). Precision-side: healthy-window alert rate @20/150/5 per 1000, global + worst-machine, transient-inclusive pool (sibling T5 as extended: T5's 20/150/5 triple IS the reporting shape; per-machine worst-zone recording mandatory). Delay-aware diagnostics (never primary): delay-to-detection per family, affiliation/VUS-PR companions, salience/efficiency axes (T-C6). Forbidden: overlap-TP, PA%K, test-searched thresholds, PA-ranked architecture claims, vendor-magnitude ROI prose. Every detector ships with (budget, shadow log 2–6 wks + backtesting, suppression rules, tier routing, precision-side metrics) per T-C7; retrain evidence-triggered (metric/drift/MAINT_EVENT/new-regime), never calendar-only; drift tracked independent of model output (PSI/KS vs deploy baseline).

## §5. Generation requirements BACK-PROPAGATED to the twin (normative extensions to sibling upgrade-spec §5/T5)

Training constrains generation (ordering principle). Each item EXTENDS the sibling spec; conflicts resolve in training's favour (none intended — mapped below):

1. **Duty-cycle bound:** seeded episodes MUST cover the STARVED-heavy regime (≥40% STARVED steps) on bottleneck machines: minimum 25% of training episodes STARVED-heavy + 10% BLOCKED-visible, else H3's deciding slice is untestable. (Extends §5 wear-stratified splits with state-stratification.)
2. **Magnitude ladder (incl. 1–3σ incipient):** extend FAULT_RANGES with an incipient rung 1–3σ (below the 4–7σ caricature band) at ≥20% of fault injections, seeded and labelled; wear-knee drift (C3, γ=2.0σ at w=1) counts toward the ladder's low end. Without this, H1/H2 verdicts are caricature-conditional (H1-A1, H2-A2).
3. **Anomaly-rate budget:** cap injected fault mass so healthy-window replay pools stay calibratable: fault steps ≤8% of scored steps per episode; calibration-normals ~300/episode-channel retained as DIRECTIONAL floor (U10; sibling §5 wording kept, not hardened). (Gives T5's 20/150/5 reporting a pool it can measure.)
4. **Stratification keys + metadata the twin MUST export per episode/window:** `episode_id` (seed), `wear_endpoint` (max w_m), `maint_flag`, fault family + `mode` (observation-only/physical-propagation), `root_id`/`hop` or `root_ids[]`, sensor-vs-process mode label, per-machine state histogram (%RUN/STARVED/BLOCKED/DOWN), warm-up flag (§5.5), kit-funnel class counts. Without these keys TF1 stratification is unimplementable.
5. **Warm-up exclusion:** first 15 steps of every episode flagged warm-up (AR1/transient settle); excluded from normal-baseline fits AND calibration piles; INCLUDED in a separate transient-inclusive precision pool (sibling R3 direction).
6. **Partition-rebalancing for the 13% kit funnel:** oversample assembly/rework-completing trajectories (seeded AGV-priority + buffer-cap variants) until kits-completed ≥30/episode-batch median (vs current ~22–24 sunk from ~180 parts); log funnel metrics per batch. No architecture choice substitutes for missing kit examples (Recommendation 5 tradeoff).
7. **T9 mapping note:** the sibling spec has no T9; the "T9 duty-cycle/magnitude-ladder/rate-budget proposals" referenced in the task brief are read as §5-dataset-design + T5-precision-bar + T7-calibration-test + U10-normals-guidance jointly. Training-side justification: duty-cycle bound justifies H3's STARVED-heavy slice (TF5); ladder justifies H1-unknown + H2-A2 honesty (TF6/TF3); rate budget justifies T5-reportability + POT-peak viability (TF9); rebalancing justifies minority-slice imbalance claims (TF2). If sibling authors locate a literal T9 elsewhere, this list re-maps to it without content change.

## §6. CPU-only tooling shortlist (floor, not ceiling; every component justified vs student-team maintenance cost — contradictions-map Row 9)

Tracking/registry: MLflow tracking + registry (webhooks/tags + immutable bundles + gates + shadow/canary + rollback; trigger decoupled from data selection). Data: versioned Parquet + `scaler.pkl` + `window_config.json` + ingestion metadata (J16 artifact contract). Pipeline/leakage: scikit-learn `Pipeline` + `imblearn.pipeline` + `TunedThresholdClassifierCV`. Precision/shadow: replay harness + shadow 2–6 wks + backtesting + deterministic suppression BEFORE scoring + 3-tier routing. Export: PyTorch → ONNX CPU with parity assert. Refused ceiling: Feast-style feature store, Kubeflow, AutoML ("15 features, not 1500" — Row 9 Leaning-B). Sim2Real: twin keeps FAULT_RANGES randomization + seeded episode IDs for future pairing; no sim-pretrained promotion without paired-real validation (≥50 paired-real windows/channel, sibling §9.2).

## §7. Proposed Linear training-pipeline issue definition (MINIPRO style)

**Goal:** Freeze the training-first pipeline (splits/leakage/imbalance/gate law + co-default state arms + M-first norm with reversal wire + 3-arm calibration) and run the M0b training battery to fire H1–H5 tripwires, back-propagating generation requirements (§5) to the twin. Success = every H-wire adjudicated with a logged number; no PA-ranked claim; no unlocked numeric shipped as fact.

**Tasks:**
1. Write `train-preregistration.md` + `calibration-preregistration.md` (H1–H5 tripwires verbatim, seeds, arms, budgets 20/150/5, pre-registered POT-q/GMM configs) — owner: detector lead.
2. Implement pipeline §§3.1–3.7 (cleaning → state co-default arms → M-first norm switch → windowing/multi-scale → grouped splits + leakage unit test → weights-first imbalance → 3-arm calibration) — owner: detector lead.
3. Run M0b battery (quantile + GDN-light + MP guardrail + KNN/PCA/iForest/LOF/OLS; H1 N-vs-S + hedge slice; H3 C/F/P + per-class; H4 M-vs-G × backbones; H5 firing test) with delay + cost companions — owner: detector lead.
4. Export §5 generation-requirement deltas to the twin (ladder, duty-cycle, rebalancing, metadata keys, warm-up flags) as a sibling-spec extension note — owner: synthesis lead.
5. Log `m0b-train-runtable.csv` + `h5-calibration-runtable.csv` + shadow cross-partition results; update hypothesis registry (Falsified/Survived with date + battery ID) — owner: detector lead.

**Files:** `src/train_pipeline.py` (new), `src/calibration.py` (new), `tests/leakage_unit_test.py` (new), `train-preregistration.md`, `calibration-preregistration.md`, `m0b-train-runtable.csv`, `h5-calibration-runtable.csv`, sibling `upgrade-spec.md` §5 extension note.

**Tests:** leakage unit test failing closed (TF1 tripwire); H1 +3pp/0.30 wire; H2 +3pp/0.60 wire; H3 +2pp/>0/+2pp-A1 wires; H4 ±2pp signed wire; H5 2pp/3-per-1000/≥2-of-3-stability wire; M0b bars (F1≥0.85, AC@1≥70% + ≥10pp ablation, flip<40%, <600s, 0-diverge ×5, shadow cross-partition).

**Exit criteria:** all five H-wires adjudicated (Survived/Falsified/Ambiguous H5) with logged numbers; pipeline defaults locked (state rung, norm scope, calibration rule incl. budget-conditional branch); §5 generation deltas filed to sibling; registry updated, not rewritten. If max(raw-F1) < 0.60 → exit as battery-inconclusive, escalate §5 ladder/rebalancing to P0 (H2-A2 branch).

---

*End — training-preprocessing spec: per-family formulation + hedge slice · 5-arm gated shortlist · 7-stage pipeline (co-default state, M-first norm + wire, grouped splits, weights-first, 3-arm calibration) · raw-F1 + 20–150/1000 + delay-aware protocol · 7 back-propagated generation requirements · CPU-only tooling floor · MINIPRO-style Linear issue definition.*
