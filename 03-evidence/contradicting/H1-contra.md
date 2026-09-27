# Contradicting evidence — H1 (topology-masked PCMCI + flip-gate)

Date: 2026-09-11 · Agent: falsification-agent · Target: H1-topology-pcmci-flip-gate.md
Tripwire: falsified by masked flip >14.4% OR blind flip ≤19.4% on same battery.

## Queries (5, cascade academic→tech→web)
1. (academic) "PCMCI causal discovery false positive rate constraint-based time series benchmark"
2. (academic) "prior knowledge constraints causal discovery no improvement blind search stability"
3. (tech) "PCMCI tigramite causal graph instability seed sensitivity spurious links"
4. (web) "PCMCI false positive rate high autocorrelation spurious edges benchmark 20% 30%"
5. (academic) "Bagged PCMCI bootstrap aggregation reduces false positives stability causal discovery"

## Relevance gate
Kept only sources reporting measured FPR/flip/stability numbers or prior-constraint effects on constraint-based discovery. Dropped pure API docs (tigramite readthedocs, pcmci.py source) and unrelated score-based (A*/GES) papers.

## Findings (strongest contradicting first)
- C-H1-1 (prior-fragility, DIRECT counter): "Guide, Not Bind: Why Defeasible Priors Fail in Augmented Lagrangian Causal Discovery" (arXiv 2609.03442, 2026-09-03) — shows soft topology priors can be suppressed before counterfactual checks, i.e. a plausible-but-wrong mask can HURT vs blind. If H1's 32-node mask contains imperfect edges, masked flip can exceed blind.
- C-H1-2 (prior-fragility): "Robust Causal Discovery under Imperfect Structural Constraints" (arXiv 2511.06790) — performance "degrades sharply" under imperfect priors (overlooked true edges / spurious introduced). Directly attacks H1's assumption of a correct mask.
- C-H1-3 (base-instability, partial counter): Debeire et al., "Bootstrap aggregation and confidence measures" (PMLR 236 / arXiv 2306.08946) — Bagged-PCMCI+ "greatly reduces the number of false positives compared to the base PCMCI+ algorithm" and improves stability via majority voting. Implies base PCMCI+ IS seed/bootstrap-unstable, so H1's ≤14.4% flip without ensembling is a non-trivial bet; averaging/bagging, not the mask, may deserve the credit (matches H1's own alternative).
- C-H1-4 (weak counter): Sandia benchmarking report (OSTI 1991387) — PCMCI "consistently exhibits near perfect false positive rates in all scenarios" for T>50. Cuts AGAINST falsification (supports H1); recorded for honesty.
- C-H1-5: Hasan et al. KCRL (PMLR 182) / KGS — prior-constrained search helps ONLY when prior knowledge is "completely true" (their stated limitation). Same fragility point as C-H1-1/2.

## Gap-pass
No retrieved study runs masked-vs-blind PCMCI+ flip on a 32-node twin battery with ≥5 seeds. The numeric tripwire (14.4% / 19.4%) is untested anywhere. Gap 1 stands.

## Field-level citation check (R/P/H: Reported / Precise / Hit-tripwire)
| ID | Reported number | Precise (same metric?) | Hits tripwire? |
|---|---|---|---|
| C-H1-1 | qualitative failure mechanism, no flip % | No (ALM, not PCMCI+) | No |
| C-H1-2 | "degrades sharply", no flip % | No | No |
| C-H1-3 | FPR reduction, bagged vs base | Partial (FPR, not flip-gate) | No |
| C-H1-4 | near-perfect FPR, T>50 | Partial | No (supports H1) |
| C-H1-5 | convergence gain, true-prior only | No | No |

## Verdict: NOT FALSIFIED
No source reports masked flip >14.4% or blind ≤19.4% on a comparable battery. Prior-fragility literature (C-H1-1/2/5) weakens H1's mechanism confidence and promotes its stated alternative (credit averaging/bagging, demote mask) but does not trip the wire. Recommendation: H1 survives; add mask-perturbation ablation (corrupt 10% of mask edges) to the verification battery.
