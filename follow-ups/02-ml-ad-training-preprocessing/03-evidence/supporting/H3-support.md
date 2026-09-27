# H3 supporting — state-as-covariate single model beats filter-only and per-state on STARVED-heavy

Date: 2026-09-13 · Twin ML training · Parent F1 Locked · Phase-1 base: A05 (M2AD), A11 (missingness/route structure), A19 (steady-state pipeline snippet)

## Registered claim (H3)
Single-model-with-state-covariate + DOWN-masking beats filter-only (≥+2pp) AND per-state models (>0) on STARVED-heavy episodes (≥40% STARVED steps).

## Supporting findings

### S-H3-1 — M2AD STATUS covariates + GMM+Gamma dependence-correct calibration (Phase-1 A05 re-verified abstract-level, L1)
- Source: https://proceedings.mlr.press/v258/alnegheimish25a.html (PMLR v258, AISTATS25, fetched — abstract+bib; full PDF not opened per TEXT-ONLY rule)
- Claim: framework takes categorical asset STATUS covariates alongside sensors; per-sensor GMM → p-values → Fisher → Gamma calibration (χ² shown miscalibrated, Prop. 2); area error; 130-asset Amazon case study. Headline +21% overlap-TP is INADMISSIBLE as number (mechanics transfer only).
- Level/confidence: L1 / High for covariate+calibration mechanics, Low for magnitude.
- Supports H3 vs alternatives: licensed precedent for "state as input covariate, not just filter key" (Arm C design) + regime-conditioned thresholds leg (prediction 3). Counters A2 (filtering suffices) at mechanism level: dependence/heterogeneity across sensors/systems needs explicit conditioning.

### S-H3-2 — Product-aware (mode-specific) vs global-agnostic: +14.6% F1, blind-spot stress test 22.2% vs 100% detection (NEW, L3 preprint full-text)
- Source: https://arxiv.org/html/2606.00052 (Islam & Carden, May 2026, TEP Modes 1/3/5, full HTML fetched)
- Claim: product-aware autoencoder bank F1 0.5322 vs global 0.4643 (+14.62%), ROC-AUC 0.7220 vs 0.6805 (+6.10%); unannounced mode-transition stress: global detection as low as 22.2% (Mode1→5), product-aware 100%; per-mode 95th-percentile thresholds (Eq.7).
- Level/confidence: L3 (preprint) / Medium-High for direction (controlled TEP benchmark), Medium for twin transfer (chemical-process, not line STARVED physics; supports conditioning principle, not the C-vs-P ordering).
- Supports H3 vs alternatives: strongest NEW witness that ignoring operating mode creates blind spots — supports tripwire-1 direction (C−F ≥+2pp). NUANCE for A1: this paper's winner is per-mode bank (Arm P family!), not single-model-covariate — it simultaneously supports conditioning and warns that A1 (per-class wins) is live. H3's distinguisher (per-class arm alongside P) is thus mandatory, not optional.

### S-H3-3 — State-conditioned association learning (SCAL): normal-lag profile + discrepancy-weighted scoring, top F1 on SWaT/WADI/HAI/ADAPT (NEW, L2 journal, SNIPPET-ONLY)
- Source: https://iopscience.iop.org/article/10.1088/1361-6501/aea242 (Meas. Sci. Technol., abstract via search; full text not opened)
- Claim: fixed empirical normal-lag profile per state; discrepancy weights reconstruction error; avg F1 95.60% across 3 benchmarks, 97.69% ADAPT; Anomaly-Transformer missed-alarm 13.71%→1.00%.
- Level/confidence: L2 SNIPPET-ONLY / Low-Medium (never sole support; metric protocol — raw vs PA — unverified from abstract).
- Supports H3 vs alternatives: second conditioning-mechanism witness (state-conditioned profiles ≈ covariate + regime thresholds); counters A2.

### S-H3-4 — DCD-VAE disentangles operating-condition vs operating-state features (NEW, L2-adjacent proceedings PDF abstract, SNIPPET-ONLY)
- Source: DCD-VAE paper via search ("Unsupervised anomaly detection of machines operating under time-varying conditions", Politecnico repository PDF)
- Claim: distribution-constraint decomposition separates OC-linked vs OS-linked latent features under time-varying conditions.
- Level/confidence: L3/L2 SNIPPET-ONLY / Low (single study, abstract only).
- Supports H3 vs alternatives: representation-level precedent for covariate modeling (C arm) over filtering; relevant if STARVED offsets entangle with fault signatures.

### S-H3-5 — Steady-state-gated pipeline precedent: sync → steady-state detect → reconcile → regime-ID, ~32% steady-state (Phase-1 A19 + NEW PHM circuit-breaker L1+ variant, snippet-level)
- Sources: A19 (https://www.mdpi.com/2227-9717/11/8/2376, snippet, 403-blocked) + https://papers.phmsociety.org/index.php/phme/article/download/4896/2947 (PHM-EU, fetched via search abstract: "separate statistical tolerance bands for discrete operating condition strata... L1+ restricts training to steady-state thermal")
- Claim: regime-stratified tolerance bands; steady-state-restricted training variant.
- Level/confidence: L4/L3 SNIPPET-ONLY / Low-Medium.
- Supports H3 vs alternatives: DOWN-masking + RUN-normal-only training discipline (Thibault-style steady-state pipeline): exclude unlearnable/non-representative strata from baseline — counters A3-oblivious designs (masking ablation in H3 isolates this leg).

## How support maps to H3 tripwires
- Tripwire-1 (C−F ≥+2pp STARVED-heavy): S-H3-1 (mechanism), S-H3-2 (+14.6% F1 direction + blind-spot stress), S-H3-3/S-H3-4 (conditioning witnesses).
- Tripwire-2 (C−P >0): NOT supported as stated — S-H3-2 favors P-family over global; H3 survives only if single-covariate beats per-state on twin STARVED data (data-thin STARVED-normal argument, prediction 2, awaits battery).
- Threshold leg (prediction 3, ≥1pp from regime-conditioned thresholds): S-H3-1 (Gamma calibration) + S-H3-2 (per-mode P95 thresholds) — mechanism supported.

## Gap-pass per sub-hypothesis
- Covariate-vs-filter margin on STARVED-heavy: SUPPORTED (direction) — follow-up query: "STARVED blocked starved state covariate vs filter-only anomaly detection F1".
- Single-covariate vs per-state ordering: GAP/CONTESTED (S-H3-2 cuts toward A1) — follow-up query: "single model with mode covariate vs per-mode ensemble anomaly detection F1 comparison".
- DOWN-masking isolated contribution: PARTIAL (S-H3-5 steady-state-gating precedent) — follow-up query: "steady-state gated training downtime masking anomaly detection ablation".
