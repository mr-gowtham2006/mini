# Evidence log — follow-up 02 (03-evidence)

## Run 2026-09-13 — SUPPORTING H1–H5 (twin ML training; parent F1 Locked raw-only)

Scope: supporting/ only. No status changes. No contradicting-search. Fetch cap 25 → used 11 (8 success, 1 timeout, 1 binary-PDF-unverified, 1 blocked-PoW noted). TEXT ONLY honored.

### Relevance gate (query → keep/drop/rewrite)
- "supervised anomaly detection synthetic unknown generalization Han Liu" → HIT (Lau full-text, OSAD abstracts, DevNet). Han-1% kept as cited-via-Lau snippet-only (primary unfetched — flagged, not sole support).
- "GDN raw F1 SWaT WADI vs KNN" → HIT (GDN full-text Table 2). PA-numbered rows encountered → flagged inadmissible, mechanics-only.
- "M2AD status covariates Gamma calibration" → HIT (PMLR abstract+bib; full PDF unopened per TEXT-ONLY — mechanics via source-table + abstract).
- "per-sensor normalization median IQR z-score vs min-max" → HIT (RobustScaler docs, TSFM study full-text). Sciencedirect comparison kept as abstract snippet-only.
- "POT SPOT EVT streaming" → HIT (KDD17 landing+abstract; HAL PDF blocked by Anubis PoW — prior A09 extraction stands, logged miss).
- "STAD supervised labels empirical" → HIT (STAND full-text; STAD-FEBTE dropped — industrial case-study abstract, tabular-diagnosis scope, low marginal value vs STAND).
- "regime conditioned operating state covariate steady-state" → HIT (TEP product-aware full-text, SCAL abstract, DCD-VAE abstract, PHM circuit-breaker strata). Thibault primary not separately fetched — S-H3-5 via A19 + PHM L1+ variant; logged partial.
- "fixed percentile threshold production alert budget" → PARTIAL (Multigrid fetch timeout — prior J26 snippet stands; corroborated via TheCodeForge/Arun Baby/MAY2704 snippets + GDN max-validation + TEP P95).
- "DevNet few labeled anomalies" → HIT (abstract+repo). "TSFM normalization" → HIT (full-text). FluxEV PDF → binary, UNVERIFIED + snippet fallback.

### Per-source verdicts
| Source | Verdict | File |
|---|---|---|
| arxiv.org/html/2511.16145v1 (STAND) | HIT full-text | H1-support (S-H1-1), H2-support (S-H2-4) |
| arxiv.org/html/2506.13955v1 (Lau) | HIT full-text | H1-support (S-H1-2) |
| Han et al. 2022 via Lau §2 | SNIPPET-ONLY (primary unfetched) | H1-support (S-H1-3) |
| arxiv 1911.08623 DevNet + repo | HIT abstract+repo (full text unopened) | H1-support (S-H1-4) |
| arxiv 2203.14506 / 2310.12790 OSAD | SNIPPET-ONLY | H1-support (S-H1-5) |
| ar5iv 2106.06947 (GDN) | HIT full-text | H2-support (S-H2-1/2), H4-support (S-H4-1), H5-support (S-H5-3) |
| A03/A05/J06 PA rows | HIT — numbers INADMISSIBLE, mechanics only | H2-support (S-H2-3), H5-support (S-H5-2) |
| PMLR v258 M2AD | HIT abstract+bib (PDF unopened) | H3-support (S-H3-1), H5-support (S-H5-2) |
| arxiv.org/html/2606.00052 (TEP product-aware) | HIT full-text | H3-support (S-H3-2), H5-support (S-H5-3) |
| IOP SCAL 10.1088/1361-6501/aea242 | SNIPPET-ONLY | H3-support (S-H3-3) |
| DCD-VAE proceedings PDF | SNIPPET-ONLY | H3-support (S-H3-4) |
| MDPI A19 + PHM-EU 4896 strata | SNIPPET-ONLY | H3-support (S-H3-5) |
| sklearn RobustScaler docs | HIT | H4-support (S-H4-2) |
| ar5iv 2512.02833 (TSFM norm) | HIT full-text | H4-support (S-H4-3) |
| sciencedirect S2214579623000400 + Lacuna | SNIPPET-ONLY | H4-support (S-H4-4) |
| KDD17 SPOT landing + A09 prior | HIT abstract + prior extraction; HAL re-fetch MISS (Anubis PoW) | H5-support (S-H5-1) |
| J13 + TheCodeForge/Arun Baby/MAY2704 | HIT pattern-level (L4) | H5-support (S-H5-4) |
| Multigrid alert-budget guide | MISS (timeout; J26 snippet stands) | H5-support (S-H5-4) |
| WSDM21 FluxEV PDF | UNVERIFIED (binary) + snippet fallback | H5-support (S-H5-5) |

### Gap-pass per sub-hypothesis (with follow-up queries)
- H1 known margin ≥+3pp: SUPPORTED (direction; 3 witnesses) → q: "STAND BCE supervised vs reconstruction unsupervised raw-F1 full table".
- H1 unknown recall ≥0.30: PARTIAL (non-temporal theory + snippet OSAD) → q: "open-set supervised time series unseen fault family recall" ; "semi-supervised TSAD known+synthetic unknown-slice ablation".
- H1 dose optimum: PARTIAL (n′=n+n⁻ rule, non-temporal) → q: "synthetic anomaly dose-response sensor time series dilution ablation".
- H2 wire clearability: SUPPORTED bench precedent (SWaT/WADI raw gaps) → q: "KNN PCA vs GDN raw F1 rectangular synthetic fault ablation".
- H2 graph-ablation ≥10pp leg: SUPPORTED bench precedent → q: "GDN learned vs complete graph ablation F1".
- H2 classical cost leg: GAP → q: "GDN vs KNN PCA training inference time CPU benchmark".
- H3 covariate-vs-filter ≥+2pp: SUPPORTED (direction + blind-spot stress) → q: "starved/blocked state covariate vs filter-only anomaly F1".
- H3 single-vs-per-state ordering: GAP/CONTESTED (S-H3-2 favors P-family) → q: "single mode-covariate vs per-mode ensemble anomaly F1 comparison".
- H3 DOWN-masking leg: PARTIAL → q: "steady-state gated training downtime masking ablation".
- H4 estimator leg (robust vs minmax): SUPPORTED → q: "RobustScaler vs MinMaxScaler anomaly detection F1".
- H4 scope leg (Gap 1 core): PARTIAL (zero direct scope papers) → q: "per-sensor per-machine vs global normalization multivariate AD ablation".
- H5 POT fit (Gap 4): PARTIAL → q: "SPOT DSPOT short series few-peaks threshold stability".
- H5 GG worst-machine edge: PARTIAL (mechanics strong) → q: "Gamma Fisher p-value aggregation dependent sensors false alarm".
- H5 percentile incumbency + budget practice: SUPPORTED → q: "alert budget threshold calibration production shadow re-measure".

### Outputs
supporting/H1-support.md, H2-support.md, H3-support.md, H4-support.md, H5-support.md — one per hypothesis, none skipped.

## Run D1 — Devil's-advocate (contradicting) sweep, 2026-09-13
Role: contradict H1–H5 with supporting-search thoroughness. 25 falsification queries (5/hypothesis), 3 verification fetches (DRA abs, TimeEval evaluation-paper page, M²AD PMLR page; TranAD paper via full-text snippet), 0 PDFs ingested (abstract/snippet + verified-page fallback throughout). Fetch cap 25 → this run used 3. No status changes; registry untouched. Files: `contradicting/H1-contra.md … H5-contra.md`.

| Hyp. | Queries | Strength | One-line justification |
|---|---|---|---|
| H1 (supervised + unknown-family) | 5 | **Provisionally falsified** (tripwire-2 leg) | DRA/DevNet open-set collapse + Schmidl Finding 2 (VERIFIED) + RedLamp diversity-gap/false-anomaly threaten `recall_unknown ≥ 0.30`; known-family margin leg survives. |
| H2 (classical gate) | 5 | **Not falsified after 5** | All deep-clears-wire hits (TranAD +17%, GDN +54%, USAD +0.096) are PA-on; PA-proof evidence (Schmidl AUC, Wu&Keogh flaw taxonomy) cuts toward gate-holds. |
| H3 (state-as-covariate) | 5 | **Provisionally falsified** (tripwire-2 leg) | Per-regime 100%-vs-22.2% (product-aware) + HSMM+PCA 98–100% per-mode precedents directly threaten C > P; covariate leg kept alive by SCAL/S2–S3. |
| H4 (per-machine norm) | 5 | **Not falsified after 5** (closest call) | No direct M-vs-G ablation exists (open gap confirmed); live G-threats logged: TranAD global-min-max SOTA precedent (VERIFIED Eq.1), scale-erasure mechanism, thin-pool (~200/episode) estimation noise. |
| H5 (thresholding arms distinguishable) | 5 | **Not falsified after 5** | No shared-calibration 3-arm comparison found; live devices logged: POT needs n∼1000 vs ~300/episode pools (→ POT≈Pct collapse risk), GG payoff doubt on univariate-dominated faults; M²AD +21% disclosed pro-GG. |

Cross-cutting notes: (a) PA-on vs PA-off protocol mismatch voided most contra magnitude claims — the F1 lock is doing load-bearing work across all five files. (b) Supporting-side hits disclosed in-file (H1:S1–S3, H2:S1–S4, H3:S1–S3, H4:S1–S3, H5:S1–S4); nothing suppressed. (c) Awaiting M0b battery for numeric tripwire firing on all five; literature alone fired no wire. Devil's-run fetch total: 3/25 cap.
