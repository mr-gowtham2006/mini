# Contradicting evidence — H3 (ranked trace AC@1 ≥70%, sim-scoped)

Date: 2026-09-11 · Agent: falsification-agent · Target: H3-ac1-sim-scoped.md
Tripwire: falsified by AC@1 <0.70 on full battery (<14/20, <23/32), all seeds counted.

## Queries (5, cascade academic→tech→web)
1. (academic) "BARO Bayesian root cause localization top-1 accuracy microservice benchmark"
2. (academic) "causal root cause analysis top-k accuracy 0.9 microservice anomaly localization CausalRCA"
3. (tech) "root cause localization AC@1 accuracy benchmark Nezha Eadro Groot AIOps"
4. (academic) "SimpleRCA baseline matches state of the art root cause analysis microservice evaluation"
5. (web) "RCAEval benchmark root cause analysis BARO CIRCA RCD average accuracy low 0.5"

## Relevance gate
Kept sources with Top-1/AC@1/Avg@5 numbers on named benchmarks. Dropped vendor marketing without numbers and the NOFire AI-SRE PDF (commercial comparison, out of scope).

## Findings (strongest contradicting first)
- C-H3-1 (ceiling-premise counter): Eadro (ICSE 2023) — HR@1 0.982, HR@5 0.990 avg; 290%–5068% above baselines. Nezha (FSE'23) 0.87; SimpleRCA re-evaluation 0.93 on Nezha-TT. AC@1≥70% is ROUTINE on microservice benchmarks → H3's "0.46–0.54 ceiling" does not generalize; the sim-scoped 70% target is unambitious, not bold.
- C-H3-2 (ablation-gap counter): "Rethinking the Evaluation of Microservice RCA" (ACM 3797100 / arXiv 2510.04711) — SimpleRCA (3-sigma/P95 rules, NO topology prior) matches or beats 11 SOTA models (0.93 vs Nezha 0.87; 0.83 vs BARO 0.50/0.00). Directly threatens H3's "ablation gap ≥10pp for topology prior" expected outcome.
- C-H3-3 (mechanism counter): "Root Cause Analysis for Microservices based on Causal Inference: How Far Are We?" (ASE'24, arXiv 2408.13729) — PC/FCI/Granger/LiNGAM/fGES causal methods "mostly perform similarly to Dummy (random)". Causal-graph routing adds ~nothing on their systems.
- C-H3-4 (supports H3's ceiling on one benchmark): RCAEval (WWW 2025, arXiv 2412.17015) — best avg Avg@5 CIRCA 0.46 / RCD 0.54; BARO Avg@5 0.47 with AC@1 rows as low as 0.0–0.33 on network faults. On RCAEval-style data the ceiling is real.
- C-H3-5 (partial counter): Groot (ASE 2021, eBay production, 952 incidents) — top-1 78%, top-3 95% with event-graph + domain rules. Graph-structured ranking clears 70% top-1 in production, but WITH heavy domain rules (not a pure topology prior).

## Gap-pass
No retrieved study runs ranked-trace depth≤3 over a topology prior on the seeded twin battery. Whether the twin's faults look like RCAEval-hard (C-H3-4) or Nezha-easy (C-H3-1) is unknown — the battery decides.

## Field-level citation check (R/P/H)
| ID | Reported number | Precise (AC@1, twin-like?) | Hits tripwire? |
|---|---|---|---|
| C-H3-1 | Eadro HR@1 0.982; SimpleRCA 0.93 | Metric yes, system no (microservices) | No (different battery; but voids ceiling premise) |
| C-H3-2 | SimpleRCA ≥ SOTA w/o prior | Yes vs SOTA, microservices | No (voids ablation-gap expectation, not the 0.70 bar) |
| C-H3-3 | causal ≈ random | Partial | No |
| C-H3-4 | best Avg@5 0.46–0.54 | Supports H3 | No (supports) |
| C-H3-5 | Groot top-1 78% | Partial (rules-assisted) | No |

## Verdict: PROVISIONALLY FALSIFIED (premise, not numeric bar)
The numeric tripwire (AC@1<0.70 on the twin battery) is untested — no falsification there. But H3's load-bearing premises are contradicted: (a) the 0.46–0.54 "ceiling" is benchmark-specific (Eadro/Nezha/Groot clear 70%+ top-1 elsewhere); (b) the ≥10pp topology-prior ablation gap is contradicted by SimpleRCA parity and causal≈Dummy. H3 survives ONLY if re-scoped: claim "≥70% on OUR battery where RCAEval-hard fault mix holds" and pre-register the graph-free ablation as the riskiest check. If the ablation gap <10pp, H3's alternative (concede BARO parity) triggers.
