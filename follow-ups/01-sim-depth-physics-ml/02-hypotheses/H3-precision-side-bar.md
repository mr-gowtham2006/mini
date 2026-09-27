# H3 — Precision-side bar alongside F1

Status: Contested · Source: R3 (SIM_SPEC §§4–5/A-S4 vs C05/C06/I12 + EEMUA 191 I23/I24) / G4 / H-seed-d · Date: 2026-09-13 · Updated 2026-09-13 per claim-lock annex (10/1000 reading provisionally refuted R2; survives as 20–150/1000 range; practice existence locked L4)

## Claim
A precision-side bar — alerts per 1000 healthy windows, measured on fault-free replay segments of the frozen twin, with VETO_ASM2 + flip-gate + chain-cards as the precision mechanisms — demotes at least one sensitivity-tuned detector setting that raw point-wise F1 ranking would keep: some setting in the top quartile by raw-F1 exceeds the pre-registered budget of >10 alerts/1000 healthy windows and is therefore rejected despite its F1 rank.

## Origin
R3 contradiction: lab loss (sensitivity-first, 4–7σ mags + quantile catch rate) vs plant loss (trusted-alert rate under flood risk; "85% accurate" models get muted; realistic ceiling FPR ~9%/FNR ~12%, A-S4; EEMUA ≈1 alarm/10 min). Balance Leaning-B → spec needs a precision-side bar alongside (not replacing) Locked F1 raw-F1. Precision mechanisms exist (veto/flip-gate/chain-cards) but are unmeasured.

## Falsification Criteria
Runnable on the frozen M0b battery + fault-free healthy-window replay:
1. Sweep detector settings (threshold/Q_DET variants); rank by raw-F1 (PA-off, fixed-percentile).
2. For each setting, count alerts/1000 healthy windows on fault-free segments.
3. Tripwire: zero top-quartile-F1 settings exceed 10 alerts/1000 → H3 false.
4. Robustness check: rank-reversal must persist at neighboring budgets 5 and 20/1000 for at least one demotion, else the demotion is threshold-artifact.

## Predictions (must observe if true)
- P1: ≥1 top-quartile-F1 setting breaches 10/1000 and is demoted; the kept setting has lower raw-F1 but meets budget.
- P2: Enabling VETO_ASM2 + flip-gate measurably cuts the healthy-window rate vs veto-off control on identical settings.
- P3: 4–7σ mag sensitivity tuning is part of the breach cause: relaxing mags or tightening Q_DET trades F1 rank for budget compliance.

## Alternative Explanations
- A1: Budget miscalibration — demotion is an artifact of the 10/1000 threshold, not plant trust. Distinguish: pre-registered budget + 5/20 sensitivity check (criterion 4); demotion surviving ≥2 of 3 budgets = real rank-reversal.
- A2: Healthy segments are too clean (no start-up/changeover transients, cf. C05/C06/I12) so the rate is underestimated and demotions undercount. Distinguish: include start-up/changeover windows in the healthy pool; if demotions only appear with them, scope the bar to transient-inclusive replay.

## Priority
P1 — does not gate physics, but gates which detector settings ship.

## Status
Contested — 10/1000 budget provisionally falsified as plant-grounded (tolerance 20–150/1000, steady-state-compliance + artifact mechanics); rate-budget practice itself locked. Battery must test 20–150/1000 stability.
