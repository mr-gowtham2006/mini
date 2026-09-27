# H5 supporting evidence — Ship-with-calibration rule

Date: 2026-09-13 · Scope: 32-machine SimPy twin · Battery guards: Locked F1–F3 respected; FAULT_RANGES held fixed across arms.
Claim tested: per-channel Q_DET operating point + noise spec ships with every new channel; uncalibrated-new-channel arm (old thresholds ported) trips >10 alerts/1000 healthy windows OR ≥5pp raw-F1 drop vs calibrated arm.

## Findings (supporting only)

### S-H5-01 — Calibration excluded from business cases; one-time calibration degrades within months (VERIFIED full text)
- Source: https://www.halkwinds.com/research/digital-twin-enterprise-adoption-report — Halkwinds Research 2026 → **L4** (analyst report; explicitly NOT a formal survey — "not based on a statistically representative survey instrument"; read as expert synthesis).
- Claims: TCO "frequently underestimated because ongoing model calibration, sensor infrastructure maintenance, and change management are excluded"; "treat twin calibration as a one-time activity… find model performance degrades within months of deployment, eroding the trust of operators"; "plan and resource ongoing model calibration as an operational process before production deployment — model drift is the most common cause of twin program performance degradation"; fleet operators amortize calibration across instances (strongest unit economics).
- HOW it supports H5: the back-loaded-value / calibration-cost ledger (R5's B-side) documented as industry pattern — calibration labor is the unpriced term that decides ship/no-ship, exactly H5's resolution rule. The trust-erosion consequence ties H5's firing failure to H3's precision bar.
- Vs alternative A1 (mis-fire is fault-mag tuning, not calibration): the reported degradation occurs without fault-mix changes (same assets, aging sensors/process drift) — points at calibration, not mag choice. Battery still must hold FAULT_RANGES fixed per protocol.
- Confidence: moderate (transparent methodology section; no hard numbers — qualitative pattern).

### S-H5-02 — 299 normals for a 95%-confidence 1% FPR claim; recalibrate on any detector/process change (VERIFIED abstract page)
- Source: https://arxiv.org/abs/2608.15090 — Deng 2026, distribution-free false-alarm calibration → **L4** (preprint; abstract + sample-planning numbers verified on abs page; full text not opened).
- Numbers: "with 150 calibration normals, a 95%-confidence distribution-free claim is supported only for target FPR ≥1.98%; a 1% target requires at least 299 normals"; "recalibration should be considered after changes to the detector, camera, illumination, tooling, material, or production process."
- HOW it supports H5: quantifies the per-channel calibration price — a new channel cannot inherit an FPR claim from old channels; it must ship its own calibration set (≈300 normals per 1% FPR at 95% confidence) plus a recalibration trigger list. Direct normative backing for "Q_DET operating point + noise spec ships with the channel."
- Confidence: moderate (preprint; vision-AD domain — transfer the sample-planning logic, not the constants).

### S-H5-03 — 47/50 daily FPs caused by drift/ambient/calibration decay (VERIFIED full text)
- Source: https://21tech.com/your-predictive-maintenance-platform-generates-100000-alerts-a-day-your-team-reads-12/ — 21Tech, Apr 2026 → **L5** (trade analysis; case numbers unaudited).
- Claim: of ~50 investigated alerts/day only ~3 actionable; "the other 47 were false positives caused by sensor drift, ambient temperature swings, or calibration decay."
- HOW it supports H5: field-observed mis-fire mechanism for uncalibrated/aged channels — the uncalibrated arm's alert-flood leg (P3: underestimated noise → flood). Predicts the failure concentrates on new/poorly-calibrated channels (H5-P2).
- Confidence: low-moderate (anecdote; FP-cause attribution is the author's, not measured).

### S-H5-04 — Minimum viable data model incl. sensor reliability; start narrow (VERIFIED full text)
- Source: https://www.smartindunews.com/news/Evolutionary_Trends/Digital_Twin_for_Industrial_Equipment_Use_Cases_Data_Requirements_and_ROI_Factors.html — SmartInduNews, Jul 2026 → **L5** (trade analysis).
- Claims: "some programs try to model the entire lifecycle at once, even when basic sensor calibration or historical records are incomplete. A narrower starting point is often more credible"; minimum viable data model = "sensor reliability, historical maintenance records, engineering assumptions, and validation checkpoints"; "ROI is often overstated when teams focus only on maintenance savings."
- HOW it supports H5: independent ship-gate formulation — a channel without calibration/validation evidence doesn't ship; scope to what the data foundation supports. Backs H5's per-channel verdict and the C04 back-loaded-ROI direction.
- Confidence: low-moderate (trade press, no data).

### S-H5-05 — Uncalibrated sensor values worsen learning; threshold-gated filtering (SNIPPET-ONLY)
- Source: https://link.springer.com/article/10.1007/s00521-021-06865-z — Springer Neural Comput. Appl. 2022, real-time uncalibrated-sensor detection → **L2 snippet-only** (publisher page JS-blocked on fetch).
- Claim (excerpt): extreme/uncalibrated values "add noise to the input signal and worsen NN learning"; system drops measured values exceeding warning/alarm thresholds so "only calibrated and non-extreme" values train the model.
- HOW it supports H5: peer-reviewed precedent that uncalibrated channels actively harm the downstream model (F1-drop leg), and that per-channel threshold specs are the shipped remedy.
- Confidence: low (snippet). Gap-pass item G-H5-a.

### S-H5-06 — Drift-flag → threshold-adjust feedback loop (SNIPPET-ONLY)
- Source: https://www.mdpi.com/2076-3417/15/12/6500 — M2D2 multi-machine drift detection 2025 → **L2 snippet-only** (MDPI fetch 403).
- Claim (excerpt): experts label drift alerts genuine/false-alarm; for false alarms "the model or threshold can be adjusted accordingly"; labeled novelties feed an anomaly database for retraining.
- HOW it supports H5: operating-process precedent for H5's ongoing-calibration implication — the ship rule needs a recalibration loop, not just a day-0 spec.
- Confidence: low (snippet). Gap-pass item G-H5-b.

### Phase-1 cache rows reused (no re-fetch)
- I12: dropout→FP fixed by sensor-health flag; overhaul needs baseline reset + suppression — per-channel calibration ops precedent.
- C02/C04: vendor 18-mo payback doesn't survive year-2 costs; back-loaded ROI — the value-timing half of R5.
- A-S14/A-S15: event-stepped DES↔continuous interface; deterministic seeded replay — the cheap-replay half (calibration battery is affordable).

## Net assessment (supporting lane only)
H5's cost/back-loaded half is the best-supported of all five hypotheses' context legs (S-H5-01 verified, S-H5-04 convergent). The firing-failure half (uncalibrated arm trips a tripwire) has mechanism support (S-H5-03, S-H5-05) but no controlled calibrated-vs-uncalibrated experiment — that is precisely the battery to run.
