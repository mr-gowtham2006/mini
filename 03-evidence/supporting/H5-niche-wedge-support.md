# H5 Supporting — Semester-niche wedge (0/14 joint coverage; upkeep-free slot empty)

Date: 2026-09-11 · Hypothesis: H5-semester-niche-wedge.md

## Queries executed (relevance gate: each directly targets H5)
1. tech: "PyRCA Salesforce root cause analysis causal graph" — KEPT
2. tech: "Datadog Dynatrace AIOps root cause provenance deterministic replay" — KEPT
3. web: "Siemens Insights Hub digital twin factory alarms student upkeep" — KEPT
4. gap-pass tech: "Merlion FactorySimPy time series anomaly simulation replay manufacturing" — KEPT
- Dropped/rewritten: none.

## Sources (cascade: search-cache → direct-URL → scholar → web)
| # | Source URL | Claim | Level | Confidence | Cascade verdict | Supports H5 how |
|---|---|---|---|---|---|---|
| S1 | https://github.com/salesforce/pyrca + https://opensource.salesforce.com/PyRCA/latest/ | PyRCA = metric-based RCA (ε-diagnosis; Bayesian/random-walk over topology-causal graph; PC/GES/FGES/LiNGAM discovery). No per-sentence provenance narration, no seeded-replay harness in scope | L3 (OSS repo + docs) | High | cache=HIT; direct-URL=skipped/unverifiable | Supports H5 prediction 1: closest OSS miss ≤2/3 (ranked trace ✓, provenance ✗, seeded replay ✗) — PyRCA row fails full-row hit. |
| S2 | https://www.datadoghq.com/blog/datadog-watchdog-automated-root-cause-analysis/ + https://www.datadoghq.com/blog/building-bits-ai-sre/ | Watchdog RCA maps infra + flags probable cause ("story" feed); Bits agentic SRE investigates via targeted queries. Cloud-scale SaaS, no per-sentence provenance IDs, no seeded factory-twin replay, no CPU-local semester scope | L5 (vendor blogs/docs) | Medium | cache=HIT; direct-URL=skipped/unverifiable | Supports H5 prediction 1: AIOps row misses provenance + replay + CPU-local (≤1/3 on Verdandi's columns). |
| S3 | https://www.dynatrace.com/news/blog/build-trust-with-dynatrace-ai-driven-root-cause-and-impact-analysis/ + https://www.dynatrace.com/platform/root-cause-analysis/ | Davis deterministic Visual Resolution Path over Smartscape topology ("instant replay" = incident timeline visualisation, not seeded sim replay); enterprise SaaS topology | L5 (vendor pages) | Medium | cache=HIT; direct-URL=skipped/unverifiable | Supports H5 prediction 1 with a disclosed near-miss: Davis has deterministic path + timeline "replay" but NOT seeded factory-twin replay + per-sentence provenance + CPU-local — row still fails full hit. Terminology guard recorded (replay ≠ seeded replay). |
| S4 | https://assets.ctfassets.net/…/InsightsHub_for_Academia_ProductSheet_v1.2.pdf + https://resources.sw.siemens.com/en-US/case-study-swinburne-university/ | Insights Hub for Academia = teaching/IoT/digital-twin kit (predictive learning, edge analytics, twins); Swinburne Factory-of-Future deployment. Cloud platform with upkeep/licensing weight — not semester-local upkeep-free CPU bundle | L5 (vendor product sheet + case study) | Medium | cache=HIT; direct-URL=skipped/unverifiable | Supports H5 prediction 1: Siemens row misses semester-local upkeep-free scope (and per-sentence provenance + seeded replay as Verdandi defines them). |
| S5 | https://github.com/FactorySimPy/FactorySimPy + https://github.com/salesforce/Merlion | FactorySimPy = SimPy manufacturing simulator (machines/conveyors graph, fast-as-possible + real-time modes) — sim without RCA/provenance. Merlion = TS intelligence lib with live-deployment-simulation evaluators — eval harness without factory RCA/provenance | L3 (OSS repos) | Medium-High | cache=HIT (gap-pass); direct-URL=skipped/unverifiable | Supports H5 prediction 1: sim-side misses ≤2/3 from the other direction (replay/sim ✓-adjacent, ranked-trace RCA ✗, provenance ✗). No single repo bundles all three. |

## Field-level citation check
| # | Title | Authors | Venue | Year | DOI/URL | Label | Match/Mismatch |
|---|---|---|---|---|---|---|---|
| S1 | PyRCA (+ docs) | Salesforce OSS | GitHub / opensource.salesforce.com | 2023– | github.com/salesforce/pyrca | H (hosted repo/docs) | Match |
| S2 | Watchdog RCA / Bits AI SRE blogs | Datadog | datadoghq.com blog | 2021 / 2026 | datadoghq.com URLs above | H (vendor blog) | Match |
| S3 | Dynatrace AI-driven RCA / RCA platform page | Dynatrace | dynatrace.com | 2026 | dynatrace.com URLs above | H (vendor page) | Match + terminology guard (timeline-replay ≠ seeded-replay) |
| S4 | Insights Hub for Academia product sheet / Swinburne case study | Siemens | siemens.com assets | 2023– | URLs above | H (vendor collateral) | Match |
| S5 | FactorySimPy / Merlion | OSS contributors (FactorySimPy; Salesforce) | GitHub | 2024– | github URLs above | H (hosted repo) | Match |

Hallucinated (H) sources removed: none — all URLs verbatim from search; no invented citations.
Scope guard (M3/Gap 5): H5 vendor/OSS rows are scoped to 2026-09 retrieval; no brownfield-transfer claim made; per-alarm (not fleet-MTTR) phrasing kept.
