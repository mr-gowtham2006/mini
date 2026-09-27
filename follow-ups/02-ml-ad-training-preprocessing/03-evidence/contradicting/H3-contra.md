# H3-contra — Devil's advocate: filter-only or per-state models WIN over state-as-covariate

Date: 2026-09-13 · Scope: contradict H3 (`H3-state-as-covariate.md`).
Falsification target: fire tripwire-1 (`F1_raw(C) − F1_raw(F) < 0.02` on STARVED-heavy) or tripwire-2 (`F1_raw(C) − F1_raw(P) < 0.00`), or promote A1 (per-class − C ≥ +2pp).
Method: 5 falsification queries (all 2026-09-13), 0 fetches (full-text snippets). TEXT ONLY; no invented sources.

## Query log (5/5 — quota met before verdict)
| # | Query | Engine | Yield |
|---|---|---|---|
| Q1 | operating regime conditional AD per-regime model beats single global model manufacturing | academic | Product-aware 100% vs global 22.2% detection; Eusipco'12 interference effects; BAFO per-regime framing |
| Q2 | noisy regime labels covariate hurts AD label noise operating mode categorical harm | academic | OOD-under-label-noise >5% AUROC drop; mislabeled-fault memorization (PHM); NRdetector |
| Q3 | masking missing data downtime AD bias exclusion vs imputation harm | websearch | NeurIPS'24 ImAD imputation-bias; INTER +20% imputation-free; Qian et al. masking-strategy sensitivity |
| Q4 | Tennessee Eastman multimode per-mode PCA vs single model FDR FAR | academic | HSMM+PCA per-mode 100% / 98.30% avg; LCPCA multimode consensus |
| Q5 | contextual conditional AD operating mode false alarms steady-state filtering production | academic | ABB production zero-FP/FN contextual; SCDT context-conditioned envelopes; SCAL 95.60% avg (mixed, see S-hits) |

## Contra evidence (fires at H3)
- **C1 — Product-aware vs global process monitoring (2026, `https://arxiv.org/html/2606.00052`):** on unannounced mode transitions the **global model detected 22.2% (2/9)** vs **product-aware (per-mode) models 100%**. Direct tripwire-2-shaped precedent: per-regime ≫ single model where regimes are thermodynamically distinct (twin RUN vs STARVED −2σ offsets are exactly this shape). Weight: HIGH.
- **C2 — HSMM+PCA multimode TEP (`https://hal.science/hal-03875921v1/document`):** per-mode PCA with adaptive thresholds: **100% detection incl. hard fault 20 (vs 2–5% for single-model HMM/MBPCA baselines), 98.30% average.** LCPCA (`doi:10.1016/j.cjche.2020.10.030`) same consensus without prior mode labels. Direct tripwire-2 precedent on the canonical multimode benchmark. Weight: HIGH.
- **C3 — Interference effects (Eusipco 2012, `https://www.eurasip.org/Proceedings/Eusipco/Eusipco2012/Conference/papers/1569587469.pdf`):** capturing several regimes in one model causes *"interference effects… slow learning and poor generalizability."* Mechanism against arm C's single-model capacity claim. Weight: MEDIUM.
- **C4 — Label-noise fragility of the covariate leg:** OOD detection loses **>5% median AUROC** with noisy train labels, competitive-method count collapses above 20% noise (`https://doi.org/10.48550/arxiv.2404.01775`); deep PHM models memorize mislabeled faults and generalize poorly (`https://ar5iv.labs.arxiv.org/html/2009.14606`). Twin state labels are sim-scripted (clean) — but any sim→real regime-label mismatch replays this failure against arm C while arms F/P (which don't trust the label as a feature) are immune. Weight: MEDIUM (transfers as Sim2Real risk, not twin-battery fact).
- **C5 — Exclusion/masking is not free (NeurIPS'24 ImAD, Qian et al. 2024):** "impute-then-detect" leans incomplete-abnormal samples toward normal (lower recall); masking-strategy choice moves downstream ROC-AUC by up to ~0.05. Analog: H3's DOWN-masking + RUN-normal-only norm fitting is a structural choice with measurable downside risk (A3 leg — the mask, not the covariate, doing the work, or masking discarding transition context). Weight: MEDIUM-LOW (missing-data analog, not state-masking proper — closest available literature; disclosed as analog).

## Supporting-side hits found (disclosed, not suppressed — they favor arm C)
- S1: SCAL state-conditioned association learning (2026, `https://iopscience.iop.org/article/10.1088/1361-6501/aea242`): state-conditioning hits 95.60% avg F1, cuts Anomaly-Transformer miss rate 13.71% → 1.00%. Direct covariate-wins precedent.
- S2: ABB production contextual AD (`https://digitalcollection.zhaw.ch/items/41156474-1c4f-4e7f-80e7-041a27f3f3fe/full`): context-integrated classifier, zero FP/FN vs SOTA.
- S3: SCDT context-conditioned envelopes (`https://ar5iv.labs.arxiv.org/html/2604.24051`); "Out of Context" formalization `p(x|c)` (`https://ar5iv.labs.arxiv.org/html/2604.13252`).

## Verdict: PROVISIONALLY FALSIFIED (tripwire-2 leg; per-state precedent is the live killer)
C1+C2 are the strongest contra hits of the whole devil's run: on multimode benchmarks, per-regime models beat single models by far more than any 0–2pp wire, via exactly the mechanism (distinct regime thermodynamics) the twin's STARVED offsets instantiate. H3's covariate arm survives only if cross-state context + data-thin STARVED pools (Prediction 2) outweigh this — an untested twin-specific claim. S1–S3 keep the covariate leg alive (conditioning helps), but they support conditioning-in-general, not single-model-over-per-state. A1 escalation (per-class models) inherits C1/C2's momentum. Final call needs the STARVED-heavy M0b subset; until then the pipeline should treat per-state as co-default, not underdog.
