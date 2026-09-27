# H1 supporting — supervised-with-free-labels + unknown-family generalization

Date: 2026-09-13 · Twin ML training · Parent F1 Locked (raw-only, PA-off) · Phase-1 base: A10 (Lau), A06 (Wenig objection), A24 (normal-only limits)

## Registered claim (H1)
Per-family supervised-with-free-labels beats normal-only on twin data incl. unknown-family slice (margin ≥+3pp raw-F1 AND recall_unknown ≥0.30).

## Supporting findings (NEW beyond Phase 1 preferred; Phase-1 rows re-cited only as anchors)

### S-H1-1 — STAND: simple supervised beats complex unsupervised under limited labels (NEW, L3 preprint)
- Source: https://arxiv.org/html/2511.16145v1 (Zhong et al., Nov 2025, "Labels Matter More Than Models", full HTML fetched)
- Claim: STAND (MLP + BiLSTM + linear scorer, BCE loss) "significantly outperforms existing complex unsupervised TSAD methods" under limited labeling budget on 5 public datasets; supervisory gain "far exceeds incremental gains from architectural innovations"; supervised predictions more consistent/localized. Point-wise F1 + AUC-ROC + Aff-F1/UAff-F1 + VUS-PR + CCE metrics.
- Level/confidence: L3 (preprint, peer review unknown) / Medium-High for direction, Medium for magnitude (no twin transfer; metrics include PA-adjacent Aff-F1/VUS companions — raw F1 leg is the admissible one).
- Supports H1 vs alternatives: directly counters A2 (normal-only suffices) on known families — predicts tripwire-1 (≥+3pp) is clearable with even a simple head. Does NOT cover unknown-family slice (gap-pass below).

### S-H1-2 — Lau et al. semi-supervised theory: uniform synthetic + known anomalies, minimax-optimal, gains on known AND unknown (Phase-1 A10 extended with full-text verification, L3)
- Source: https://arxiv.org/html/2506.13955v1 (Lau et al., Jun 2025, full HTML fetched)
- Claim: first mathematical formulation of semi-supervised AD generalizing unsupervised AD; mixing known anomalies with uniform synthetic anomalies (dose n′=n+n⁻) fixes (i) false-negative modeling (low-density normal regions misclassified) and (ii) discontinuity/irregularity of regression function (Props 1–2); ReLU-net excess risk converges at minimax-optimal rate (Thm 2). Empirics on 5 benchmarks (NSL-KDD, Thyroid, Arrhythmia, MVTec, AdvBench): VC-SA beats VC, e.g. NSL-KDD unknown DoS AUPR 0.345→0.793, Probe 0.180→0.649; gains extend to ES/DROCC; known-anomaly signal not washed out at prescribed dose.
- Level/confidence: L3 / High for mechanism (theorem + dose rule), Medium for twin transfer (NOT time-series; tabular/image/language + embeddings; uniform theory ≠ sensor-manifold license — H1's "never uniform-noise default, dose/shape via ablation" proviso stands).
- Supports H1 vs alternatives: supports prediction 2 (recall_unknown ≥0.30 plausible via known+synthetic mix) and prediction 3 (interior dose optimum; dilution/contamination caveats bound the high-dose end); counters A3 (dose poison) at the prescribed dose while licensing A3's monotone-decrease branch at saturating dose.

### S-H1-3 — Han 1% supervision precedent (via Lau §2 citation, SNIPPET-ONLY, flagged)
- Source: Han et al. 2022 as cited in Lau et al. §2 ("even with 1% labeled anomalies, methods incorporating supervision empirically outperform unsupervised AD") — primary not fetched.
- Claim: 1%-label supervision beats unsupervised baselines.
- Level/confidence: L2-cited-but-UNVERIFIED / Low-Medium (snippet-only, never sole support for STRONG claim).
- Supports H1 vs alternatives: directional support for tripwire-1 at low label budgets (twin is label-rich: free sim labels, so 1% is a floor not a ceiling).

### S-H1-4 — DevNet few-shot deviation learning, 0.005–1% labeled anomalies (NEW, L1-anchored via abstract+repo, full PDF not opened)
- Source: https://doi.org/10.48550/arxiv.1911.08623 (Pang et al., KDD19 DevNet abstract via search) + https://github.com/GuansongPang/deviation-network (repo README: "leverages a limited number of labeled anomaly data... 0.005%–1% of all training data")
- Claim: Z-score deviation loss with Gaussian prior directly optimizes anomaly scores from dozens of labeled anomalies; beats unsupervised counterparts.
- Level/confidence: L1 (KDD19 peer-reviewed) for existence/direction at abstract level; Low-Medium here (abstract+repo only, TEXT-ONLY rule — full text not opened) / Medium for H1 relevance.
- Supports H1 vs alternatives: second independent witness (with Han) that tiny supervision moves the needle — counters A2; deviation-loss/Z-score formulation is a candidate supervised-head design for H1 Arm S (abrupt→supervised head).

### S-H1-5 — Open-set supervised AD: seen + pseudo + latent-residual abnormalities for unseen anomalies (NEW, L3 preprint abstract, SNIPPET-ONLY)
- Source: https://doi.org/10.48550/arxiv.2203.14506 ("Catching Both Gray and Black Swans", Mar 2022, abstract via search) + https://doi.org/10.48550/arxiv.2310.12790 (Anomaly Heterogeneity Learning for OSAD, Oct 2023, abstract via search)
- Claim: OSAD explicitly trains on seen anomalies + pseudo anomalies + latent residual anomalies to detect unseen (open-set) anomaly classes; 9-datasetE outperformance on seen AND unseen.
- Level/confidence: L3 SNIPPET-ONLY / Low-Medium (never sole support).
- Supports H1 vs alternatives: mechanism precedent for H1's unknown-family hedge (recall_unknown ≥0.30): per-family formulation + pseudo-anomaly augmentation is exactly the OSAD recipe; counters A1 (caricature overfit) as a fate to design against, not a destiny.

## How support maps to H1 tripwires
- Tripwire-1 (S−N ≥+3pp raw-F1): S-H1-1 (STAND), S-H1-3 (Han 1%), S-H1-4 (DevNet) — three independent directions, none twin-scoped; numbers NOT transferable, margins must clear on M0b.
- Tripwire-2 (recall_unknown ≥0.30): S-H1-2 (Lau known+unknown gains + dose rule), S-H1-5 (OSAD recipe) — mechanism + dose discipline, not guarantees.
- Dose ablation (prediction 3): S-H1-2 prescribes n′=n+n⁻ starting dose with dilution/contamination caveats → H1's ≥3-dose-level ablation is the twin instantiation.

## Gap-pass per sub-hypothesis
- Known-family margin: SUPPORTED (direction) — follow-up query: "STAND BCE supervised vs reconstruction unsupervised point-wise raw F1 5-dataset full table" (verify margin sizes under raw protocol).
- Unknown-family recall ≥0.30: PARTIAL — Lau is non-temporal, OSAD abstracts are snippet-only. Follow-up queries: "open-set supervised time series anomaly detection unseen fault family recall" ; "semi-supervised TSAD known+synthetic unknown slice ablation".
- Dose interior optimum on sensor manifolds: PARTIAL — follow-up query: "synthetic anomaly dose-response time series sensor data dilution contamination ablation".
