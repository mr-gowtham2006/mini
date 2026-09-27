# H5 — Thresholding-arm selection: fixed-percentile vs POT/SPOT vs GMM+Gamma (joint raw-F1 + 20–150/1000 criterion)

Seed: contradictions-map Row 3 (threshold structure; Unresolved — i.i.d. validation-max vs GPD exceedances vs dependent per-sensor p-values) · Theme T-C6 · Gap 4 · upgrade-spec T7 · Date: 2026-09-13 · Status: Inconclusive (NOT falsified after 5-query devil's sweep 2026-09-13 — POT≈Pct collapse risk + GG-payoff doubt live, M²AD disclosed pro-GG; awaits M0b firing test) · Priority: P1

## Claim
The three calibration arms (fixed-percentile / POT-SPOT / GMM+Gamma) are DISTINGUISHABLE on twin data, and the joint criterion (raw-F1 + precision-side healthy-window alert rate at the 20–150/1000 range) selects a budget-stable winner — i.e. threshold choice is a live decision, not a wash.

## Null (H0)
Arms collapse: all three within ±2pp raw-F1 AND within ±3/1000 alert rate of each other — threshold machinery is interchangeable on twin data and the simplest arm stands by default.

## Falsification criterion + numeric tripwire (runnable on M0b battery / training runs)
Arms (identical detector scores; thresholds fit on CLEAN-EPISODE VALIDATION ONLY; never test-searched; NO overlap-TP, NO PA%K):
- Arm Pct: fixed-percentile / max-validation (simplest, F1-locked baseline).
- Arm POT: POT/SPOT EVT with pre-registered risk q (stationarity assumed; DSPOT covers abrupt drift, not knee-drift).
- Arm GG: GMM+Gamma dependence-correct multi-sensor calibration (per-sensor p-values, χ²-miscalibration corrected).
Joint criterion: raw point-wise F1 (PA-off) + healthy-window alert rate measured at 20, 150 (edges) + 5 (stability) per 1000, global + worst-machine (T5 shape).

TRIPWIRE — H5 is FALSE (arms indistinguishable → keep Pct, no further threshold work) iff ALL hold:
1. `max(F1) − min(F1) < 0.02` across the three arms, AND
2. `max(alertRate) − min(alertRate) < 3/1000` at BOTH 20 and 150 budgets.
H5 survives if EITHER spread trips (F1 spread ≥ 2pp OR alert-rate spread ≥ 3/1000 at either budget edge) AND the winning arm is stable across ≥2 of the 3 budgets (20/150/5); an unstable "winner" (different arm per budget) is recorded as H5-ambiguous, not H5-survived.

## Predictions
1. Arms separate on the precision side even if F1 is close: GG shows the lowest worst-machine alert rate (dependence correction pays off on 32-machine joint scoring); POT shows the highest variance across budgets (stationarity vs wear-drift tension, Gap 4).
2. Pct is competitive on raw-F1 (F1-locked home turf) but loses the joint criterion on worst-machine rates.
3. POT degrades specifically on post-knee wear-drift episodes (EVT stationarity tripwire): POT-vs-Pct gap on the wear-drift subset exceeds the gap on abrupt-fault episodes.

## Alternatives + distinguisher
- A1 (simplest wins outright): Pct wins or ties the joint criterion → machinery unjustified at twin scale. DISTINGUISHER: the joint criterion itself — A1 is the H0 branch with Pct as the retained default.
- A2 (budget-dependent winner): different arms win at 20 vs 150 → no single threshold rule; calibration becomes budget-conditional. DISTINGUISHER: the ≥2-of-3-budgets stability check — A2 is exactly the H5-ambiguous branch, resolved by shipping budget-conditional calibration, not a single winner.
- A3 (GG tax without gain): GG matches Pct within the wire but costs ≥10× calibration compute → dependence correction is formalism without payoff on twin data. DISTINGUISHER: calibration-cost companion alongside the wire; A3 fires when the wire holds AND cost(GG)/cost(Pct) ≥ 10.

## Verification method
M0b calibration firing test (upgrade-spec T7): three arms on clean-episode validation fits, scored on fixed test episodes; raw-F1 + alerts/1000 at 20/150/5 (global + worst-machine); wear-drift vs abrupt subset split for Prediction 3; calibration-cost timing companion. Pre-register risk q (POT) and GMM/Gamma configs before the run.

## Expected outcome
H5 survives with a budget-stable winner on the joint criterion (GG favored on worst-machine precision side, Pct retained on raw-F1-only reads), OR resolves to H5-ambiguous (budget-conditional calibration) — either way the threshold rule is evidence-backed. Clean H0 collapse keeps fixed-percentile and closes Gap 4 with the cheapest answer.

*Status: Inconclusive — NOT falsified 2026-09-13 (H5-contra collapse devices live, kill not achieved). Awaits M0b calibration firing test.*
