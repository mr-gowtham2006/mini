# H1-contra — Devil's advocate: supervised-with-free-labels FAILS (normal-only wins / caricature-keying)

Date: 2026-09-13 · Scope: contradict H1 (`H1-supervised-with-free-labels.md`).
Falsification target: fire tripwire-1 (`F1_raw(S) − F1_raw(N) < 0.03`) or tripwire-2 (`recall_unknown(S) < 0.30`).
Method: 5 falsification queries (all 2026-09-13), 1 verification fetch. TEXT ONLY; no invented sources.

## Query log (5/5 — quota met before verdict)
| # | Query | Engine | Yield |
|---|---|---|---|
| Q1 | Schmidl time series anomaly detection supervised methods fail unseen anomalies generalization benchmark | academic | Schmidl et al. VLDB'22 finding 2 (VERIFIED via fetch); DRA/DevNet open-set failure |
| Q2 | synthetic anomaly augmentation hurts generalization TSAD misdosed augmentation | academic | RedLamp diversity-gap + false-anomaly; NCAD/AnomalyBert heuristic-coverage critique; fixed-augmentation generalization drop (Tack/Sohn line) |
| Q3 | unsupervised one-class beats supervised TSAD unknown anomaly types 2024–2025 | websearch | RoCA/RoC/FOCA/NeuCoReClass one-class lines; TimeRCD: fixed-shape memorization fails zero-shot |
| Q4 | open-set supervised AD DevNet unseen anomalies worse than unsupervised detector bias | academic | DRA abstract VERIFIED (arxiv abs fetch); IJCAI'21 bias-effect paper |
| Q5 | simulated synthetic fault training fails real incipient faults sim-to-real gap caricature | academic | Sim2Real fault-diagnosis gap literature; reality-gap calibration requirement |

## Contra evidence (fires at H1)
- **C1 — Schmidl et al., VLDB 2022 (71 algos, 976 datasets), General Finding 2, VERIFIED** (`https://timeeval.github.io/evaluation-paper/`): *"Despite that supervised algorithms use additional information during training (labels for normal and anomalous points), they do not achieve superior results compared to semi-supervised or even unsupervised approaches."* Direct precedent for tripwire-1 firing. Weight: HIGH (largest TSAD benchmark; threshold-agnostic AUC metrics).
- **C2 — Ding et al., DRA, CVPR 2022 (VERIFIED abstract `https://arxiv.org/abs/2203.14506` + paper text):** DevNet *"improves over KDAD in detecting the seen anomalies but fails to discriminate unseen anomalies from normal samples"*; *"supervised models can be biased by the given anomaly examples and become less effective in detecting unseen anomalies than unsupervised detectors (DevNet vs KDAD on Tile)."* Direct precedent for tripwire-2 firing (`recall_unknown < 0.30` mechanism: seen-class bias). Weight: HIGH.
- **C3 — Pang et al., IJCAI 2021, "Understanding the Effect of Bias in Deep Anomaly Detection" (`https://doi.org/10.24963/ijcai.2021/456`):** labeled anomalies similar to normal data warp the scoring function; TPR on dissimilar unseen classes drops >50% in their Cellular-Spectrum case. Mechanism for H1-A1 (caricature overfit) on the twin's 4–7σ rectangulars. Weight: MEDIUM-HIGH.
- **C4 — RedLamp (Obata et al., 2025, `https://arxiv.org/html/2505.20765v1`):** pseudo-anomaly training suffers (1) **diversity gap** (augmentations fail to cover all anomalies → high FNR) and (2) **false anomalies** (augmentations generate normal-looking samples → overfit → high FPR); binary-assumption methods (NCAD, CutAddPaste, COUTA) are overconfident in generated labels. Direct precedent for H1-A3 (dose poison) and against "synthetic dose always helps." Weight: MEDIUM-HIGH.
- **C5 — Fixed-augmentation generalization drop (`https://arxiv.org/html/2406.10617v1`):** fixed augmentations for all ID samples *"perform well on standard benchmarks but significantly drop when tested for generalization."* Fires at H1 Prediction 3 (interior dose optimum may not exist — monotone-decreasing A3 branch). Weight: MEDIUM.
- **C6 — TimeRCD (2025, `https://arxiv.org/html/2509.21190v5`):** morphological augmentation (jitter/mask) *"risks reinforcing the bias that specific shapes are intrinsically normal"* via semantic coupling; relational/contextual formulation needed. The twin's rectangular faults are exactly "specific shapes" — supervision on them teaches shape identity, not deviation-from-context. Weight: MEDIUM.
- **C7 — Sim2Real fault-diagnosis gap (TMech 2025 CSRA `https://doi.org/10.1109/tmech.2025.3599061`; JIM 2026 review `https://doi.org/10.1007/s10845-026-02795-6`; OSF 2026 manufacturing review `https://osf.io/86cgu`):** models trained on simulated faults need explicit domain adaptation (contrastive transfer, MD-CGAN alignment, reality-gap analysis) to survive contact with real data; naive synthetic-trained transfer degrades. Twin free labels are sim labels — same gap applies to the unknown-family slice. Weight: MEDIUM (domain-adjacent, not TSAD-protocol-identical).

## Supporting-side hits found (disclosed, not suppressed)
- S1: DRA itself shows supervised + open-set machinery CAN hold unseen recall — but only with disentangled residual/pseudo-anomaly heads, i.e. more than H1's plain per-family heads. Narrows, not kills, C2.
- S2: CutAddPaste/CAPMix/IGCL lines show assumption-driven synthetic supervision beating pure normal-only baselines on known families — supports H1 tripwire-1 leg, not tripwire-2.
- S3: Schmidl metrics are threshold-agnostic AUC (not raw-F1 PA-off); supervised-vs-one-class gap could compress or flip under the F1 lock. Scope caveat on C1.

## Verdict: PROVISIONALLY FALSIFIED (tripwire-2 leg; tripwire-1 leg survives)
The unknown-family leg (`recall_unknown(S) ≥ 0.30`) is under severe, multi-source threat (C2+C3+C6 mechanism + C1 precedent): naive per-family supervision on 4–7σ rectangulars is exactly the regime where published supervised models collapse on unseen classes. The known-family margin leg is NOT falsified (S1/S2 + free-label density). Net: H1 cannot survive as stated without an open-set/disentangled head or a dose-ablation proof; fallback (normal-only + drift-chain, labels for windowing discipline only) is the live alternative. Final kill requires the M0b hedge split — literature alone cannot fire the numeric tripwire on twin data.
