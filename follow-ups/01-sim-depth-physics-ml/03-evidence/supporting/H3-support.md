# H3 supporting evidence — Precision-side bar alongside F1

Date: 2026-09-13 · Scope: 32-machine SimPy twin · Battery guards: Locked F1 (bar is alongside, never replacing raw-F1), F2, F3 respected.
Claim tested: ≥1 top-quartile raw-F1 detector setting breaches the pre-registered budget (>10 alerts/1000 healthy windows on fault-free replay) and is demoted; rank-reversal persists at 5 and 20/1000.

## Findings (supporting only)

### S-H3-01 — Alarm fatigue is an accuracy problem; 50+ detections suppressed on one compressor (VERIFIED full text)
- Source: https://www.augury.com/blog/machine-health/why-your-team-has-stopped-trusting-their-predictive-maintenance-alerts/ — Augury (vendor practitioner blog, Jun 2026) → **L4**.
- Claim: "most teams think it's a volume problem. It's not. It's an accuracy problem"; spike (process fluctuation) vs trend (gradual consistent change) distinction is "everything"; every detection vetted by CAT II–IV analyst; case: 50+ detections on one high-pressure compressor in a year, zero sent as alarms (all process fluctuation), so that when an alarm IS sent "the customer knows it's real."
- HOW it supports H3: field-operating proof that a precision mechanism (human veto gate ≈ VETO_ASM2 + flip-gate analogue: suppress spike-without-trend) demotes high-sensitivity detections the raw detector would keep. Directly predicts H3-P2 (veto/flip-gate measurably cuts healthy-window rate).
- Vs alternative A1 (budget miscalibration artifact): the demotion criterion here is analyst-adjudicated ground process (fluctuation vs trend), not a tuned rate threshold — demotion survives any budget.
- Confidence: moderate (vendor blog; single anecdote; no rates published). Downgraded from L3: vendor-owned channel.

### S-H3-02 — Follow-through 94%→43%; trust set by worst zone, not average (VERIFIED full text)
- Source: https://dev.to/assettechinsights/designing-for-operator-trust-in-industrial-aiot-the-engineering-problem-nobody-talks-about-1nf0 — AssetTech, DEV community, Jul 2026 → **L5** (anonymous practitioner essay; treat as mechanism hypothesis, not measurement).
- Claim: alert follow-through 0.94 (wk1) → 0.81 → 0.67 → 0.43 (wk16) while model shows precision 0.86 / recall 0.91; "86% aggregate precision may have 94% in most zones and 60% in one specific zone — trust is determined by the worst zone"; remedies: per-zone/equipment/sensor precision monitoring, proactive threshold recalibration on baseline-shift triggers, <30 s operator-evaluable alert context, follow-through rate as first-class metric.
- HOW it supports H3: the exact rank-reversal H3 predicts (high-F1 setting rejected on trust grounds) with a stronger prescription — the bar should be per-context (per-machine/per-channel), not just global 10/1000. Supports H3-P1 mechanism and sharpens the battery design (record alerts/1000 per machine, flag worst-machine breach).
- Vs A1: proposes the same robustness answer H3 pre-registers (multi-budget check) plus a per-zone decomposition that makes threshold-artifact less likely.
- Confidence: low (anonymous, no data). Mechanism-grade only.

### S-H3-03 — 87,000 alerts/month, 3/50 actionable; 20:1 audit rule (VERIFIED full text)
- Source: https://21tech.com/your-predictive-maintenance-platform-generates-100000-alerts-a-day-your-team-reads-12/ — 21Tech, Apr 2026 → **L5** (trade analysis; case numbers unaudited).
- Claim: Ohio plant, 340 assets, $1.8M sensors: 87,000+ alerts first month; crew investigates ~50/day, ~3 lead to action (47 FPs from "sensor drift, ambient temperature swings, or calibration decay"); missed spindle bearing failure 11 days ($340k); remedy target 35–40 prescriptive/day; "audit your alert-to-action ratio… if it exceeds 20:1, you have an alert fatigue problem"; multi-sensor fusion cuts FP ≥60% (vendor-reported, LOW credibility — do not cite magnitude).
- HOW it supports H3: field quantification of the breach regime H3's budget encodes (unbudgeted detectors flood 1000× over); the 20:1 alert-to-action audit is an independent convergence on H3's rate-based demotion rule; FP-cause list (drift/ambient/calibration) links H3's bar to H5's calibration rule.
- Confidence: low-moderate for magnitudes (unaudited anecdote + secondhand stats); moderate for the mechanism (alert-to-action audit).

### S-H3-04 — Calibrated thresholds with alarms-per-hour reporting; NAB +17% (VERIFIED full text)
- Source: https://www.nature.com/articles/s41598-026-48227-6 — Khan et al., Sci Rep 2026 → **L2**.
- Claim: Platt scaling → ECE ≈0.03; "operating points determined through threshold-sensitivity analyses that report achievable TPR, FPR and alarm rates per hour, thereby linking technical performance to control-room decision-making"; early-warning governance +17% NAB over Isolation Forest; "fixed thresholds can become unstable… elevated false alarms under benign distribution shifts" (lit-review synthesis).
- HOW it supports H3: peer-reviewed instance of exactly H3's apparatus — sensitivity-tuned operating points ranked WITH a false-alarm/rate budget (alarms-per-hour ≈ alerts/1000 windows), calibrated probabilities as the demotion currency. Supports H3-P3 (sensitivity tuning trades F1 rank for budget compliance) and parallels the 5/20 robustness check (threshold-sensitivity analysis).
- Confidence: moderate-high. Caveat: TEP benchmark scope; single operating-point family.

### Phase-1 cache rows reused (no re-fetch)
- I23/I24: EEMUA 191 ≈1 alarm/10 min steady-state (HSE-endorsed) — the normative anchor H3's 10/1000 budget scales from.
- C05 (dev.to postmortem): 94% precision / 89% recall in test → production collapse via FP shutdown + missed failure — the rank-reversal parable.
- I12: 5-s intervals; dropout→FP fixed by sensor-health flag; suppression windows — precision mechanisms with field precedent.
- A-S4: FPR ~9% / FNR ~12% realistic ceiling — the sensitivity-side bound H3's bar complements.

## Net assessment (supporting lane only)
Convergent support across L2 (calibrated rate-budget operating points), L4 (veto-gate field case), L5 (follow-through dynamics, flood quantification). Strongest strengthening vs registry: S-H3-02's per-zone worst-case argument — recommend the battery record per-machine alert rates, not only the global 10/1000.
