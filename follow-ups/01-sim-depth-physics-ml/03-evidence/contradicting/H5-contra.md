# H5-contra — Falsification attempt: ship-with-calibration rule

Date: 2026-09-13 · Role: devil's advocate · Hypothesis: H5 (uncalibrated-new-channel arm floods >10/1000 OR drops ≥5pp raw-F1 vs calibrated arm, FAULT_RANGES fixed; verdict per-channel)
Falsification criterion targeted: uncalibrated arm ≤10/1000 AND ΔF1 ≥ −0.05 (calibration dispensable); registered alternative A2 (one global threshold suffices) + domain-randomisation/A1 directions.
Searches run: 5 (all 2026-09-13). Fetches: 1 verified (PMLR v258 M2AD abstract page); 1 blocked (zenodo record → browser-check wall, snippet-only → UNVERIFIED).

## Contra evidence

### C-H5-1 (HIT — zero-shot / training-free transfer with a single threshold)
- Zenodo 18682497 (2026-02-18): "fully training-free, self-supervised, zero-shot anomaly detection framework… operates without labeled anomalies, backpropagation, or retraining… deterministic statistical embeddings capturing marginal behavior and cross-sensor dependencies" for multivariate sensor streams. (Full record UNVERIFIED — browser wall; claim from abstract page only.)
- OpenCSI (pubdb 2607.26665): "model trained on one deployment holds a single empty-versus-occupied decision threshold zero-shot across nearly all transfer cells, reaching binary F1 up to 0.99 where standard normalization drops to 0.87 or fails outright, with no target-domain data or retraining" — including a same-room chip swap isolating hardware from geometry.
- NeurIPS 2023 ACR ("Zero-Shot Anomaly Detection via Batch Normalization"): off-the-shelf deep detectors adapt to new-normal batches with "no training data… for the new normal".
- Zero-shot bearing generalisation (arxiv 2601.11415 snippet): strict zero-shot across rolling-element bearings "without retraining, fine-tuning, or target-domain labels" with "quantile-based thresholding… high quantile of the nominal score".
- Verdict: HIT — convergent zero-shot literature says new sensors/channels can hold one transferred operating point at high F1 with no per-channel spec. Directly instantiates the falsification conjunction (within budget AND within 5pp).

### C-H5-2 (HIT — A2: one threshold works everywhere via normalised scoring)
- tsanomaly (PyPI v0.4.1): "One threshold works everywhere — every anomaly gets a 0-100 score… A 90 means the same rarity on any metric, so you can rank and alert across metrics with a single cutoff"; sampling/patterns "learned per metric" automatically.
- Triaxis GDI: "unsupervised on every run… no training labels, no per-dataset retuning… identical code, zero per-dataset configuration" across published datasets, CPU-only.
- signalmap (PyPI v0.5.3): "zero-config unsupervised condition monitoring over arbitrary recorded/raw signals, with an extensible adapter model for new sources."
- safeband (GitHub MarekWadinger): "Self-adapting — limits track environmental change and sensor aging online; no manual re-tuning, no batch retraining."
- Verdict: HIT for A2 — production-grade tooling converges on score-normalisation + single global cutoff as the shipped design, contradicting the per-channel Q_DET/noise-spec ship-blocker. (Quality caveat: product pages, not peer-reviewed — weighted accordingly, but convergent across 4 independent implementations.)

### C-H5-3 (MIXED — M2AD: heterogeneity handled by calibration, but automatically)
- M2AD (PMLR 258:4384–4392, AISTATS 2025, abstract page fetched + verified): "residuals aggregated into a global anomaly score through a Gaussian Mixture Model and Gamma calibration… theoretically address heterogeneity and dependencies across sensors and systems… outperforms existing methods by 21%… 130 assets in Amazon Fulfillment Centers."
- Verdict: MIXED — supports "heterogeneity needs handling" (H5's premise) but contradicts "manual per-channel spec before ship": M2AD's GMM+Gamma layer learns the cross-sensor calibration from nominal data automatically. The ship rule's cost premise (unpriced re-tuning labour) is answered by automating calibration, not mandating it per channel.

### C-H5-4 (HIT — domain randomisation replaces calibration labour)
- Sim2Real survey lines: domain randomisation (dynamics/observation-noise/mass/friction) yields policies "directly applied to the physical world without any real-world fine-tuning"; DROPO recovers randomisation distributions from limited offline data; SPiDR/NeurIPS-2025 penalises uncertainty for zero-shot safety.
- RAPT (arxiv 2602.01515): "optionally calibrating these signals on a small amount of verified nominal real-world data… suppress static Sim-to-Real bias" — calibration framed as optional small-sample step, complementing (not gating) randomisation.
- Verdict: HIT — the registered G5/S6 joint rule already keeps domain randomisation; contra shows randomisation + tiny nominal-sample auto-calibration can hold both H5 legs with "zero-marginal-calibration-cost" in the per-channel-manual sense. If the calibrated arm = auto-calibrated (not hand-spec'd), the uncalibrated-vs-calibrated gap may sit within 5pp/10-1000 and fire the tripwire.

### C-H5-5 (MISS — IT-monitoring channel-management results do not transfer)
- PRTG/LoopString/S7-1500 queries returned only device-management trivia (channel add/remove UX, CiR reconfig) — no evidence on threshold porting success or failure.
- Unsupervised-transfer + adversarial VAE zero-shot domain adaptation (ar5iv 2008.07815; arxiv 2609.03505 IoT): domain-invariant latents transfer across operating conditions — adjacent HIT for transferability, but operating-condition transfer ≠ new-channel porting, so kept as adjacent, not decisive.
- Verdict: MISS — no direct "ported thresholds worked fine on CH8/CH9/CH10-equivalent industrial channels" study found; the exact per-channel battery comparison is unrun anywhere in the retrieved set.

## Falsification verdict: PROVISIONALLY FALSIFIED after 5 searches (as a *manual-spec* ship-blocker)
Zero-shot single-threshold results (C-H5-1), normalised-score single-cutoff tooling (C-H5-2), automatic GMM+Gamma cross-sensor calibration (C-H5-3), and randomisation+auto-calibration (C-H5-4) converge: channels CAN ship without hand-written per-channel Q_DET/noise specs provided calibration is automated (normalised rarity scores, learned GMM/Gamma layer, or self-adapting limits). What survives is a narrowed rule: "every channel ships with an *automatically derived* operating point + noise characterisation" — the manual-spec reading is provisionally falsified; the auto-calibration reading is not. Decisive test: add a third arm (global/auto-calibrated threshold, per H5-A2) to the battery — if it matches per-channel hand calibration on both legs, scope the rule down. Falsification strength: **Provisionally falsified after 5 searches** (manual-spec interpretation; auto-calibration interpretation not falsified).
