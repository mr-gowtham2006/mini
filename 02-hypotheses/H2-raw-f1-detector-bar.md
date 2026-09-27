# H2 — Raw-F1-only detector bar necessity

Status: proposed
Priority: P0 (load-bearing for detector evaluation)

## Claim
X produces Y under Z: Evaluating detectors on the twin's 20–32 fault battery with raw point-wise F1 under fixed-percentile (per-machine quantile) calibration produces a ranking where deep/GNN detectors score F1 ≤0.50 (collapse vs their PA-reported ~0.9), while the per-machine quantile detector holds F1 ≈0.73 conditional (M0b), under identical thresholds and no point-adjustment.

## Origin
Source contradiction C2 (deep/GNN ~0.9 F1 own-protocol [A-S7/A-S11/A-S12/A-S13] vs collapse under raw calibration: TCN-GAT 0.886→0.281 [A-S14], PA invalid [A-S8/A-S9/A-S10], raw SOTA2/WADI TranAD 49.5/MTAD-GAT 41.7/USAD 30.6). Gaps 2 + 4: no per-machine-quantile ablation for TCN-GAT-style collapse; raw-protocol detector comparison on the twin's own battery still owed (M0b).

## Falsification
Would be disproven by: any deep/GNN baseline scoring raw point-wise F1 ≥0.60 on the twin battery under the same fixed-percentile calibration, OR the quantile detector scoring raw F1 <0.60 conditional (M0b). Numeric tripwire: GNN raw-F1 ≥0.60 → bar unnecessary → H2 false; quantile raw-F1 <0.60 → H2 false.

## Predictions
1. TCN-GAT-style baseline: PA-F1 ≥0.80 but raw-F1 ≤0.50 on the twin battery (collapse gap ≥0.30).
2. Per-machine quantile detector: raw conditional F1 ≥0.65 (target ~0.73 M0b).
3. Switching PA on flips the ranking (GNN on top); switching PA off restores quantile on top — protocol decides the winner.

## Alternatives
Most likely if false: a calibrated deep detector transfers to the twin series and beats the quantile baseline on raw F1 (ranking is dataset-specific, not protocol-driven). Then the raw-F1 bar stays but the "learned-graph deprioritized" verdict is revoked.

## Verification-method
Retrieval/evidence type: experimental battery evidence (M0b): run quantile vs ≥1 deep baseline on the same 20–32 seeded faults, fixed-percentile calibration, raw point-wise F1 only, plus PA-on control run to demonstrate the flip. Literature (Kim 2109.05257, Schmidl 2023, ICCI 2026) frames the bar; only the battery decides H2.

## Expected-outcome
If holds, MUST observe: GNN raw-F1 ≤0.50 AND quantile raw-F1 ≥0.65 on the same battery AND PA-on control reversing or compressing the gap.

## Priority / Status
Priority P0. Status: proposed (awaiting M0b battery; no retrieval in this phase).
