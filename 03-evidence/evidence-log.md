# Evidence log — verdandi-anomaly-twin-trace

## 2026-09-11 · falsification-agent (contradicting sweep, 25 queries)
Scope: H1–H5 tripwires in 02-hypotheses/hypothesis-registry.md. Cascade academic→tech→web; relevance gate (numbers-or-mechanism only); gap-pass; field-level R/P/H check per note.

| H | Queries | Strongest contra | Verdict |
|---|---|---|---|
| H1 | 5 | Defeasible-prior failure (arXiv 2609.03442); imperfect-prior sharp degradation (arXiv 2511.068xx/2511.06790); Bagged-PCMCI+ base instability (PMLR 236) | Not falsified — tripwire untested; mechanism weakened, add mask-perturbation ablation |
| H2 | 5 | GDN F1 0.81 SWaT raw point-wise (AAAI'21) trips GNN≥0.60 wire; PA-critique (Kim et al.) defuses MTAD-GAT/GTAD numbers only | Provisionally falsified — scope to M0b battery, pre-register PA-off |
| H3 | 5 | Eadro HR@1 0.982 / SimpleRCA 0.93 w/o prior / causal≈Dummy (ASE'24) void ceiling + ablation-gap premises; RCAEval 0.46–0.54 supports ceiling on hard mix | Provisionally falsified (premise) — numeric bar untested; battery mix decides |
| H4 | 5 | FIDES 92–94% fidelity / grounded-RAG (open CAN exceed 65%); false-confidence literature (shape≠truth) pressures template≥95% | Provisionally falsified — relax to ≥90% + escape taxonomy; comparator should be open+RAG |
| H5 | 5 | Nearest misses: Eadro (T), RCAEval (T+R), Groot (T), Cloud-OpsBench (T+R); none covers T+P+R jointly; CPU-local is table stakes | Not falsified — survives conditionally on H4's provenance leg |

Notes: contradicting/H1-contra.md … H5-contra.md. No numeric tripwire tripped on a same-battery comparison for any H (all gap-passes negative). Cross-dependency flagged: H5 rests on H4's provenance leg.
