# H1 — Supervised-with-free-labels + unknown-family slice beats normal-only on twin data

Seed: contradictions-map Row 2 (supervision vs caricature; Leaning-A with mandated hedge) · Theme T-C2 · Gap 3 · Date: 2026-09-13 · Status: Contested (tripwire-2 leg provisionally falsified 2026-09-13, known-family margin leg survives; no wire fires before M0b) · Priority: P0

## Claim
Per-family supervised training using the twin's FREE sim labels beats a normal-only baseline on twin M0b battery data on raw point-wise F1 (PA-off), WITHOUT collapsing on faults from families absent from training.

## Null (H0)
Supervised-with-free-labels − normal-only raw-F1 < +3pp on the M0b twin battery, OR unknown-family recall < 0.30 — i.e. supervision keys on 4–7σ rectangular caricatures and fails the hedge.

## Falsification criterion + numeric tripwire (runnable on M0b battery / training runs)
Arms (identical features, windows, calibration; episode-seeded grouped CV; all stats inside folds):
- Arm N: normal-only (one-class) trained on RUN-normal windows only.
- Arm S: supervised per-family (abrupt→supervised head; wear-drift→normal-only/drift-chain complement per upgrade-spec T8; sensor-vs-process→parity-supervised head), synthetic dose/shape via ablation (never uniform-noise default).
- Hedge split: held-out unknown-family slice = wear-drift + SENSOR_VS_PROCESS episodes ABSENT from training, fixed before the run.
- Calibration: fixed-percentile on clean-episode validation only (F1 lock); report alerts/1000 at 20/150/5 as companions.

TRIPWIRE — H1 is FALSE if EITHER fires:
1. `F1_raw(S) − F1_raw(N) < 0.03` (under +3pp margin), or
2. `recall_unknown(S) < 0.30` on the held-out unknown-family slice.
H1 survives only if BOTH hold: margin ≥ +3pp AND unknown recall ≥ 0.30.

## Predictions
1. S beats N by ≥ +3pp raw-F1 on known families (abrupt faults: spike/drift/bias/delay/breakdown/quality).
2. S holds recall_unknown ≥ 0.30 (supervision generalizes past caricature magnitudes via per-family formulation + dose ablation).
3. Dose ablation shows an interior optimum: zero synthetic dose underperforms; saturating dose dilutes known-signal (recall_known drops while recall_unknown stalls).

## Alternatives + distinguisher
- A1 (caricature overfit): S wins known families but recall_unknown < 0.30 → supervision memorized 4–7σ rectangulars. DISTINGUISHER: the unknown-family slice itself — A1 passes tripwire-1 and fails tripwire-2; H1 requires both.
- A2 (normal-only suffices): N matches S within ±3pp AND N's recall_unknown ≥ recall_unknown(S) → free labels add nothing. DISTINGUISHER: head-to-head delta on the SAME battery; if |Δ| < 3pp, adopt N (simpler) and falsify H1.
- A3 (dose poison): any synthetic dose hurts (dilution/contamination dominates) → S < N. DISTINGUISHER: dose-ablation curve (0 / low / high); A3 predicts monotone-decreasing F1 in dose.

## Verification method
M0b battery training runs: episode-seeded grouped CV, wear/maint/family stratification, 50%-overlap FFT windows with purge/embargo, per-family heads, dose/shape ablation (≥3 dose levels), fixed-percentile calibration on clean-episode validation, raw-F1 primary + unknown-slice recall + alerts/1000 companions. H2 gate applies to any deep backbone used in S or N.

## Expected outcome
H1 survives with margin (Leaning-A direction): S clears +3pp on known families and holds the 0.30 unknown floor, with dose ablation selecting a non-extreme dose. Fallback if falsified: ship normal-only + drift-chain complement (T8), keep free labels for window-label/back-labeling discipline only.

*Status: Contested — tripwire-2 leg provisionally falsified 2026-09-13 (H1-contra C1–C7; known-family leg survives). No retrieval consumed beyond evidence runs.*
