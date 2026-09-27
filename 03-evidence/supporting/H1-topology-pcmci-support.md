# H1 Supporting — Topology-masked PCMCI + flip-gate (bagging stability)

Date: 2026-09-11 · Hypothesis: H1-topology-pcmci-flip-gate.md

## Queries executed (relevance gate: each directly targets H1)
1. academic: "PCMCI topology constrained causal discovery manufacturing sensor" — KEPT
2. academic: "bagged bootstrap PCMCI stability causal discovery Debeire" — KEPT
3. tech: "PCMCI+ tigramite topology prior causal graph discovery" — KEPT
4. gap-pass academic: "PCMCI false positive spurious links high-dimensional time series autocorrelation" — KEPT
- Dropped/rewritten: none (all 4 passed gate on first formulation).

## Sources (cascade: search-cache → direct-URL → scholar → web)
| # | Source URL | Claim | Level | Confidence | Cascade verdict | Supports H1 how |
|---|---|---|---|---|---|---|
| S1 | https://proceedings.mlr.press/v236/debeire24a.html (also arXiv:2306.08946) | Bagged-PCMCI+ (bootstrap aggregation + majority vote) improves precision AND recall vs base PCMCI+, plus per-link confidence measures | L2 (PMLR CLeaR 2024 proceedings + arXiv preprint) | High | cache=HIT; direct-URL=skipped/unverifiable (no fetch this pass); scholar=not-queried; web=n/a | Direct support for H1's flip-gate/seed-sweep mechanism: bootstrapped re-runs + majority voting stabilise the edge set — same principle as H1's ≥5-seed flip-rate gate. Comparator only (cannot confirm topology-mask leg). |
| S2 | https://proceedings.mlr.press/v124/runge20a/runge20a.pdf | PCMCI+ improves CI-test reliability via optimised conditioning sets; order-independent, consistent in oracle case; higher adjacency detection power incl. contemporaneous links | L1 (PMLR v124, UAI 2020) | High | cache=HIT; direct-URL=skipped/unverifiable; scholar=not-queried | Supports tau/contDM architecture choice in H1 (PCMCI+ with tau=2 as the base algorithm whose stability the mask + flip-gate then improve). |
| S3 | https://doi.org/10.1021/acs.iecr.4c01155 | Causal discovery for topology reconstruction in industrial chemical processes; flags temporal aggregation/subsampling/unobserved confounders as false-prediction drivers | L1 (Ind. Eng. Chem. Res., peer-reviewed) | Medium | cache=HIT (snippet only); direct-URL=skipped/unverifiable | Supports H1's blind-vs-masked contrast: unmasked discovery on process data is confounder-prone → motivates topology mask as false-positive guard. |
| S4 | https://doi.org/10.48550/arxiv.2407.12254 (COKE) | Chronological-order + expert-knowledge initial graph, then PNS edge modify (remove/add per expert knowledge) before selection | L2 (arXiv preprint) | Medium | cache=HIT; direct-URL=skipped/unverifiable | Supports H1's topology-prior leg: expert/topology-constrained initial graph is an established pattern (remove/add edges per domain knowledge), precedent for P&ID mask. |
| S5 | https://doi.org/10.2172/1991387 (Benchmarking PCMCI for Spatiotemporal Systems) | PCMCI phase-2 MCI tests prune temporally-induced spurious causality by conditioning on lagged observations; reduces PC1 false-positive rate | L3 (US DOE tech report / benchmark) | Medium | cache=HIT (gap-pass); direct-URL=skipped/unverifiable | Supports H1 prediction 2 framing (spurious = mask-absent edges): autocorrelation-induced spurious edges are the known failure mode the flip-gate + mask target. |

## Field-level citation check
| # | Title | Authors | Venue | Year | DOI/URL | Label | Match/Mismatch |
|---|---|---|---|---|---|---|---|
| S1 | Bootstrap aggregation and confidence measures to improve time series causal discovery | Debeire et al. | PMLR v236 (CLeaR 2024) | 2024 | proceedings.mlr.press/v236/debeire24a.html + arXiv:2306.08946 | P (preprint+proceedings) | Match (title/venue/year consistent across PMLR + arXiv + DLR e-lib copies) |
| S2 | Discovering contemporaneous and lagged causal relations in autocorrelated nonlinear time series datasets | Runge | PMLR v124 (UAI) | 2020 | proceedings.mlr.press/v124/runge20a | R (peer-reviewed proceedings) | Match |
| S3 | Causal Discovery for Topology Reconstruction in Industrial Chemical Processes | (not extracted — snippet only) | Ind. Eng. Chem. Res. | 2024 | doi:10.1021/acs.iecr.4c01155 | R | Partial — authors unverified (snippet only); flagged for full-fetch pass |
| S4 | COKE: Causal Discovery with Chronological Order and Expert Knowledge… | (not extracted) | arXiv | 2024 | arXiv:2407.12254 | P | Partial — authors/venue-details unverified; flagged |
| S5 | Benchmarking the PCMCI Causal Discovery Algorithm for Spatiotemporal Systems | (not extracted) | US DOE / OSTI report | 2023 | doi:10.2172/1991387 | H (grey/tech-report) | Partial — authors unverified; flagged |

Labels: R = peer-reviewed · P = preprint/proceedings-preprint copy · H = grey/hosted (report/repo/docs/blog).
Hallucinated (H) sources removed: none — all 5 URLs returned verbatim by search; no invented citations. S3–S5 author fields marked unverified rather than filled from memory.
