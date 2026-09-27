# Academic Literature Review — Verdandi anomaly-twin-trace

Date: 2026-09-11 | Reviewer: academic-reviewer | Scope: ACADEMIC base only (no industry/market)
Topic: 32-machine factory twin; topology-constrained PCMCI + quantile detector + template+verifier narration + seeded replay.

## 1. Search strategy (verbatim log)

| # | Query (verbatim) | Tool | Retrieved → Screened → Kept |
|---|---|---|---|
| Q1 | PCMCI causal discovery time series stability short samples tigramite | academic | 8 → 8 → 4 |
| Q2 | GDN graph neural network anomaly detection multivariate time series F1 | academic | 8 → 8 → 2 |
| Q3 | "point-adjusted" F1 overestimates anomaly detection evaluation critique (†) | academic | 8 → 7 → 4 |
| Q4 | PCMCI spurious edges false positive autocorrelation critique (†) | academic | 8 → 8 → 4 |
| Q5 | RCAEval benchmark root cause analysis microservices AC@K | academic | 8 → 8 → 3 |
| Q6 | LLM incident root cause analysis grounding microservices TAMO KRCA | academic | 8 → 8 → 4 |
| Q7 | causal digital twin industrial control system simulation fault injection | academic | 8 → 8 → 4 |
| Q8 | SWaT WADI anomaly detection benchmark TranAD USAD MTAD-GAT raw F1 | academic | 8 → 8 → 3 |
| Q9 | GNN anomaly detection fails generalize illusion progress simple baseline outperforms (†) | tech | 8 → 8 → 4 |
| Q10 | OpenRCA LLM root cause hallucination ungrounded incident analysis (†) | tech | 8 → 6 → 4 |
| Q11 | USAD unsupervised anomaly detection multivariate time series autoencoder OmniAnomaly stochastic (snowball of top-3) | academic | 6 → 6 → 2 |
| Q12 | BARO CIRCA Bayesian metric-based root cause analysis microservices robustness (snowball of top-3) | academic | 6 → 6 → 2 |

(†) = contradiction-seeking query. Total: 12 queries (≥8 required, ≥2 contradiction required — 4 delivered).
Top-3 snowballed: (a) Deng & Hooi GDN 2021 → MTAD-GAT/OmniAnomaly baselines, TranAD comparison; (b) Kim et al. point-adjustment critique → Xu et al. 2018 / Audibert USAD 2020 / Su MAD-GAN lineage, USAD+OmniAnomaly retrieved directly (Q11); (c) RCAEval → BARO, CIRCA retrieved directly (Q12).

## 2. Thematic synthesis

### T1. Causal discovery on time series: PCMCI works, but only inside its contract
- Consensus (STRONG): PCMCI/PCMCI+ (Runge et al., Science Advances 2019; UAI 2020) controls autocorrelation-induced false positives far better than naive PC via the MCI test, which conditions on parents of both variables. Assumptions are explicit in tigramite docs: causal stationarity, no hidden confounders (PCMCI; PCMCI+ relaxes contemporaneous links), faithfulness.
- Stability caveat (STRONG, contradiction retained): short samples + strong autocorrelation degrade power and calibration. Independent robustness studies confirm: PCMCI+ output varies across realizations/sample sizes on hydrological data (AIES 2025); Sandia benchmark (OSTI 1991387) documents temporally-induced spurious causality as the exact failure MCI must prune; biosphere–atmosphere application (Krich et al., Biogeosciences 2020) warns contemporaneous common drivers produce spurious lagged AND contemporaneous links PCMCI cannot resolve. LIPCROSS high-recall variant (NeurIPS 2020) admits no false-positive control proof under latent/nonlinear/autocorrelated settings.
- Remedies with evidence: Bagged-PCMCI+ (Debeire et al., PMLR 2024) improves precision AND recall via bootstrap majority vote plus per-link confidence — directly supports Verdandi's flip-gate/seed-sweep design (tau=2, n≥800, flip-gate).
- Verdict for Verdandi: topology-constrained PCMCI (mask from known machine graph) + short-lag restriction + multi-seed flip gate is academically defensible; blind discovery on 32 nodes with short runs is not. C2 resolution (flip-gated tau-2) STANDS.

### T2. GNN/detector literature: reported SOTA collapses under raw protocol
- Consensus (STRONG): point-adjusted F1 systematically overestimates. Kim et al. (arXiv:2109.05257) prove even random scores become SOTA under PA. Schmidl et al. ("Fancy Algorithms and Flawed Evaluation Methodology", 2023) show point-wise F1 ~0.43 is achievable by predicting everything anomalous; composite/event-wise scores tell the opposite story. Follow-ups (2024–2026) show PA%K, affiliation-F1, and ROC-AUC are likewise gameable by best-of-N seed shopping; only raw point-wise/PR-based metrics stay flat.
- Direct collapse evidence (STRONG, contradiction retained): TCN-GAT on SWaT 0.886→0.281 under fixed-percentile calibration with 53× validation/test score-distribution shift (IEEE ICCI 2026 negative-result study). Raw-protocol leaderboard (SOTA2/WADI): TranAD 49.5, MTAD-GAT 41.7, USAD 30.6 F1 — far below PA-era claims (~0.9). GDN itself (Deng & Hooi, AAAI 2021) reports F1 0.81/0.57-range on its own datasets under its own protocol.
- Generalization critique (MODERATE–STRONG): survey/benchmark papers (2024–2026) find GNNs often lose to MLP/simple baselines (smoothing dilutes anomalies), suffer normality shift across deployments (TUNE/AAAI), OOM at 500k–1M scale, and fail cross-dataset transfer (~random AUROC zero-shot).
- Verdict for Verdandi: per-machine quantile calibration + raw-F1-only admissibility rule is REQUIRED, not optional. Learned-graph detector (GDN-style) is correctly deprioritized vs veto-mask+stats; RQ1 STANDS. Any detector claim must report raw point-wise F1, never PA.

### T3. RCA benchmarks: AC@K ceiling is modest; graph-free robust methods lead
- Consensus (STRONG): RCAEval (Pham et al., arXiv:2412.17015; ICSE 2025 companion; 735 failure cases, 11 fault types, 15 baselines, AC@k/Avg@k metrics) is the reference benchmark. Best reported Avg@5 ≈ 0.46–0.54; DELAY/LOSS network faults are near-death for all methods.
- Nuance: BARO (FSE 2024, Best Artifact) is end-to-end, needs no service-call/causal graph, nonparametric and rotation-invariant — strongest dishonest-baseline for Verdandi's topology-prior claim. CIRCA (intervention-recognition CBN + regression hypothesis testing) is the causal-inference representative but needs architecture-derived parents. "How Far Are We?" (2024) survey stresses interventional-recognition gaps.
- Verdict for Verdandi: AC@1 ≥70% on 20–32 injected faults is above the RCAEval SOTA band — defensible ONLY as real-path/sim-ground-truth result (free labels invert benchmark economics), never as general claim. C3 resolution (0.80–0.8125 real-path, F1 conditional) STANDS with that scoping.

### T4. LLM-RCA grounding: tools + constraints beat open narration
- Consensus (MODERATE–STRONG): TAMO (TSC 2025; tool-assisted LLM agent, multi-modality alignment + localization tools) and KRCA (2026; progressive multi-stage narrowing, 0.88/0.79, −77.3% time) show grounded, tool-using agents cut MTTR substantially. OpenRCA (ICLR 2025; 68GB telemetry, Claude 3.5 RCA-agent solves only simplest cases) and OpenRCA 2.0 (2026; outcome→process labels, intervention-asymmetry argument) document systematic failure: 12 pitfall types across 1,675 runs; ungrounded accuracy ~38.5%; hill-climbing on outcome labels without addressing causation (Traversal critique).
- Verdict for Verdandi: template + verifier + fallback (provenance IDs mandatory, LLM open-endedness deferred to M2) is the academically correct posture. C1 resolution (template-level 1.00 PROVEN, full-LLM open) STANDS.

### T5. Digital-twin simulation: DES + fault injection is established; factory twin stays hand-rolled
- Evidence: co-simulation DT training Bayesian-network diagnostics at equipment+factory levels with fault injection (Sensors 2022); hardware-in-the-loop causRCA machinery dataset with twin-injected valve-leak/filter-clog ground truth (Zenodo 2025); VALOR/DT-based IDS frameworks executing 23 attack scenarios in-twin; causal digital twins for ICS reporting 0.90–0.94 F1 on SWaT/WADI/HAI with 78.4% RCA accuracy (2025 — note: verify PA-vs-raw protocol before citing as support); sandbox PLC/ROS twin cases.
- Verdict for Verdandi: hand-rolled seeded twin with free sim ground truth is standard practice for this scale; FactorySimPy rejection (discrete-only/static) is reasonable. Seeded replay determinism (SeedSequence, 0-diverge) + per-run caps STAND (C4/C6).

## 3. Consensus levels
- STRONG: PA protocol invalid (raw F1 only); PCMCI beats naive CI under autocorrelation but needs stationarity/no-confounders; RCAEval ceiling ~0.5 Avg@5; tool-grounded LLM-RCA > open narration.
- MODERATE: GNN detectors don't transfer across datasets/thresholds; bagging stabilizes PCMCI+; simulation-injected faults are valid RCA ground truth.
- CONTESTED: exact raw-F1 ranking among TranAD/GDN/MTAD-GAT/USAD (protocol-dependent); causal-DT 0.9+ F1 claims (protocol unverified); LLM-RCA % gains across incompatible benchmarks.

## 4. Gaps relevant to Verdandi
1. No published topology-constrained PCMCI + flip-gate study at 32-node factory scale with n≈800 seed sweeps — Verdandi's spike is novel support.
2. No per-machine-quantile ablation published for TCN-GAT-style collapse — Verdandi's veto-mask+quantile result fills this.
3. Template+verifier provenance rate (1.00@50) has no direct comparator — report as template-level, not LLM-level.
4. Raw-protocol detector comparison on the twin's own 20–32 fault battery still owed (M0b).

## 5. Limitations of this review
- Retrieval via Exa-backed academic search; no forward-citation chaining beyond one snowball hop; preprints (arXiv 2025–2026) included and flagged by level.
- No meta-analysis pooled effect sizes; heterogeneity of protocols forbids it.
- Industry/market angle explicitly excluded per task.
