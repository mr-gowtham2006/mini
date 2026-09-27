# Iteration 001 — Verdandi full literature + market research

## Research Question
Full literature survey + market/problem/competitor/factor analysis for Verdandi (32-machine factory twin, ranked causal trace + why-explanation + seeded replay, CPU <10min). Use searxng properly.

## Date
2026-09-11

## Actions Taken
- Phase 1: literature review team (academic + industry + merger)
  - 23 verbatim queries (12 academic incl 4 falsification-seeking; 11 industry incl 2 falsification-seeking), all searxng
  - 47 sources (26 academic L1-L3 + 22 industry L1-L5, 1 dedup), PRISMA flow 180→177→89→47
  - 6 contradictions mapped; merger stalled on 2 files → lead fallback built source-table.md + contradictions-map.md
- Phase 2: hypothesis formation (single deep delegate, no retrieval)
  - 5 falsifiable hypotheses H1-H5, each with numeric tripwire + verification method
- Phase 3: adversarial evidence team (confirmation + falsification, equal resources)
  - Supporting: 5 notes, 17 queries; Falsification: 5 notes, 25 queries (5/H)
  - H1 not falsified; H2/H3/H4 provisionally falsified; H5 not falsified (conditional on H4)
  - Claim-lock annex: 4 locked / 5 unresolved (missing legs named) / 3 refuted universals
- Phase 4: synthesis (single deep delegate, no retrieval)
  - 6 findings (3 established High 85-90%, 1 conditional Moderate 70-78%, contested pair + conditional wedge), conclusions, 7 open questions, 5 decision recommendations

## Key Decisions
| Decision | Rationale |
|---|---|
| Lead fallback for 2 merge files | Merger stalled; mechanical dedup mappable from angle tables |
| Universal claims refuted, scoped forms kept | GDN 0.81 SWaT / FIDES 92-94% / Eadro 0.982 each break universality without touching twin-scoped bars |
| No numeric project numbers presented as established | F1~0.73/AC@1/flip/1.00 all lack same-battery lock; M0b + audits owed |

## Results
- Hypotheses falsified: 0 fully (3 provisionally on premise/scope)
- Hypotheses corroborated: 1 conditional (H1)
- Contested: 4 (H2/H3/H4 + H5-conditional)
- Overall confidence: Established 85-90% (protocol/platform/wedge-conjunction) / Contested (all numerics)

## What Was Learned
The wedge (ranked-trace + provenance + replay, CPU-local) survives; every number needs a pre-registered battery. Strongest risks: mask-correctness (H1), SimpleRCA parity on ablation (H3), forced-hallucination through templates (H4, which H5 inherits).

## Next Steps / Open Paths
M0b pre-registered battery; 10%-mask-corruption ablation; graph-free ablation; n≥50 RAG-control grounding audit; brownfield transfer study; 9/14 competitor-row re-audit + fetch/author-verification pass.
