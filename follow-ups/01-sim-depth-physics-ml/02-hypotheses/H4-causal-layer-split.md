# H4 — Causal-layer split (PCMCI+ for abrupt, graph-distance for wear drift)

Status: Contested · Source: R4 (parent-F4/A-S1/A-S3/I28 vs A-S7 + A-S1-limits + A-S4 §VII) / G3 / H-seed-c · Date: 2026-09-13 · Updated 2026-09-13 per claim-lock annex (regime split locked L3; P3 graph-free-control riskiest; complement win unresolved U6)

## Claim
Causal-layer split for the 32-machine twin: keep topology-masked PCMCI+ (tau=2) + flip-gate for abrupt/propagating faults, and add a graph-change-distance complement (reference-graph Jaccard-style distance, A-S7 direction) for wear-drift (T3) chains. On the frozen M0b battery: PCMCI+-alone AC@1 (depth≤3, all seeds) on the wear-drift subset is <50%, the split arm recovers ≥50% on that subset with a ≥10pp gain over PCMCI+-alone, and the abrupt/propagating subset regresses by <10pp vs PCMCI+-alone.

## Origin
R4 regime mismatch: masked lagged discovery assumes informative per-step transitions; wear drift (slow, predictable, nonstationary across the knee) violates exactly that — A-S7's adapted-FCI + Jaccard-vs-reference-graph is a direct mechanism match to the T3 regime, compounding F4's own prior-fragility flag. I28 validates walk+mask architecture but not PCMCI-for-drift. Resolution is a falsifiable split, pressuring conditional F4 only (no Locked F1–F3 tension).

## Falsification Criteria
Runnable on the frozen M0b battery with fault-subset scoring (wear-drift vs abrupt/propagating), three arms (PCMCI+-alone, complement-alone, split) + graph-free control (BARO-style/no-prior walk), identical seeds:
1. Tripwire A: PCMCI+-alone wear-drift AC@1 ≥50% → split unnecessary → H4 false.
2. Tripwire B: (split − PCMCI+-alone) wear-drift AC@1 <10pp → complement adds nothing → H4 false.
3. Tripwire C: split abrupt-subset AC@1 drops ≥10pp vs PCMCI+-alone → split remedy rejected even if drift leg passes.

## Predictions (must observe if true)
- P1: PCMCI+-alone scores <50% AC@1 on wear-drift while holding ≥70%-direction on abrupt/propagating (the regime split is real).
- P2: Split arm lifts wear-drift AC@1 by ≥10pp to ≥50% with abrupt-subset within ±10pp of PCMCI+-alone.
- P3: Graph-free control does NOT match the complement on wear-drift (credit belongs to graph distance, not mere ordering).

## Alternative Explanations
- A1: Wear chains are traceable by simpler means (threshold-crossing order, delay timing) without graph-distance machinery. Distinguish: graph-free baseline arm; if it matches the complement on wear-drift, A1 wins and the complement is cut.
- A2: Drift failure is a window/scale artifact (A-S4 §VII short timescales), not a discovery-regime failure. Distinguish: multi-scale window variant of PCMCI+-alone; if it alone recovers the drift subset, the fix is windowing (H2), not a second causal method.

## Priority
P0 — decides the causal-layer architecture for the upgrade spec.

## Status
Contested — PCMCI+ drift-collapse locked (TimeGraph B1/C1 0.00/1.00); complement win rests on single-source A-S7; ordering/timing alternative (P3) highest risk. Three-arms-plus-BARO-control battery decisive.
