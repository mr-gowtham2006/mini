# Contradicting evidence — H4 (template + verifier grounding ≥95%, open ≤65%)

Date: 2026-09-11 · Agent: falsification-agent · Target: H4-template-verifier-grounding.md
Tripwire: falsified by template <95% (<48/50) OR open ≥80% on same alarm set.

## Queries (5, cascade academic→tech→web)
1. (academic) "LLM hallucination rate open-ended generation factuality benchmark without verification"
2. (academic) "template constrained generation grounding provenance LLM structured output faithfulness"
3. (tech) "LLM AIOps incident report generation hallucination RCA free-form narration accuracy"
4. (academic) "retrieval augmented generation faithfulness F1 0.9 RAG grounded narration factual consistency high"
5. (web) "structured output false confidence schema valid but factually wrong LLM constrained decoding quality"

## Relevance gate
Kept sources with measured hallucination/faithfulness rates or constrained-decoding quality analysis. Dropped vendor API docs (Outlines/AWS how-tos) without error numbers.

## Findings — squeeze from BOTH sides
Against "open ≤65%" (open narration CAN ground well):
- C-H4-1 (DIRECT counter): FIDES (arXiv 2606.05644) — context fidelity 92–94% on LLaMA3-70B, F1 62–63%, +14–28pts over standard RAG. Open generation with retrieval grounding far exceeds 65%.
- C-H4-2 (counter): Grounded Decoding (arXiv 2606.00432) — highest FActScore / evidence-support / citation-F1 among decoding-time RAG methods. Retrieval-anchored open narration is a solved-direction, not a 65% ceiling.
- C-H4-3 (counter, AIOps-specific): TAMO-FoA (IEEE CCWC 2026) — 76.3% root-cause identification accuracy in 3-month production deployment with tool-augmented (open) generation; 17.2% over baselines. Open agentic narration ≥65% in production.
Against "template ≥95%" (templates/verifiers do NOT guarantee truth):
- C-H4-4 (DIRECT counter): BoundaryML "Structured Outputs Create False Confidence" (2025-12) + TianPan "JSON Mode Won't Save You" (2026-04) + TDS "Your JSON Is Valid But Your Data Is Wrong" (2026-08/09): constrained decoding guarantees SHAPE, not truth — "forces the model to prioritize format compliance over semantic correctness"; "stripped of its ability to say 'I don't know'... forced to hallucinate to satisfy the grammar constraint." Template-shaped output with wrong IDs/values is the characteristic failure — exactly H4's "unlinked escapes" risk.
- C-H4-5 (mechanism, AIOps): "Why Do AI Agents Systematically Fail at Cloud RCA?" (arXiv 2602.09937) — "Hallucination in Interpretation observed in 71.2% of executions"; ORCA-bench (arXiv 2607.28545) — best RCA accuracy 25.3% medium / 10.0% realistic. Agentic RCA narration fails far below 80% even WITH tools; but note this also undermines any high open-narration baseline (supports H4's skepticism of open form, contradicts C-H4-3's optimism — benchmark vs production split, recorded).
- Supporting H4 (honesty): "Exploring LLM-based Agents for RCA" (arXiv 2403.04123) — 26% of CORRECT retrieval predictions contain hallucinations; ReAct grounding cuts it to <1–6%. Verifier-style grounding loops work — H4's mechanism is sound, its NUMBERS are the risk.

## Gap-pass
No retrieved study runs template-vs-open narration on the same n≥50 factory-alarm set with provenance-ID audit. Both tripwire numbers (95%, 80%/65%) are untested in-domain.

## Field-level citation check (R/P/H)
| ID | Reported number | Precise (alarm-set grounding?) | Hits tripwire? |
|---|---|---|---|
| C-H4-1 | fidelity 92–94% | No (QA benchmarks, not alarms) | No (but voids "open ≤65%" universality) |
| C-H4-2 | best FActScore/citation-F1 | No | No (same) |
| C-H4-3 | 76.3% production RCA acc | Partial (RCA,靠近 open≥65%) | No (<80% wire, but breaks ≤65% expectation) |
| C-H4-4 | qualitative: valid-but-wrong | Mechanism vs template≥95% | No (no 50-alarm rate) |
| C-H4-5 | 71.2% interp hallucination; ORCA 25.3%/10% | Partial | Mixed (hurts open-baseline hopes too) |

## Verdict: PROVISIONALLY FALSIFIED
Neither wire is tripped (no same-set 50-alarm comparison exists), but both halves of H4's joint claim are pressured: retrieval-grounded open narration demonstrably exceeds 65% (C-H4-1/2/3), and template/shape enforcement demonstrably fails to ensure truth (C-H4-4). H4's ≥95% template target with "zero unlinked escapes" is the riskier half — C-H4-4's forced-hallucination mechanism predicts exactly verifier-passing-but-wrong IDs. Recommendation: keep the audit design but relax the expected outcome to template ≥90% with escape taxonomy, and pre-register the open+RAG control (not pure open) as the fair comparator — pure-open is a strawman C-H4-1 already knocks down.
