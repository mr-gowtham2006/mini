# H2 — Classical-tripwire gate: KNN/PCA raw must be beaten before any deep claim

Seed: contradictions-map Row 1 (GDN vs KNN; Unresolved, dataset×protocol-specificity) · Theme T-C1 · Gap 9 · Date: 2026-09-13 · Status: Inconclusive (NOT falsified after 5-query devil's sweep 2026-09-13 — PA rows voided, gate holds, awaits M0b read-off) · Priority: P0 (gate)

## Claim
On twin M0b battery data, classical baselines (KNN, PCA; iForest/LOF/OLS as controls) set the bar: NO deep-detector claim (GDN-light or any successor) is admitted unless the deep model beats the best classical raw-F1 by a material margin under identical calibration and splits.

## Null (H0)
Deep needs no gate — i.e. GDN-light's published raw numbers (SWaT 0.81 / WADI 0.57 / SMD 0.529) are assumed to transfer to the twin without a same-battery classical comparison.

## Falsification criterion + numeric tripwire (runnable on M0b battery)
Battery: quantile + GDN-light + MP-discord guardrail + KNN/PCA/iForest/LOF/OLS tripwires; identical episode-seeded grouped CV, identical features/windows, fixed-percentile calibration on clean-episode validation, raw point-wise F1 primary (PA-off), delay + train-cost as companions.

TRIPWIRE — H2 (the gate) is FALSE — i.e. the gate is CLEARED and the deep claim proceeds — iff:
`F1_raw(GDN-light) − max(F1_raw(KNN), F1_raw(PCA)) ≥ 0.03` (+3pp).
Otherwise the gate HOLDS: H2 survives, the deep claim is rejected/demoted, and the classical winner is the standing detector until a deep arm clears the wire. (Controls iForest/LOF/OLS reported; they do not move the wire.)

## Predictions
1. KNN and/or PCA land within ±3pp of — or above — GDN-light raw on twin data (twin fault mix is caricature-rectangular, the regime where simple distance/reconstruction baselines are strongest).
2. Any PA-on control flips or compresses the deep-vs-classical gap (protocol-dependence check per F1 lock) — reported, not ranked.
3. Train-cost companion favors classical by ≥1 order of magnitude on CPU (gate has a cost leg, not just F1).

## Alternatives + distinguisher
- A1 (twin is WADI-easy for GNNs): GDN-light clears the wire by ≥ +3pp → the twin's fault mix rewards learned graphs (GDN 0.81-SWaT precedent transfers). DISTINGUISHER: the tripwire itself — A1 is exactly H2-false; on clearing, H2 is marked Falsified and the graph-ablation leg (≥10pp) becomes the next hurdle.
- A2 (both fail): ALL arms < 0.60 raw-F1 → the battery/fault mix is uninformative, not a classical win. DISTINGUISHER: absolute bar check — if max(raw-F1) < 0.60, verdict is "battery inconclusive," H2 stays Active (gate untested), and the fault-magnitude ladder (1–3σ incipient) is the back-propagated requirement to generation.

## Verification method
Same M0b battery run as the MINIPRO-10 decision apparatus: pre-registered arms, seeded replay, shadow cross-partition, delay + train-cost companions. No separate experiment — H2 is a gating read-off from the shared battery.

## Expected outcome
Gate HOLDS (H2 survives): classical matches or beats GDN-light raw on twin data, forcing every downstream deep reading (H1/H3/H4/H5 deep arms) to report its classical delta. If cleared instead, H2 is falsified cleanly and the program pivots to graph-ablation-first evaluation.

*Status: Inconclusive — gate holds after 5-query devil's sweep 2026-09-13 (H2-contra: all hits PA-voided). Read off the M0b battery.*
