# H3 — AC@1≥70% sim-scoped achievability

Status: proposed
Priority: P0 (load-bearing for trace-ranking claim)

## Claim
X produces Y under Z: Ranked upstream trace (depth ≤3) over the topology prior on the seeded twin battery (20–32 injected faults, free sim ground truth, CPU <10 min) produces Accuracy@1 ≥70% (target 0.80–0.8125 real-path), which does NOT transfer as a general claim to real-incident telemetry.

## Origin
Source contradiction C4 (AC@1≥70% achievable [project battery 0.80–0.8125; A-S19 0.88] vs SOTA ceiling Avg@5 0.46–0.54, DELAY/LOSS near-death [A-S15], BARO graph-free leads [A-S16]; unresolved-scoped). Gap 5 (no brownfield transfer evidence) bounds scope; sim-ground-truth validity from T5.

## Falsification
Would be disproven by: ranked-trace AC@1 <70% on the full 20–32 fault battery under seeded replay (all seeds counted, no fault subsetting), OR any presentation of the result as general (non-sim-scoped) without new real-incident evidence. Numeric tripwire: AC@1 <0.70 (i.e., <14/20 or <23/32 top-1 hits) → H3 false.

## Predictions
1. Sim battery: AC@1 ≥0.70 with seed hashes and ≤3-step paths logged.
2. Ablating the topology prior (BARO-style graph-free ranking) drops AC@1 by ≥10pp on the same battery.
3. DELAY/LOSS-type faults (per A-S15) are the misses — error concentrates there, not uniformly.

## Alternatives
Most likely if false: sim labels flatter the ranking and graph-free BARO matches or beats the topology-prior trace (domain prior adds nothing; RCAEval ceiling generalizes). Then scope the claim down to Avg@K or concede parity with BARO.

## Verification-method
Retrieval/evidence type: experimental battery evidence (seeded replay, CPU <10 min wall-clock logged): full-battery AC@1 count with replay hashes; graph-free ablation as control. Literature (RCAEval, BARO/CIRCA) supplies the ceiling comparator only.

## Expected-outcome
If holds, MUST observe: AC@1 ≥0.70 on the complete battery AND ablation gap ≥10pp AND total runtime <10 min CPU, all in one logged run-table.

## Priority / Status
Priority P0. Status: proposed (awaiting full-battery run; no retrieval in this phase).
