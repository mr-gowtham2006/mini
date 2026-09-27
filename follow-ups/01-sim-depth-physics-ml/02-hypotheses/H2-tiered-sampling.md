# H2 — Tiered sampling (RMS base + impulses + multi-scale export)

Status: Contested · Source: R2 (A-S10/A-S12/A-S2 vs I03/I04/I08/I06/I07) / G2 / H-seed-b · Date: 2026-09-13 · Updated 2026-09-13 per claim-lock annex (separation leg provisionally refuted R1; F1 leg intact; metric validity locked L2)

## Claim
Tiered sampling — 1/step RMS-grade base (MES/historian economy) + wear-coupled impulse term on the observation channel + multi-scale export windows (0.5/1.0/2.0-step-equivalent) with an explicit no-bearing-frequencies claim boundary — improves envelope-domain fault features without degrading detection on the frozen M0b battery: (a) envelope-band energy separation (fault vs healthy window means) on seeded impulse faults rises ≥15% vs RMS-only baseline, AND (b) skew/kurtosis-conditioned quantile raw-F1 (PA-off) does not drop ≥3pp vs baseline.

## Origin
R2 contradiction: diagnosis needs fast-channel impulse content and full bandwidth (A-side) vs plants historize RMS/envelope at ≤1 Hz and never claim bearing frequencies (B-side). Synthesis is tiered; guardrail is the A-S4 §VII half-modelled-timescale warning (partial impulses can HURT). G2: impulses must clear envelope-domain acceptance, not raw-waveform matching. Respects Locked F1–F3 (battery-scoped detection claim only).

## Falsification Criteria
Runnable on the frozen M0b battery, RMS-only vs tiered arms, identical seeds/faults:
1. Compute envelope-band energy separation gain: (μ_fault − μ_healthy)/μ_healthy per arm on seeded impulse faults.
2. Compute skew/kurtosis-conditioned raw-F1 per arm (quantile detector, fixed-percentile, PA-off).
3. Tripwire: separation gain <15% OR ΔF1_tiered−base ≤ −0.03 on conditioned faults → H2 false.

## Predictions (must observe if true)
- P1: Tiered arm shows ≥15% envelope-separation gain on impulse faults while conditioned-F1 stays within ±3pp of baseline.
- P2: Never-downsampled fast derivation beats a downsampled-then-rederived control by ≥8pp-equivalent feature separation (A-S12 direction).
- P3: No bearing-frequency claim is needed for the gain; envelope statistics carry it.

## Alternative Explanations
- A1: Half-modelled harm (A-S4 §VII) — partial impulses add noise without the fast physics they mimic; separation gain is noise-fitting. Distinguish: envelope-domain acceptance on held-out seeds + conditioned-F1 leg; passing separation but failing F1 = harm confirmed, H2 false.
- A2: Multi-scale windows alone (no impulse term) explain the gain. Distinguish: three-way arms (RMS-only / windows-only / windows+impulse); if windows-only matches tiered, scope the win to windowing.

## Priority
P0 — decides sampling-rate design and the impulse-term fate (H1's riskiest term).

## Status
Contested — ≥15% separation leg provisionally falsified (low-rate sufficiency both sides + 2–10 kHz resonance requirement); conditioned-F1 leg not falsified; windows-only alternative (A2) live. Battery decisive.
