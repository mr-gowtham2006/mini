# H2 supporting evidence — Tiered sampling (RMS base + impulses + multi-scale export)

Date: 2026-09-13 · Scope: 32-machine SimPy twin · Battery guards: Locked F1–F3 respected.
Claim tested: tiered arm (1/step RMS base + wear-coupled impulse + 0.5/1.0/2.0-step-equivalent multi-scale export, no-bearing-frequencies boundary) raises envelope-band energy separation ≥15% on seeded impulse faults with conditioned raw-F1 within ±3pp of RMS-only baseline.

## Findings (supporting only)

### S-H2-01 — Targeted envelope-spectrum SNR optimisation beats 5 methods on 3 experimental gear datasets (VERIFIED full text)
- Source: https://arxiv.org/html/2405.00727v2 — Schmidt, Wilke & Gryllias, arXiv:2405.00727v2 (eess.SP, peer-reviewed version trajectory; as-fetched preprint) → **L4**.
- Claim: GES2N objective directly maximises squared-envelope-spectrum SNR over targeted cyclic bands via gradient filter optimisation (SciPy CG); 4 derived objectives outperform CYCBD, ACYCBD, MOMEDA, L2/L1 envelope norm, spectral negentropy on three gearbox datasets under time-varying speed; SES metrics M1–M4 (target-harmonic amplitude vs noise floor / extraneous component).
- HOW it supports H2: validates H2's acceptance statistic itself — envelope-band energy separation is the community's working detection metric, and targeted sub-band optimisation (not full-waveform matching) is what moves it. Directly licenses H2's "envelope-domain acceptance test, not raw-waveform match" and the no-bearing-frequencies boundary (targeted cyclic bands, not claimed characteristic frequencies).
- Vs alternative A1 (half-modelled harm): this is full-signal filtering, not twin-simulated impulses — so it supports the metric, not the twin's impulse term per se. The conditioned-F1 leg of H2 remains the guardrail.
- Confidence: moderate-high for the metric; moderate for transfer (gearboxes, kHz sampling → twin's 1/step RMS + impulse term is a coarse analogue).

### S-H2-02 — Multiband envelope spectra: fault energy spread over multiple narrow bands (SNIPPET-ONLY)
- Source: https://doi.org/10.3390/s18051466 — Duan et al., Sensors 18(5):1466 (2018) → **L2 snippet-only** (MDPI fetch 403; DOI resolves citation stub only).
- Claim (excerpt): repetitive transients appear as impulses in envelope spectrum AND spread over a wide frequency range, so fault components appear in multiple narrow bands; method integrates multiband diagnostic information.
- HOW it supports H2: mechanism match for multi-scale export — single-band/single-scale features under-capture impulsive faults; multiband integration is the published fix. Supports P3 (envelope statistics carry the gain without bearing-frequency claims).
- Confidence: low-moderate (snippet-only). Gap-pass item G-H2-a.

### S-H2-03 — Optimal sub-band selection on 250 kHz AE signals (SNIPPET-ONLY)
- Source: https://doi.org/10.3390/s18051389 — Sensors 18(5):1389 (2018) → **L2 snippet-only**.
- Claim (excerpt): per-sub-band envelope power spectra over 31 reconstructed sub-bands from 250 kHz acoustic-emission signals; "selection of an appropriate sub-band is essential… reveal intrinsically explicit information about different fault types" via Gaussian-model health index.
- HOW it supports H2: sub-band decomposition + selection is load-bearing for impulsive-fault diagnosis at high sampling rates — supports the tiered claim that a fast/derived tier carries information the base tier cannot, and that sub-band machinery (not raw downsampling) is the right interface.
- Confidence: low-moderate (snippet-only). Gap-pass item G-H2-a (same fetch pass as S-H2-02).

### S-H2-04 — Adaptive sub-band kurtogram direction (SNIPPET-ONLY, two strands)
- Strand a: adaptive reweighted kurtogram, J. Sound Vib.–adjacent SHM journal 2024 (DTCWPT band division, "far richer band division patterns") — https://doi.org/10.1177/14759217231226267 → **L2 snippet-only**.
- Strand b: STAKgram (subband trimmed-average kurtogram, robust under interference), IOP 2024 — https://iopscience.iop.org/article/10.1088/1361-6501/ad7b64 → **L2 snippet-only**.
- HOW they support H2: multi-scale/sub-band sensing with robust band selection is an active, winning direction (2024), i.e. H2's multi-scale export windows track the field's motion, and trimmed/robust statistics answer the "impulse noise" objection inside H2's conditioned-F1 leg.
- Confidence: low (snippets). Gap-pass item G-H2-b.

### Phase-1 cache rows reused (no re-fetch; snowball seeds)
- A-S12 (full text, Phase 1): full bandwidth beats downsampled by ≥8pp across 19 datasets; fixed-duration STFT sub-band modelling handles arbitrary rates — H2's P2 ("never-downsampled fast derivation beats downsampled control") is near-verbatim this row.
- A-S2: spectral kurtosis + kurtogram + envelope pipeline is the standard impulsive-fault chain — the pipeline H2's impulse term feeds.
- A-S10: multi-scale 0.5/1.0/2.0 s windows over vibration + current + AE catch transients + wear — H2's window prescription source.
- I03: raw vibration ≥10k values/s burst-collected, FFT'd, only derived values historized — industry existence proof of the tier boundary (fast tier sensed, derived tier stored).
- I04 / I08: 1 s / 5 s / 20 s scan classes; max 1 Hz sufficient for MES/analytics — RMS-grade base tier normative anchor.

## Net assessment (supporting lane only)
Envelope-domain + sub-band machinery is strongly supported as the right acceptance layer (S-H2-01 verified; S-H2-02/03/04 directional snippets). The twin-specific risk (simulated impulses adding noise — A1/A-S4 §VII) is NOT resolved by any finding here; it is exactly what H2's conditioned-F1 leg must test on the battery.
