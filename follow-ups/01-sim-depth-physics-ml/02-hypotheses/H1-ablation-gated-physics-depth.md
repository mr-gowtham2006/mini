# H1 — Ablation-gated physics depth

Status: Contested · Source: R1 (A-S9/A-S11/A-S6/A-S2 vs A-S5 + C01–C04) / G1 / H-seed-a · Date: 2026-09-13 · Updated 2026-09-13 per claim-lock annex (L1 locks residual-gain existence only; per-term deltas pending battery; contra C-H1-1..4 pressured pass predictions)

## Claim
Each new physics term added to the 32-machine SimPy twin (lumped thermal lag, drive-current coupling, wear-knee scalar, wear-coupled impulse term) earns its place only by moving a frozen-battery metric: vs the ablated twin (term off, everything else fixed — seeds, mask, calibration, T=300/CAL_WIN=120), raw point-wise detection-F1 shifts by ≥3pp OR ranked-trace AC@1 (depth≤3) shifts by ≥10pp (≥2/20 or ≥2/32 top-1), all seeds counted, wall <600s, 0-diverge ×5. Any retained term below both deltas is cut. Verdict is per-term.

## Origin
R1 contradiction: mechanism precedent for deeper per-machine physics (thermal/drive/wear/impulse, A-side) vs the only head-to-head detection ablation showing lower fidelity matching high fidelity (A-S5) plus deployment postmortems saying twins stall on decision/data gaps, not physics (C-side). Balance Leaning-B → depth is guilty until proven useful. G1 rule: every term passes detection/traceback ablation or is wasted CPU. Respects Locked F1 (raw F1 only), F2 (no platform framing), F3 (no joint wedge claim).

## Falsification Criteria
Runnable on the frozen M0b battery (20–32 seeded faults, quantile detector fixed, fixed-percentile calibration, PA-off):
1. Per term, run full vs ablated arms on identical seeds. Record ΔF1_raw and ΔAC@1.
2. Tripwire: term retained in the spec yet ΔF1 < 0.03 AND ΔAC@1 < 0.10 → H1 false for that term (and the registry entry is falsified if any below-delta term is kept).
3. Budget tripwire: full-twin wall ≥600s or any diverge across 5 seeds → term fails regardless of metric gain.

## Predictions (must observe if true)
- P1: ≥1 term (predicted: wear-knee or current coupling) clears a delta; ≥1 term (predicted: thermal lag or raw impulse) fails and is removed.
- P2: Ablation deltas replicate across ≥5 seeds (sign-stable); removing a passing term reverses the gain.
- P3: Wall-clock stays <600s and 0-diverge holds with passing terms in; failing terms add cost without metric gain.

## Alternative Explanations
- A1: Fidelity ≠ detection (A-S5) — low-fi already matches high-fi; observed gains come from seed/mask/calibration choice, not the term. Distinguish: ablation fixes seeds, mask, and Q_DET across arms; only the term toggles. If Δ vanishes under seed-sweep, A1 wins.
- A2: Gain is fault-mix-specific (e.g. only on T3 wear faults). Distinguish: report per-class Δ (abrupt vs drift vs sensor); a term passing only one class is scoped to that class, not general depth.

## Priority
P0 — gates every physics addition; decides the whole upgrade spec.

## Status
Contested — supporting L1 (SciRep residual gain + A-S4) vs contra pressure (low-fi parity, overfitting sign-reversal); no battery tripwire fired. Per-term verdicts pending frozen-battery ablation.
