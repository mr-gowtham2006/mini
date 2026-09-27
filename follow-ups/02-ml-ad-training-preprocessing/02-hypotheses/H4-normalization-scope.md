# H4 — Per-machine vs global normalization decided by ablation (open gap)

Seed: contradictions-map Row 4 (per-machine robust vs global min-max; Unresolved, genuine open gap — zero retrieved papers compare them) · Gap 1 (Q-C) · Date: 2026-09-13 · Status: Inconclusive (NOT falsified after 5-query devil's sweep 2026-09-13 — closest call, zero direct-comparison papers; G-side threats live, awaits M0b ablation) · Priority: P1

## Claim
Per-machine (per-sensor robust: median/IQR, fit on RUN-normal-only inside folds) normalization beats global-per-channel min-max (fit on train stats) on twin M0b battery raw-F1 by a material margin — i.e. the licensed interim default is also the ablation winner.

## Null (H0)
Normalization scope is a wash or reversed: per-machine − global raw-F1 < +2pp (tie within noise, or global wins).

## Falsification criterion + numeric tripwire (runnable on M0b battery / training runs)
Arms (identical model, features, windows, calibration, splits; ONLY the norm scope varies; ALL norm stats fit inside folds on RUN-normal-only; test transformed, never refit):
- Arm M: per-machine per-sensor robust (median/IQR).
- Arm G: global-per-channel min-max on train stats.
- Same fixed-percentile calibration on clean-episode validation; raw point-wise F1 primary (PA-off); worst-machine alert rate companion (scope effects concentrate per-machine).

TRIPWIRE (direction + magnitude) — H4 is FALSE if:
`F1_raw(M) − F1_raw(G) < 0.02` (under +2pp).
Survival requires M − G ≥ +2pp. A reversed outcome (G − M ≥ +2pp) falsifies H4 STRONGLY and promotes G to the pipeline default.

## Predictions
1. M wins by ≥ +2pp: twin machines span classes (A/B/C/ASM/RWK) with class-scaled dynamics, so global min-max compresses small-class signals.
2. The gap concentrates on minority classes (C/ASM/RWK) and worst-machine alert rates, not the global mean — scope is a heterogeneity treatment.
3. Result is backbone-invariant in sign (holds for both classical and deep backbones under the H2 gate), though magnitude may shrink for scale-invariant models (trees).

## Alternatives + distinguisher
- A1 (global wins): G − M ≥ +2pp → shared operating points dominate; per-machine norms overfit thin per-machine normal pools. DISTINGUISHER: the signed tripwire — A1 is the strong-falsification branch; on firing, pipeline adopts G.
- A2 (backbone-erased): |M − G| < 2pp on scale-invariant backbones but ≥ +2pp on distance/deep ones → scope matters only where the model reads scale. DISTINGUISHER: cross-backbone interaction (run M-vs-G under KNN AND tree AND GDN-light arms); A2 predicts a backbone × scope interaction, H4 predicts sign-consistency.
- A3 (threshold personalization absorbs scope): with per-asset thresholds, M ≈ G within ±2pp → inference-side personalization substitutes for training-side scope. DISTINGUISHER: M-vs-G ablation WITH and WITHOUT per-asset thresholds; A3 predicts the gap vanishes under personalization.

## Verification method
M0b ablation: M-vs-G under ≥2 backbones (classical tripwire arm + primary arm), episode-seeded grouped CV, inside-fold norm fitting (leakage unit test must pass), raw-F1 primary + worst-machine alerts/1000 companion. Interim default until run: M (per-sensor robust, RUN-normal-only).

## Expected outcome
H4 survives with a heterogeneity-concentrated margin (M − G ≥ +2pp, largest on C/ASM/RWK), locking M as the pipeline default. If falsified, the pipeline adopts the winner (G on strong falsification; backbone-conditional rule on A2; personalization-substitution on A3) — any branch resolves Gap 1 either way.

*Status: Inconclusive — NOT falsified 2026-09-13 (H4-contra closest call; open gap confirmed). Awaits M0b ablation.*
