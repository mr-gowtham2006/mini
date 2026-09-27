# H1 supporting evidence — Ablation-gated physics depth

Date: 2026-09-13 · Scope: 32-machine SimPy twin · Battery guards: Locked F1 (raw point-wise F1, PA-off), F2, F3 respected.
Claim tested: each added physics term (thermal lag, drive-current coupling, wear-knee, impulse term) must move frozen-battery ΔF1 ≥3pp or ΔAC@1 ≥10pp vs ablated twin, else cut.

## Findings (supporting only; contradicting lane owned by teammate)

### S-H1-01 — Physics-guided residuals + calibrated CRNN beat SOTA on TEP (VERIFIED full text)
- Source: https://www.nature.com/articles/s41598-026-48227-6 — Khan et al., Sci Rep 16:17488 (2026), peer-reviewed journal → **L2**.
- Claim: physics-model residual generation (measured − nominal) + compact rolling/spectral descriptors + attention CRNN over 120-step windows + Platt scaling.
- Numbers: ~99.0% accuracy, AUC-ROC/AUC-PR ≈1.00, macro F1 0.93±0.02 (1000 block-bootstrap BCa), ECE ≈0.03 after calibration, NAB score +17% over Isolation Forest baseline, "low nuisance alarm operation."
- HOW it supports H1: the detection gain is explicitly attributed to the physics-residual channel — "residuals amplify departures from expected behavior" and "improve sensitivity to small drifts masked in raw signals." That is the H1 mechanism (added physics term → detection number moves), demonstrated against SOTA baselines under identical evaluation. Prediction P1 direction (current/thermal-coupled terms clear a delta).
- Vs alternative A1 (fidelity≠detection, gains are seed/mask artifacts): weakens A1 — the paper's ablated comparison is architectural (residual-enriched vs raw-signal SOTA families), not seed choice; and a TEP benchmark paper it cites reports deep detectors plateau at F1 ≈0.907–0.917 across families, so the 0.93 with residuals is above the data-only ceiling.
- Confidence: moderate-high. Caveat: no arm that toggles ONLY the physics term with all else fixed (H1's exact ablation); comparison is pipeline-vs-baseline. Gap-pass item G-H1-a.

### S-H1-02 — Neuro-symbolic DT: physics constraints contribute + failure accuracy 94.2% (landing VERIFIED, ablation numbers SNIPPET-ONLY)
- Source: https://www.techscience.com/cmc/v88n3/68140 — Alzaben et al., CMC 88(3) 2026 → **L2**, but contribution-split numbers below are **snippet-only, UNVERIFIED** (search excerpt of Table 6).
- Claim: temporal transformer + physics-informed constraints (thermodynamic/mechanical/fatigue) + CVAE counterfactuals; 24,042 sensor readings (CNC, pumps, compressors, arms); failure-prediction accuracy 94.2%, R² 0.918 (+25.1% vs baselines), failures −51.7% vs rule-based scheduling.
- Ablation excerpt (UNVERIFIED): multi-task learning 24.4%, positional encoding 20.3%, physics constraints 11.7% (mainly eliminating physically-impossible outliers).
- HOW it supports H1: per-component ablation with directional deltas is exactly H1's prescribed method; physics term carries a double-digit share. 11.7% exceeds H1's 3pp F1 bar in spirit (different metric, so directional only).
- Vs A1: ablation fixes data/splits across arms — matches H1's distinguish protocol (only the term toggles).
- Confidence: low-moderate (numbers unverified; journal venue is low-tier). Gap-pass item G-H1-b: fetch full-text HTML table to confirm.

### S-H1-03 — DT-assisted induction-motor diagnosis with thermal analysis (SNIPPET-ONLY)
- Source: https://iopscience.iop.org/article/10.1088/1361-6501/addbfe — IOP Meas. Sci. Technol. 2025, ITSCF diagnosis coupling real-time current-updated DT with temperature-rise analysis → **L2 snippet-only**.
- Claim (excerpt): DT state parameters updated in real time from measured current; fault diagnosis incorporates induced temperature-rise characteristics.
- HOW it supports H1: direct precedent for H1's two riskiest terms combined (drive-current coupling + thermal lag) as a shipped diagnostic channel on motors — the twin's thermal state is load-bearing for detection, not decoration.
- Confidence: low (landing excerpt only). Gap-pass item G-H1-c.

### S-H1-04 — PMSM DT + LightGBM feature-group ablation (SNIPPET-ONLY, method precedent)
- Source: https://doi.org/10.65455/30tg2482 — Gao 2026, feature groups (thermal, mechanical, electrical, control, aging) removed one at a time + compact AI4I-like subset control → level **L4** (unfamiliar venue), snippet-only (DOI resolves citation stub only).
- HOW it supports H1: this is H1's ablation-battery design verbatim (per-group removal + compact-subset control for A1). Cite as method precedent, not magnitude evidence.
- Confidence: low. Gap-pass item G-H1-d.

### S-H1-05 — Hybrid physics+AI beats either alone; residual-growth as drift signal (VERIFIED full text)
- Source: https://www.halkwinds.com/research/digital-twin-enterprise-adoption-report — Halkwinds Research 2026, practitioner report → **L4**.
- Claim: "using physics models to constrain and interpret ML outputs… the combination outperforms either approach in isolation"; standard hybrid = physics predicts nominal, AI models the residual; "when the residual grows systematically, it signals genuine asset change or a calibration problem."
- HOW it supports H1: cross-industry pattern that physics terms earn their place via the residual channel — and the residual doubles as H1's wall/diverge ledger signal (growing residual = term or calibration failing).
- Vs A1: reports the combination winning, not low-fi matching high-fi; scoped as practitioner pattern, not experiment.
- Confidence: moderate (no fixed respondent count — report says so explicitly; treat as expert synthesis, not survey).

### Phase-1 cache rows reused (no re-fetch; snowball seeds)
- A-S11 (SPEEDAM 2024): spindle DT with FOC drive + force/vibration ODEs, HIL-validated ML datasets — drive-ODE precedent for current-coupling term.
- A-S9: hybrid thermal model beats load-blind empirical and slow FE — thermal-lag term precedent.
- A-S6: stroke-dependent Archard reproduces wear knee without geometry updates — wear-knee term precedent.
- A-S4 §VII (full text): DT-simulated normal + few anomalies trains Siamese AE to FPR ~9%/FNR ~12% — physics-simulated training data moves detection numbers; same paper's skew/kurtosis-harm warning retained as H1's impulse-term risk flag.

## Net assessment (supporting lane only)
Direction of new evidence favors H1's P1 (≥1 term clears a delta; current/thermal coupling strongest). No finding here marks any term corroborated — synthesis decides after the contradicting lane + battery.
