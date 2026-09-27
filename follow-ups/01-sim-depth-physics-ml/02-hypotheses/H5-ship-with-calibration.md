# H5 — Ship-with-calibration rule

Status: Contested · Source: R5 (A-S14/A-S15 vs C02/C04/I12) / G5 / H-seed-e · Date: 2026-09-13 · Updated 2026-09-13 per claim-lock annex (manual-spec reading provisionally refuted R3; auto-calibration reading survives; 299-rule + M2AD magnitudes unresolved)

## Claim
Ship-with-calibration rule: every new twin channel (CH8 motor current, CH9 energy, CH10 air pressure, thermal state, impulse envelope) ships a Q_DET operating point + noise spec; without it, thresholds ported from the old 7-channel twin mis-fire on the new channels. On the frozen M0b battery with FAULT_RANGES held fixed, the uncalibrated-new-channel arm (old thresholds ported) vs the calibrated arm differs by a firing failure: uncalibrated exceeds >10 alerts/1000 healthy windows OR drops ≥5pp raw point-wise F1 (PA-off) vs calibrated.

## Origin
R5 ledger mismatch: in-step ODEs are O(1) and replay-safe (A ledger, <600s verified-cheap) while per-channel calibration + re-tuning labor is unpriced (B ledger: back-loaded value, year-2 accounting failures). Unresolved → resolution rule, not a winner: a channel without calibration + noise spec doesn't ship. G5/S6 joint rule: document transfer boundary, keep domain randomization. Respects Locked F1–F3 (battery-scoped firing claim only).

## Falsification Criteria
Runnable on the frozen M0b battery + fault-free healthy-window replay, two arms (calibrated vs uncalibrated-new-channel), identical seeds/faults/fault-mags:
1. Record alerts/1000 healthy windows and raw-F1 per arm.
2. Tripwire: uncalibrated arm ≤10 alerts/1000 AND ΔF1_uncal−cal ≥ −0.05 → H5 false (calibration dispensable).
3. Otherwise H5 holds for that channel; verdict is per-channel.

## Predictions (must observe if true)
- P1: ≥1 new channel (predicted: current or impulse envelope) trips a leg uncalibrated — alert flood or ≥5pp F1 drop — and both legs clear once calibrated.
- P2: Failure concentrates on new channels; old 7 channels hold their operating points across both arms (porting is the cause, not global drift).
- P3: Noise-spec mismatch predicts the firing direction: underestimated noise → alert flood; overestimated → F1 drop.

## Alternative Explanations
- A1: Mis-fire comes from fault-mag choice (4–7σ sensitivity tuning), not missing calibration. Distinguish: FAULT_RANGES fixed across arms — only Q_DET/noise-spec porting toggles. If varying mags erases the calibrated/uncalibrated gap, A1 wins.
- A2: One global threshold suffices; per-channel specs are overkill. Distinguish: global-threshold third arm; if it matches per-channel calibration on both legs, scope the rule down to a global Q_DET.

## Priority
P1 — gates channel acceptance (ship rule), not physics existence.

## Status
Contested — manual per-channel ship-blocker provisionally falsified (zero-shot/single-cutoff/auto-GMM/randomisation convergence); narrowed auto-calibration rule survives. Third global/auto arm (A2) decisive.
