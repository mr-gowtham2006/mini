# H3 — State-as-covariate single model beats filter-only and per-state models (STARVED-heavy tripwire)

Seed: contradictions-map Row 6 (state ladder; Leaning-B ladder, covariate interim) · Theme T-C3 · Date: 2026-09-13 · Status: Contested (tripwire-2 leg provisionally falsified 2026-09-13 by per-mode precedents; covariate-vs-filter leg survives; per-state co-default until M0b) · Priority: P1

## Claim
A single model with machine state as covariate (+ DOWN-masking: DOWN steps excluded from normal-baseline training and masked at scoring) beats BOTH (a) filter-only (RUN-only baseline, STARVED scored naively) AND (b) per-state models, on STARVED-heavy episodes — the regime that decides the issue given measured 29.5% STARVED.

## Null (H0)
State-as-covariate adds nothing: its raw-F1 on STARVED-heavy episodes is within noise of filter-only or of per-state models.

## Falsification criterion + numeric tripwire (runnable on M0b battery / training runs)
Arms (identical channels/features/windows/norms; episode-seeded grouped CV; norms fit on RUN-normal-only inside folds; PA-off):
- Arm C (covariate): single model, state (RUN/STARVED/BLOCKED/DOWN) as categorical input + DOWN-masking + regime-conditioned thresholds.
- Arm F (filter-only): train RUN-only, score all states naively.
- Arm P (per-state): separate models per state (STARVED model trained on STARVED-normal).
- Subset: STARVED-heavy episodes = episodes with ≥40% STARVED machine-steps (STARVED is 29.5% overall, so this isolates the deciding regime); DOWN (~1.7%, ~160 machine-steps) masked throughout as unlearnable.

TRIPWIRE — H3 is FALSE if EITHER fires on the STARVED-heavy subset:
1. `F1_raw(C) − F1_raw(F) < 0.02` (under +2pp vs filter-only), or
2. `F1_raw(C) − F1_raw(P) < 0.00` (loses to or ties per-state).
H3 survives only if C beats F by ≥ +2pp AND beats-or-ties P strictly above 0 (C > P).

## Predictions
1. F collapses on STARVED-heavy episodes (STARVED −2σ offsets scored against a RUN baseline → FP flood); C recovers ≥ +2pp via the covariate + regime-conditioned thresholds.
2. P underperforms C on STARVED-heavy (per-state STARVED model is data-thin and misses cross-state context); gap C − P > 0 but smaller than C − F.
3. Regime-conditioned thresholds contribute independently: ablating them (C without conditioned thresholds) costs ≥ +1pp, implicating the threshold leg, not just the feature leg.

## Alternatives + distinguisher
- A1 (per-class wins where heterogeneity is large): per-class models (A/B/C/ASM/RWK) beat single-model C by ≥ +2pp on the same subset → heterogeneity scale defeats covariate capacity. DISTINGUISHER: per-class arm (run alongside P) — if per-class − C ≥ +2pp, H3 is falsified in favor of A1 and the ladder moves one rung up.
- A2 (filtering suffices): F matches C within ±2pp → STARVED offsets are harmless under robust norms. DISTINGUISHER: tripwire-1 itself; on firing, adopt the simpler filter rule.
- A3 (DOWN-masking does the work): C-without-masking ≈ F, C-with-masking wins → the mask, not the covariate, is the active ingredient. DISTINGUISHER: masking ablation inside arm C.

## Verification method
M0b training runs on the STARVED-heavy episode subset + full-episode companions: fixed arms C/F/P (+ per-class A1 arm, + masking/threshold ablations), episode-seeded grouped CV, raw-F1 primary, healthy-window alert rate (20/150/5) companion to catch STARVED-FP flooding. H2 gate applies to deep backbones.

## Expected outcome
H3 survives on the STARVED-heavy subset (covariate + DOWN-masking as interim pipeline rule), with A1 flagged as the live escalation if per-class gaps appear on heterogeneous classes. If falsified via tripwire-1, pipeline keeps filter-only + regime-conditioned thresholds; if via tripwire-2/A1, pipeline moves to per-class models.

*Status: Contested — tripwire-2 leg provisionally falsified 2026-09-13 (H3-contra C1+C2 per-regime precedents; covariate leg alive via S1–S3). Per-state co-default until M0b.*
