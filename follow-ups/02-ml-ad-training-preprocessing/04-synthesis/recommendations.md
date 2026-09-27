# Recommendations — follow-up 02: ML AD training + preprocessing

Date: 2026-09-13 · Each block: Decision + Tradeoffs (numbers/sources) + verb-led Next actions (verb + artifact + owner) + numeric Drop condition. Battery-scoped only; no platform/ROI framing (parent F2 Locked); raw-F1-only scoring, PA-off (parent F1 Locked); extends parent F1–F3 and sibling upgrade-spec, never contradicts.

---

## Recommendation 1: Freeze the leakage + imbalance + gate rules as non-negotiable pipeline law before any training run

**Decision:** Choose episode-seeded grouped CV + inside-fold everything (TF1) + weights-first/SMOTE-default-OFF (TF2) + classical-tripwire gate on every deep arm (TF3) as frozen law in the training-pipeline issue, over letting each experiment pick its own splits/handling/baselines.

**Tradeoffs:** (1) Grouped splits + inside-fold fitting cost ~15–25% usable-data overhead vs window-random splits (thin pools: ~200 RUN-normal steps/machine/episode) — but window-random leaks episode identity and SMOTE-before-split over-optimism is measured (+2.6pp phantom: 0.724 vs 0.698, imbalanced-learn official docs), so the "cheap" alternative buys inflated numbers. (2) Running KNN/PCA/iForest/LOF/OLS on every deep comparison ~doubles battery compute — but without the gate any deep win is PA-suspicious by default (TRANAD +17%, GDN +54%, USAD +0.096 all PA-voided), and classicals are CPU-cheap (cost leg OQ-7 to confirm).

**Next actions:**
- Write `train-preregistration.md` (seed list, grouped-fold scheme, purge length = 1 window, inside-fold checklist, H1–H5 tripwires verbatim, PA-off lock) — owner: detector lead.
- Implement `leakage_unit_test.py` (episode-ID recovery ≤ chance+2pp; norm-refit detection; SMOTE-outside-pipeline rejection) failing closed in CI — owner: detector lead.
- Log every run to `m0b-train-runtable.csv` (arms, seeds, raw-F1, alerts/1000 @20/150/5, delay, wall-clock) — owner: detector lead.

**Drop condition:** If grouped-vs-random ablation shows |ΔF1_raw| < 0.01 AND episode-recovery ≤ chance +2pp on twin data → relax grouping to random+purge (TF1 tripwire); if SMOTE-inside ablation gains ≥+2pp minority-slice → promote SMOTE to default-ON for those families (TF2 tripwire). Until either fires, the law stands.

---

## Recommendation 2: Ship BOTH state arms to M0b with no default winner — choose co-default over covariate-first

**Decision:** Choose per-state co-default (single-with-covariate Arm C AND per-state bank Arm P AND per-class escalation arm, all + DOWN-masking + regime-conditioned thresholds) over the H3-file's covariate-interim framing, because tripwire-2 is provisionally falsified by per-regime 100%-vs-22.2% precedents (TF5).

**Tradeoffs:** (1) Three state arms × ablations (masking, threshold-conditioning) cost ~3× the state-handling battery — but picking covariate-first now risks a 77.8pp-scale blind-spot miss (product-aware stress: global 22.2% vs per-mode 100%, Islam & Carden L3) on exactly the STARVED-heavy regime that is 29.5% of twin steps. (2) Per-state STARVED models train on thin pools (~200 steps/machine/episode → high-variance medians/IQRs) — the covariate arm hedges precisely this; co-default is the hedge made explicit.

**Next actions:**
- Implement arms C/F/P + per-class arm + masking/threshold ablations on the STARVED-heavy (≥40%) subset — owner: detector lead.
- Record healthy-window alert rates @20/150/5 to catch STARVED-FP flooding — owner: detector lead.
- Pre-register the three-way tripwire (C−F ≥2pp AND C−P >0 survive; per-class−C ≥2pp escalates) in `train-preregistration.md` — owner: synthesis lead.

**Drop condition:** C−F < 0.02 → drop covariate, keep filter+thresholds; C−P ≤ 0 → drop covariate-default, promote per-state bank; per-class−C ≥ +2pp → escalate to per-class models (H3-A1). Any branch resolves TF5 either way.

---

## Recommendation 3: Adopt per-machine-robust as the ship-first norm with a signed reversal wire — choose explicit safer-default over evidence-claim

**Decision:** Choose interim default M (per-machine per-sensor median/IQR, RUN-normal-only, inside folds) + mandatory M-vs-G ablation across ≥2 backbones with worst-machine companions, over claiming the literature decides scope (it does not — TF8 open gap, zero direct papers).

**Tradeoffs:** (1) Per-machine fitting on ~200-step pools adds estimator noise vs 32×-larger global pools (H4-C3 arithmetic) and risks scale-erasure of 4–7σ magnitude events (TranAD's global min-max feeds the SOTA line, VERIFIED Eq.1) — the reversal wire exists for exactly this: G−M ≥ +2pp promotes G with no surprise. (2) Running M-vs-G under KNN + tree + GDN-light triples norm-ablation cost — but scope×backbone interaction is the empirical norm (MTSC study), so a single-backbone verdict would be backbone-lore, not a scope verdict.

**Next actions:**
- Implement norm-scope switch (`per-machine-robust | global-minmax`) with inside-fold fitting + leakage-test hook — owner: detector lead.
- Run M-vs-G under classical + primary arms; log per-machine norm-estimation variance + worst-machine alerts/1000 — owner: detector lead.
- Add the per-asset-threshold interaction (OQ-9) as a second-factor arm — owner: detector lead.

**Drop condition:** M−G < +2pp → H4 false, adopt winner; G−M ≥ +2pp → STRONG falsification, adopt G as default; |M−G| < 2pp on trees but ≥+2pp on distance/deep → backbone-conditional rule (H4-A2); gap vanishes under personalization → personalization-first rule (H4-A3).

---

## Recommendation 4: Run the 3-arm calibration firing test with the joint criterion — choose distinguish-or-drop over threshold-theology

**Decision:** Choose the H5 firing test (Pct vs POT/SPOT with pre-registered q vs GMM+Gamma; identical scores; validation-only fits; raw-F1 + alerts/1000 @20/150/5 global + worst-machine; ≥2-of-3 stability; cost timing) over adopting any single threshold method on principle, because incumbency (Pct) and design-spec handicap (POT n∼1000 vs ~300 pools) are locked but the winner is not (TF9).

**Tradeoffs:** (1) Three calibration arms + pooled-POT variant + wear-drift-vs-abrupt subset split cost ~4× calibration work — but POT≈Pct collapse (C1) and GG-tax-without-gain (C4/A3) are both live, and only the joint criterion separates "competitive on F1, loses on worst-machine rate" (H5 prediction 2). (2) Pre-registering POT's risk q and GMM configs forbids post-hoc q-shopping — Threshold-Paradox 2026 shows calibration-data choice dominates method choice, so unregistered q is a second threshold-theology.

**Next actions:**
- Implement three calibration arms + pooled-POT variant with pre-registered configs in `calibration-preregistration.md` — owner: detector lead.
- Score on fixed test episodes; log raw-F1 + alerts/1000 @20/150/5 (global + worst-machine) + calibration wall-clock to `h5-calibration-runtable.csv` — owner: detector lead.
- Convert the SPOT HAL PDF to `.md` and re-verify spec numbers before quoting beyond n∼1000/stationarity (annex field-check P→R) — owner: evidence curator.

**Drop condition:** Spreads inside wire (ΔF1 <2pp AND Δalert <3/1000 at both budgets) → H5 false, keep Pct, close Gap 4 cheap; winner differs per budget → H5-ambiguous, ship budget-conditional calibration; cost(GG)/cost(Pct) ≥ 10 with wire held → A3, drop GG.

---

## Recommendation 5: Back-propagate the training-dictated generation requirements to the twin NOW — choose training-constrains-generation over parallel drift

**Decision:** Choose to issue the §9 generation-requirements list in `training-preprocessing-spec.md` (duty-cycle bound, 1–3σ magnitude ladder, anomaly-rate budget, stratification keys + metadata export, warm-up exclusion, partition-rebalancing for the 13% funnel) as normative constraints on sibling upgrade-spec §5/T5, extending (never contradicting) it — per the user-mandated ordering principle.

**Tradeoffs:** (1) Duty-cycle floors + magnitude-ladder + rebalancing force twin re-runs (seeded episodes with incipient faults are new simulation work, ~+4% multi-scale export already budgeted) — but without 1–3σ faults every supervised-vs-normal verdict is caricature-conditional (H1-A1), and without funnel rebalancing the kit classes have ~22 sunk examples total: no threshold or architecture choice can conjure data that was never generated. (2) Exporting stratification metadata (episode_id, wear_endpoint, maint_flag, fault family/mode, state histograms, warm-up flags) costs schema work — but episode-seeded + wear-stratified splits (TF1) are unimplementable without it.

**Next actions:**
- Append the training-dictated requirements to sibling upgrade-spec §5 as an extension note (duty-cycle bound, ladder, budget, keys, warm-up, rebalancing) — owner: synthesis lead.
- Freeze the unknown-family holdout definition (wear-drift + SENSOR_VS_PROCESS absent from training) before generation re-runs — owner: detector lead.
- File the Linear training-pipeline issue per the spec's MINIPRO-style definition — owner: synthesis lead.

**Drop condition:** If M0b on current-generation data returns max(raw-F1) ≥ 0.85 with unknown-recall ≥ 0.30 (all wires clear without new generation) → downgrade ladder/rebalancing to P2 nice-to-have; if max(raw-F1) < 0.60 (H2-A2 battery-inconclusive) → escalate generation requirements to P0 blockers, no further training tuning until the ladder ships.

---

*End — 5 recommendations, each with Decision + Tradeoffs (numbers + sources) + Next actions (verb + artifact + owner) + numeric Drop condition. No platform/ROI framing; no PA rankings; all inside the follow-up scope.*
