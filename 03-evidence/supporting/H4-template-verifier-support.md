# H4 Supporting — Template+verifier grounding posture (open narration ungrounded; tool-grounding + provenance verifiers)

Date: 2026-09-11 · Hypothesis: H4-template-verifier-grounding.md

## Queries executed (relevance gate: each directly targets H4)
1. academic: "LLM hallucination ungrounded root cause analysis OpenRCA" — KEPT
2. academic: "knowledge retrieved RCA large language models microservices KRCA TAMO" — KEPT
3. web: "LLM root cause analysis provenance verifier template grounding hallucination" — KEPT
- Dropped/rewritten: none.

## Sources (cascade: search-cache → direct-URL → scholar → web)
| # | Source URL | Claim | Level | Confidence | Cascade verdict | Supports H4 how |
|---|---|---|---|---|---|---|
| S1 | https://doi.org/10.48550/arxiv.2606.27154 (OpenRCA 2.0) | Agents name ≥1 correct root-cause service in 76.0% but ground it in a verified causal path in only 61.5% ("ungrounded diagnosis"); exact-set recovery only 20.7% avg over 11 frontier LLMs | L2 (arXiv preprint) | Medium-High | cache=HIT; direct-URL=skipped/unverifiable; scholar=not-queried | Supports H4 prediction 2 (open narration ≤65%): independent ungrounded-diagnosis rate in the same band as H4's open-control expectation. |
| S2 | https://doi.org/10.48550/arxiv.2602.09937 ("Why Do AI Agents Systematically Fail at Cloud RCA?") | Full OpenRCA × 5 LLMs = 1,675 runs classified into 12 pitfall types (intra-agent / inter-agent / comms) | L2 (arXiv preprint) | Medium-High | cache=HIT; direct-URL=skipped/unverifiable | Supports H4 origin (C3 ungrounded-open-narration side): systematic, taxonomised failure of open agentic RCA at scale. |
| S3 | https://doi.org/10.1109/tsc.2025.3629066 / https://arxiv.org/html/2504.20462v5 (TAMO) | Tool-assisted LLM agent (multi-modality alignment + localisation + root-cause tools) addresses input/context/graph limits for fine-grained RCA | L1 (IEEE TSC 2025) | Medium-High | cache=HIT; direct-URL=skipped/unverifiable | Supports H4 alternatives-leg honesty + verifier necessity: grounding comes from tools/constraints, not free narration — consistent with template+verifier posture (and names the tool-agent alternative if H4's template leg fails). |
| S4 | https://arxiv.org/html/2606.18037v1 (ProvenanceGuard) | Source-aware verifier: atomic-claim decomposition → source-routed NLI + attribution check → per-claim verdicts over MCP traces | L2 (arXiv) | Medium | cache=HIT; direct-URL=skipped/unverifiable | Supports H4 verifier-gate mechanism: per-sentence/per-claim provenance verification is an existing pattern, precedent for H4's verifier + fallback design. |
| S5 | https://www.microsoft.com/en-us/research/blog/veritrail-detecting-hallucination-and-tracing-provenance-in-multi-step-ai-workflows/ (VeriTrail) | Closed-domain hallucination detection with traceability: evidence trails mapping outputs back to source sentences | L5 (vendor research blog) → paper behind it | Low-Medium | cache=HIT; direct-URL=skipped/unverifiable | Weak support for H4 provenance-ID leg (sentence-level evidence trails); low weight — vendor blog, not the paper. Needs full-paper fetch in a later pass. |

## Field-level citation check
| # | Title | Authors | Venue | Year | DOI/URL | Label | Match/Mismatch |
|---|---|---|---|---|---|---|---|
| S1 | OpenRCA 2.0: From Outcome Labels to Causal Process Supervision | (not extracted) | arXiv:2606.27154 | 2026 | doi/arXiv URLs above | P | Partial — authors unverified; flagged |
| S2 | Why Do AI Agents Systematically Fail at Cloud Root Cause Analysis? | (not extracted) | arXiv:2602.09937 | 2026 | doi/arXiv URLs above | P | Partial — authors unverified; flagged |
| S3 | TAMO: Fine-Grained Root Cause Analysis via Tool-Assisted LLM Agent… | (not extracted) | IEEE TSC (doi:10.1109/tsc.2025.3629066) | 2025 | doi + arXiv:2504.20462 | R | Partial — authors unverified; venue/year/DOI match |
| S4 | ProvenanceGuard: Source-Aware Factuality Verification for MCP-Based LLM Agents | (not extracted) | arXiv:2606.18037 | 2026 | arXiv URL above | P | Partial — authors unverified; flagged |
| S5 | VeriTrail: Closed-Domain Hallucination Detection with Traceability (blog) | Microsoft Research | MSR blog | 2025 | microsoft.com URL above | H (hosted blog) | Match (as blog pointer; paper itself unfetched) |

Hallucinated (H) sources removed: none — all URLs verbatim from search; no invented citations.
