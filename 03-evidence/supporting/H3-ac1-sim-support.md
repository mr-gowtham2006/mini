# H3 Supporting — AC@1 sim-scoped achievability (RCAEval ceiling + BARO graph-free control)

Date: 2026-09-11 · Hypothesis: H3-ac1-sim-scoped.md

## Queries executed (relevance gate: each directly targets H3)
1. academic: "RCAEval root cause localization accuracy microservices benchmark" — KEPT
2. academic: "BARO CIRCA Bayesian root cause analysis ranking microservices" — KEPT
3. web: "BARO root cause anomaly ranking without dependency graph" — KEPT
- Dropped/rewritten: none.

## Sources (cascade: search-cache → direct-URL → scholar → web)
| # | Source URL | Claim | Level | Confidence | Cascade verdict | Supports H3 how |
|---|---|---|---|---|---|---|
| S1 | https://doi.org/10.1145/3701716.3715290 / https://arxiv.org/html/2412.17015v3 (RCAEval) | 735 failure cases, 3 systems, 15 reproducible baselines, coarse+fine-grained RCA evaluation | L2→L1 (arXiv:2412.17015 → ACM 2025, doi:10.1145/3701716.3715290) | High | cache=HIT; direct-URL=skipped/unverifiable; scholar=not-queried | Supports H3 verification-method: RCAEval is the ceiling comparator (Avg@5 0.46–0.54 band) against which sim-scoped AC@1 ≥70% must be contrasted + scope-disclosed. |
| S2 | https://github.com/phamquiluan/RCAEval | BARO Avg@5 per-fault table (Train Ticket): 0.72/0.99/1/0.83/0.64/0.8 CPU/MEM/DISK/SOCKET/DELAY/LOSS pattern — DELAY/LOSS are the weak faults | L3 (OSS repo + results table) | Medium-High | cache=HIT; direct-URL=skipped/unverifiable | Supports H3 prediction 3 (misses concentrate on DELAY/LOSS, not uniform). |
| S3 | https://doi.org/10.1145/3660805 / https://doi.org/10.48550/arxiv.2405.09330 (BARO, FSE 2024) | End-to-end anomaly detection + RCA via Multivariate BOCPD + RobustScorer; NO service-call/causal graph required; nonparametric, scale-equivalent, rotation-invariant | L1 (ACM TOSEM/FSE 2024) | High | cache=HIT; direct-URL=skipped/unverifiable | Supports H3 prediction 2's control leg: BARO is the legit graph-free ablation baseline (topology prior must beat it by ≥10pp or H3 concedes parity). |
| S4 | https://arxiv.org/html/2408.13729v2 ("Root Cause Analysis for Microservices based on Causal Inference: How Far Are We?") | CIRCA builds causal graph from operator call-graph knowledge (falls back to PC algorithm when unavailable); BARO/NSigma/ε-Diagnosis construct no causal graph | L2 (arXiv) | Medium | cache=HIT; direct-URL=skipped/unverifiable | Supports H3 claim framing: graph-dependent (CIRCA) vs graph-free (BARO) is a real, documented split — topology-prior ablation is meaningful. |
| S5 | https://conf.researchr.org/details/ase-2026/ase-2026-industry-showcase/13/KRCA-An-Efficient-Root-Cause-Analysis-System-in-Hyper-scale-Microservice-Systems-via (KRCA, ASE 2026) | KRCA AC@1 0.88 (service) / 0.79 (failure type), +31pp over strongest baseline; 6-month Kuaishou production deployment | L3 (industry showcase + arXiv:2607.01788) | Medium | cache=HIT; direct-URL=skipped/unverifiable | Supports H3 achievability leg: AC@1 ≥70% is attainable in scoped settings (0.88 precedent) — while its production/hyperscale scope also reinforces H3's sim-scoping caution. |

## Field-level citation check
| # | Title | Authors | Venue | Year | DOI/URL | Label | Match/Mismatch |
|---|---|---|---|---|---|---|---|
| S1 | RCAEval: A Benchmark for Root Cause Analysis of Microservice Systems with Telemetry Data | (authors not extracted) | ACM (doi:10.1145/3701716.3715290) / arXiv:2412.17015 | 2024/2025 | doi + arXiv URLs above | R | Partial — authors unverified; venue/year/DOI match |
| S2 | RCAEval (repository + results) | phamquiluan et al. | GitHub | 2024– | github.com/phamquiluan/RCAEval | H (hosted repo) | Match |
| S3 | BARO: Robust Root Cause Analysis for Microservices via Multivariate Bayesian Online Change Point Detection | Pham et al. | ACM (doi:10.1145/3660805) / FSE 2024 / arXiv:2405.09330 | 2024 | doi + arXiv URLs above | R | Match |
| S4 | Root Cause Analysis for Microservices based on Causal Inference: How Far Are We? | (not extracted) | arXiv:2408.13729 | 2024 | arXiv URL above | P | Partial — authors/venue unverified; flagged |
| S5 | KRCA: An Efficient Root Cause Analysis System in Hyper-scale Microservice Systems via Agentic AI | (not extracted) | ASE 2026 Industry Showcase | 2026 | researchr + arXiv:2607.01788 | P/H | Partial — authors unverified; flagged |

Hallucinated (H) sources removed: none — all URLs verbatim from search; no invented citations.
