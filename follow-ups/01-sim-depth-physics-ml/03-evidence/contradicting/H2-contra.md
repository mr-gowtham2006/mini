# H2-contra — Falsification attempt: tiered sampling

Date: 2026-09-13 · Role: devil's advocate · Hypothesis: H2 (tiered 1/step RMS + impulse term + multi-scale export lifts envelope separation ≥15% with conditioned-F1 within 3pp)
Falsification criterion targeted: separation gain <15% OR ΔF1 ≤ −0.03; registered alternative A1 (half-modelled-timescale harm — partial impulses add noise).
Searches run: 5 (all 2026-09-13). Fetches: 1 attempted (MDPI sensors-21-19-6678 → 403; snippet-only → UNVERIFIED full text).

## Contra evidence

### C-H2-1 (HIT — 1 Hz / low-rate sufficiency for diagnosis)
- Nature Sci. Data 2026 (APC centrifuge, s41597-026-07665-7): 12-month predictive-maintenance dataset at ONE-MINUTE intervals (~0.0167 Hz, torque + vibration-velocity RMS + dual bearing temps, ~525k obs/channel) published as research-grade PdM data. Verdict: HIT — real PdM research operates orders of magnitude below H2's 1/step base, implying coarse RMS trending carries the published fault signal without any impulse tier.
- IndustrialMonitorDirect (ISO 10816 PLC guide, 2026-09-09): "For broadband RMS trending, 1 kHz sample rate is sufficient"; practice = "1-minute RMS averages in the PLC for 30 days, plus 1-second raw data for 1 hour rolling buffer", dumped to CSV only on fault events. Verdict: HIT — plant practice stores RMS-only at historian rates and treats fast data as post-mortem, contradicting the need for a permanent impulse tier.
- IntechOpen ch.17695 (bearings fault detection): "generalized roughness faults… detection by means of the temporal vibration signal RMS analysis" is feasible; RMS evaluation "provides a good indicator". Verdict: HIT — RMS-only sufficiency claim for a whole fault class (roughness/generalized), no envelope tier needed.

### C-H2-2 (HIT — downsampling / low-rate costs little or nothing)
- MDPI Machines 12(1):17 + ICEM 2024 (multi-rate bearing study, 48 kHz → 1 kHz fractional downsampling, linear/tree/NN classifiers): "better training accuracies are not [monotonic in rate]" — accuracy depends on algorithm, not bandwidth. Verdict: HIT — directly shrinks the expected ≥15% separation gain: rate arms do not separate cleanly.
- MDPI Sensors 21(19):6678 "Cost-Effective Vibration Analysis through Data-Backed Pipeline Optimisation" (fetch 403, snippet): downsampling to lower rates gives "a performance loss, however on an absolute scale the loss is small and might be acceptable"; "decrease of the observation time window leads to a more pronounced performance and model confidence drops (based on effect sizes)". Verdict: HIT (snippet-level, UNVERIFIED full text) — attacks H2's multi-scale-window leg from the other side: windows matter more than rate, i.e. H2-A2 (windows-only explains the gain) is the live alternative.
- JMSE 11:81577 (marine-engine time–frequency fusion): "100% accuracy at lower downsampling rates and 96.3% at 100×" downsampling. Verdict: HIT — two orders of magnitude of rate reduction cost <4pp, far below a 15%-separation-need narrative.
- MDPI Proc. 2(13):781 (sensor-network sampling-rate study): cutting 500→100 Hz cost −14% only where 50–250 Hz motor-synchronous features were lost to Nyquist — i.e. losses come from aliased *known* bands, not from missing impulse content. Verdict: HIT — reframes the tier design as band-selection, not impulse-modelling.

### C-H2-3 (MIXED — envelope needs REAL bandwidth, which H2 disclaims)
- iotbearings.com (2026-03-06): envelope analysis "operates on the high-frequency structural resonance band, typically 2,000–10,000 Hz. To capture this band, the system must sample at 25,600 S/s or higher"; 1-s record at 25.6 kS/s for 1 Hz resolution.
- Monitory.ai (2026-05-04): BPFO/BPFI/BSF impulses "show up clearly in acceleration data but are invisible in velocity spectra until the defect is already severe."
- Siemens CMS manual: characteristic RMS values "are not enough for precise defect location"; damage needs spectral damage frequencies.
- Applied Sci. 11:14626 + IEEE Access 2019 (demod-band optimisation): misplaced envelope bands misidentify faults ("instinctively placing the envelope band at the resonant frequency is fraught with danger of misidentification… catastrophic failure"); optimal band must be searched (kurtogram/genetic optimisation). Verdict: HIT as contra — a 1/step wear-coupled impulse term with an explicit no-bearing-frequencies boundary is exactly the half-modelled construction A-S4 §VII warns about: a synthetic impulse without the resonance band it mimics. If envelope gain requires true 2–10 kHz content, H2's tier cannot deliver ≥15% by construction.

### C-H2-4 (HIT — sub-band/full-band no-gain + band-fragility)
- Sensors 18:1466 (MESE multiband): "selecting a signal in only one frequency band may leave out quite a few transient features" — single-sub-band envelope under-captures transients. Verdict: HIT vs H2's single impulse-term formulation (needs multiband to hold the 15% leg).
- IEEE Access 2023 (CWRU envelope + ML): "ball defects pose an exception as they cannot be effectively characterized" by envelope analysis. Verdict: HIT — a whole seeded-fault class where the envelope leg fails regardless of tiering.
- Prism/20991 (machinery looseness): overall-level alarming gives detection but late/poor diagnosis — consistent with RMS sufficiency for *detection* while undermining any claim that sub-band content adds *diagnostic* separation cheaply. Verdict: supporting-HIT for the "RMS suffices, sub-band adds little" direction.

### C-H2-5 (MISS — half-modelled-harm literature not found in target domain)
- Queries on partial-impulse-model harm returned only adjacent domains (Middleton impulsive-noise in comms/PD: SNR loss ≤5 dB, ML-IN receiver +7 dB with redesign). No manufacturing-twin study showing a wear-coupled impulse term degrading conditioned-F1. Verdict: MISS — A1's exact mechanism (ΔF1 ≤ −0.03 from synthetic impulses) has no direct literature hit; it remains a battery-empirical question.

## Falsification verdict: PROVISIONALLY FALSIFIED (envelope leg) / NOT FALSIFIED (F1 leg)
The ≥15% envelope-separation prediction is pressured from both sides: low-rate sufficiency (C-H2-1/2) says RMS already carries the signal, while real-envelope literature (C-H2-3) says genuine envelope gain needs 2–10 kHz resonance content H2 explicitly disclaims — the middle ground (a 1/step synthetic impulse term delivering ≥15%) has no precedent found in 5 searches. The conditioned-F1 leg (no ≥3pp drop) is NOT falsified — no harm study found (C-H2-5). Falsification strength: **Provisionally falsified after 5 searches** on the separation-gain leg; F1 leg **not falsified**.
