# Claim-lock annex — follow-up 02, Phase 3 Step 3.5

Date: 2026-09-13 · Gate: supporting/ (5 files, 11 fetches) + contradicting/ devil's sweep (5 files, 25 queries, 3 verification fetches) · TEXT ONLY honored (0 PDFs ingested; HAL SPOT re-fetch blocked by Anubis PoW, prior A09 extraction stands; FluxEV binary UNVERIFIED)
Parent locks honored: F1 (raw point-wise F1 only, PA-off, fixed-percentile — no PA number anywhere below); F2/F3 (battery-scoped verdicts, no ROI/wedge prose). Sibling L1–L4 compatible (L1 residual gain, L2 envelope-SNR, L3 drift collapse, L4 rate-budget cited as practice only, never as transfer numbers).

HIGH-RISK RULE (strictly enforced): a claim locks ONLY on (a) ≥2 independent domains, (b) a completed counter-search that failed to refute it, (c) ≥1 primary source fetched at field-verifiable depth. Preprint-alone, snippet-alone, or any twin-transfer numeric margin NEVER locks. All locks are SCOPE-LIMITED (mechanism / method-fact / design-constraint direction only) — no twin-transfer numbers appear anywhere.

SELF-ENFORCING: synthesis (04-synthesis/) may cite ONLY claims under ## Locked below, plus the parent F1–F3 and sibling L1–L4 locks. Anything under ## Unresolved may appear in synthesis SOLELY as an open question with its named missing leg. ## Refuted claims may not appear except as rejected alternatives with criterion + source.

## Locked (6, all scope-limited)

### LOCK-1 — Deep-vs-classical raw-F1 gap is measurable under identical calibration (method precedent, not a transfer number)
- Claim (scoped): under shared splits/features/windows and fixed validation-max calibration, a deep-vs-classical raw point-wise F1 delta is a well-defined, published quantity — the H2 wire (≥+3pp) is therefore a load-bearing instrument, not formalism.
- Domains: (i) water-plant benches — GDN raw table SWaT 0.81 vs PCA 0.23/KNN 0.08, WADI 0.57 vs 0.10/0.08 (L1 primary, full-text R); (ii) 5-dataset supervised-vs-unsupervised harness — STAND TSB-AD common harness running IForest/LOF/PCA/HBOS/KNN/KMeans vs deep lines under one protocol (L3 full-text R).
- Counter-search: H2-contra Q1–Q5 attempted deep-clears-wire transfer; every hit voided as PA-on or supervised-diagnosis (protocol mismatch), and PA-proof hits (Schmidl AUC, Wu&Keogh taxonomy) cut toward gate-holds — refutation failed.
- Primary source: Deng & Hooi, AAAI 2021 (ar5iv full HTML fetched, Eq.12 + Table 2 + §4.3 ablation).
- Does NOT lock: any twin magnitude, any "GDN transfers" statement (that is H2's H0, explicitly rejected as assumption).

### LOCK-2 — Per-sensor median/IQR robust error norm prevents high-variance-sensor dominance (estimator fact)
- Claim (scoped): normalizing per-sensor errors with median/IQR rather than mean/std is outlier-robust and prevents high-variance channels drowning stable ones. Estimator choice only — says NOTHING about per-machine vs global scope (that is unresolved U-4).
- Domains: (i) TSAD systems — GDN §3.6 Eq.12 robust norm inside the SWaT/WADI raw-winning system (L1 R); (ii) general ML tooling — sklearn RobustScaler official semantics "median and IQR often give better results" + fit/transform separation as leakage-discipline primitive (official docs R); (iii) TSFM study — mean/std-family normalizers beat min-max by 25–45%, Lag-Llama uses median/IQR for outlier robustness (L3 full-text R).
- Counter-search: H4-contra C2/C3 attacked SCOPE (scale-erasure, thin-pool noise), never the estimator — estimator leg unthreatened across 5 queries.
- Primary source: Deng & Hooi, AAAI 2021, §3.6 Eq.12 (fetched).

### LOCK-3 — Fixed-percentile / max-validation thresholding is the production-grade incumbent (incumbency, not optimality)
- Claim (scoped): validation-fit fixed-percentile (incl. max-validation and per-regime P95) is the published operating rule in SOTA systems AND the documented production practice (threshold-to-budget-then-recall). Incumbency only — does NOT lock Pct as optimal (that is unresolved U-5).
- Domains: (i) SOTA systems — GDN max-validation threshold (L1 R) + TEP product-aware per-mode P95 Eq.7 (L3 full-text R); (ii) production practice — Technolynx handful/shift + TheCodeForge 99.5th-pctile streaming + Arun Baby replay/shadow + MAY2704 alert-budget flags (L4 pattern-level P, no benchmarks claimed).
- Counter-search: H5-contra attacked POT/GG arms and calibration-data dominance, never Pct incumbency — no refutation found in 5 queries.
- Primary source: Deng & Hooi, AAAI 2021, §3.6+§4.3 (fetched).

### LOCK-4 — SPOT/POT design constraint: ~n=1000 calibration pile + stationarity; DSPOT covers abrupt drift only (design-spec constraint)
- Claim (scoped): per its own design spec, SPOT initialization fits GPD on an n∼1000 calibration batch and assumes stationary data; DSPOT extends to abrupt drift, not knee/wear drift. Twin pools (~300 normals/episode) are sub-SPOT-scale by arithmetic against twin facts (CONTEXT.md), not by transfer.
- Domains: (i) EVT-streaming — Siffer et al., KDD 2017 landing+abstract + prior A09 p1–4 extraction (L1 P, HAL re-fetch PoW-blocked, logged miss); (ii) drift-limits corroboration — libspot drifting guide "works only on stationary data" + SCS/MACS 2025 "EVT-POT assume quasi-stationary threshold regime" (contra C2, P).
- Counter-search: H5-contra S4 (DCDSPOT multi-tier POT beats single-threshold) narrows the constraint to rich-calibration regimes — consistent with, not refuting, the spec numbers.
- Primary source: Siffer et al., KDD 2017 (landing verified; full text via prior extraction, NOT this run).
- Does NOT lock: any POT-vs-Pct F1 outcome on twin data (that is unresolved U-5).

### LOCK-5 — Regime/mode conditioning beats mode-agnostic scoring on multimode benchmarks (direction only, no magnitude)
- Claim (scoped): ignoring operating mode creates blind-spot failures; conditioning on regime (as covariate, per-regime model, or conditioned thresholds) recovers detection. DIRECTION ONLY — says nothing about single-covariate vs per-state ordering (that is contested U-3) and carries no transfer number.
- Domains: (i) chemical process — TEP product-aware bank F1 0.5322 vs global 0.4643 + 100% vs 22.2% blind-spot stress (L3 full-text R); (ii) cloud assets — M2AD STATUS covariates + GMM+Gamma dependence-correct calibration, 130-asset case study, χ²-miscalibration Prop. 2 (L1 P, abstract+bib, PDF unopened); (iii) multimode TEP control — HSMM+PCA per-mode 100%/98.3% vs 2–5% single-model baselines (contra C2, P).
- Counter-search: H3-contra C1+C2 threaten the C-vs-P ORDERING (tripwire-2), never the conditioning principle — S1–S3 (SCAL 95.6%, ABB zero-FP/FN, SCDT envelopes) independently corroborate conditioning.
- Primary source: Islam & Carden, TEP product-aware, May 2026 (full HTML fetched).

### LOCK-6 — PA-on headline numbers are inadmissible as raw-F1 evidence (protocol verdict, restates parent F1)
- Claim (scoped): gains measured under point-adjustment / overlap-TP / test-calibrated-threshold protocols (TranAD +17.06%, GDN-WADI +54%, USAD +0.096, M2AD +21%, tutorial F1s) transfer MECHANICS ONLY; their numbers are void against the F1 lock.
- Domains: (i) protocol forensics — TranAD's own PA-on F1 vs PA-off F1* dual reporting + AAAI'22 rigorous-eval PA pitfall + Threshold-Paradox 2026 test-calibration bias (P–R); (ii) benchmark-validity — Wu & Keogh flaw taxonomy (triviality/density/mislabeling/run-to-failure) + Schmidl threshold-agnostic AUC General Finding 2, VERIFIED evaluation page (R).
- Counter-search: the entire H2-contra run WAS the counter-search (5 queries hunting a PA-off deep-clears-wire precedent); it returned zero admissible hits — refutation failed by exhaustion.
- Primary source: Schmidl et al., VLDB 2022 evaluation page (fetched) + TranAD VLDB 2022 dual-metric table (verified in-session).

## Unresolved (claim + named missing leg; synthesis may cite ONLY as open questions)

- U-1 (H1 unknown-family recall ≥0.30): direction has mechanism (Lau known+synthetic minimax gains, OSAD recipe) but Lau is NON-TEMPORAL and OSAD is SNIPPET-ONLY (H, removed). MISSING LEG: temporal open-set precedent or M0b hedge-split read-off. Follow-up: "open-set supervised time series unseen fault family recall".
- U-2 (H2 twin wire outcome): bench precedent shows the wire is clearable elsewhere (+58pp SWaT), twin caricature-rectangular mix predicts compression/reversal. MISSING LEG: M0b battery read-off. No literature can close this.
- U-3 (H3 single-covariate vs per-state ordering): covariate-vs-filter direction locked (LOCK-5); C-vs-P ordering CONTESTED by C1+C2 per-regime precedents. MISSING LEG: single-model-with-covariate vs per-state head-to-head on STARVED-heavy data (no retrieved study runs it).
- U-4 (H4 per-machine vs global SCOPE): estimator leg locked (LOCK-2); scope leg has ZERO direct papers anywhere found (open gap confirmed by both supporting gap-pass and H4-contra C5). MISSING LEG: any per-machine-robust vs global-min-max ablation, twin or bench. G-side threats live (TranAD global-min-max SOTA precedent VERIFIED Eq.1; scale-erasure mechanism; ~200-step thin-pool noise).
- U-5 (H5 three-arm distinguishability): arm designs + incumbency locked (LOCK-3/LOCK-4); no study runs Pct-vs-POT-vs-GG on shared validation-only calibration. MISSING LEG: M0b calibration firing test. Live devices: POT≈Pct collapse on F1 leg (C1) + GG A3 tax-without-gain branch (C4).
- U-6 (secondary): synthetic dose interior optimum on sensor manifolds (MISSING: dose-response ablation on time-series sensor data); classical CPU-cost ≥10× advantage (MISSING: GDN-vs-KNN/PCA CPU benchmark); DOWN-masking isolated contribution (MISSING: steady-state-gated masking ablation beyond analogs); GG worst-machine edge numbers (MISSING: numbers inadmissible, mechanics only).

## Refuted

NONE. No source met any H-file numeric tripwire criterion on twin data — literature alone fired no wire (devil's-run §71c; evidence-log D1). Provisional leg-level threats (H1 tripwire-2 via DRA/bias-effect/RedLamp; H3 tripwire-2 via product-aware/HSMM per-regime precedents) are recorded as CONTESTED legs under U-1/U-3, NOT refutations: each rests on adjacent-domain or protocol-adjacent precedents and awaits the M0b battery for the numeric kill. A claim enters this section only when an M0b tripwire fires or a twin-scoped primary source meets the criterion verbatim.

## Field-level citation check (per source: title / authors / venue / year / DOI-or-URL — full match → R; partial → P re-verify before synthesis reliance; no match → H remove with note)

| # | Source (as cited) | Fields verified | Label | Disposition |
|---|---|---|---|---|
| 1 | Deng & Hooi, GDN, AAAI 2021 — ar5iv 2106.06947 | title/authors/venue/year/URL all match fetched full text | R | Retain; primary for LOCK-1/2/3 |
| 2 | Zhong et al., STAND "Labels Matter More Than Models", preprint Nov 2025 — arxiv 2511.16145v1 | title/authors/year/URL match fetched HTML; venue = preprint (as cited, L3) | R | Retain at L3; harness fact only |
| 3 | Lau et al., semi-supervised AD theory, preprint Jun 2025 — arxiv 2506.13955v1 | title/authors/year/URL match fetched HTML; venue = preprint (as cited) | R | Retain; mechanism + dose rule, non-temporal caveat kept |
| 4 | Pang et al., DevNet, KDD 2019 — doi 10.48550/arxiv.1911.08623 + repo | authors/venue/year/DOI match; CLAIM verified at abstract+repo level only (full text unopened) | P | Retain as directional witness only; re-verify full text before any synthesis weight beyond "tiny supervision moves needle" |
| 5 | Han et al. 2022 (1% supervision, via Lau §2 citation) | primary unfetched; no field check possible | H | REMOVED as support (was S-H1-3). Note: cited-via-Lau snippet only; S-H1-1/S-H1-4 carry the tripwire-1 direction without it |
| 6 | OSAD "Gray and Black Swans" 2022 + Anomaly Heterogeneity Learning 2023 (abstracts) | authors/year/DOI match abstract pages; claims snippet-only | H | REMOVED as support (was S-H1-5). Note: mechanism precedent for hedge design retained ONLY as U-1 follow-up pointer, never as evidence |
| 7 | Alnegheimish et al., M2AD, PMLR v258 (AISTATS 2025) | title/authors/venue/year match PMLR page (abstract+bib fetched); full-text mechanics via source-table, PDF unopened | P | Retain mechanics-only (STATUS covariate + Gamma calibration); +21% number inadmissible per LOCK-6; re-verify PDF-to-MD before synthesis deepens reliance |
| 8 | Islam & Carden, product-aware TEP, preprint May 2026 — arxiv 2606.00052 | title/authors/year/URL match fetched HTML; venue = preprint (as cited, L3) | R | Retain; primary for LOCK-5 direction |
| 9 | SCAL, Meas. Sci. Technol. (IOP 10.1088/1361-6501/aea242) | venue/DOI match abstract page; claims snippet-only, raw-vs-PA protocol unverified | P | Retain as conditioning witness only; re-verify protocol before synthesis weight |
| 10 | DCD-VAE proceedings (Politecnico repository PDF) | repository hit via search snippet; venue/year/fields not field-verified | H | REMOVED as support (was S-H3-4). Note: representation-level precedent dropped; LOCK-5 stands on #7/#8 + HSMM without it |
| 11a | MDPI Processes A19 (steady-state pipeline) | 403-blocked; snippet only | H | REMOVED (was half of S-H3-5). Note: Thibault-style discipline now carried by PHM half alone |
| 11b | PHM-EU circuit-breaker strata (phme 4896/2947) | abstract-level strata claim via search | P | Retain as DOWN-masking discipline precedent (low-medium); re-verify before synthesis weight |
| 12 | sklearn RobustScaler official docs | official page fetched; semantics + fit/transform verified | R | Retain; primary for LOCK-2 tooling half |
| 13 | Ahmed et al., TSFM normalization study, preprint 2025 — ar5iv 2512.02833 | title/authors/year/URL match fetched HTML; venue = preprint (as cited) | R | Retain at L3; statistics-ranking + scope-factorization, forecasting-task caveat kept |
| 14 | Lima & Souza, Big Data Res. 2023 (sciencedirect S2214579623000400) + Lacuna practitioner note | DOI/venue/year match abstract page; claims snippet-only; Lacuna = L4 pattern | P | Retain as heterogeneity-concentration mechanism only; re-verify before synthesis weight |
| 15 | Siffer et al., SPOT/POT/DSPOT, KDD 2017 | title/authors/venue/year match KDD landing (fetched); full text via prior A09 extraction (HAL re-fetch PoW-blocked, logged miss) | P | Retain as design-spec source for LOCK-4 (fields R, depth P); convert HAL PDF to .md and re-verify before synthesis quotes beyond spec numbers |
| 16 | FluxEV, WSDM 2021 (sdiaa PDF) | binary fetch per TEXT-ONLY rule → fields unverifiable this run | H | REMOVED as support (was S-H5-5). Note: A3 cost-companion direction now carried by pre-registered timing plan in H5, not by FluxEV |
| 17 | Schmidl et al., TimeEval evaluation paper, VLDB 2022 (+ timeeval page) | finding text VERIFIED via fetched evaluation page | R | Retain; primary for LOCK-6 threshold-agnostic half + H1-C1 precedent |
| 18 | Ding et al., DRA, CVPR 2022 — arxiv abs 2203.14506 | abstract VERIFIED via fetch (seen-vs-unseen bias quotes) | R | Retain at abstract depth; H1 tripwire-2 threat witness (U-1) |
| 19 | Tuli et al., TranAD, VLDB 2022 — pvldb vol15 Eq.1 + F1/F1* table | §3.2 global min-max + dual-metric table VERIFIED in-session | R | Retain; H4-C1 precedent (scope) + LOCK-6 PA-duality exhibit |
| 20 | Wu & Keogh, "Current TSAD Benchmarks are Flawed" (TKDE 2023 / abs 2009.13807) | standard reference cited via contra; page not re-fetched this run | P | Retain as benchmark-validity witness; re-verify page before synthesis quotation |
| 21 | HSMM+PCA multimode TEP (HAL 03875921) + LCPCA (cjche 2020) | HAL/print identifiers via search snippets; per-mode 100%/98.3% figures snippet-level | P | Retain as H3 tripwire-2 threat witnesses (U-3); re-verify before synthesis weight |
| 22 | Multigrid alert-budget guide (timeout) + J26 prior snippet | fetch MISS; prior J26 snippet stands | P | Retain prior snippet only (budget-math direction); S-H5-4 carried by L4 corroboration without it |
| 23 | TheCodeForge / Arun Baby / MAY2704 (L4 practitioner) | pattern-level snippets, quarantine rule observed (no benchmarks claimed) | P | Retain as practice patterns for LOCK-3 production half |
| 24 | RedLamp (Obata et al. 2025, arxiv 2505.20765v1) + fixed-augmentation drop (2406.10617) + TimeRCD (2509.21190) + bias-effect IJCAI21 (10.24963/ijcai.2021/456) | identifiers via contra query snippets; DRA-half verified (see #18), remainder snippet-level | P | Retain as H1 dose-poison / caricature-overfit threat witnesses (U-1); re-verify before synthesis quotation |
| 25 | Sim2Real fault-diagnosis gap (TMech 2025 CSRA; JIM 2026 review; OSF 2026 mfg review) | DOIs via contra snippets, domain-adjacent scope as cited | P | Retain as sim-label-transfer risk witness (U-1 caveat); re-verify before synthesis weight |

Gap-fill accounting: 0 new fetches consumed (all verdicts above rest on in-run verified material or are dispositioned H/P without new retrieval; ≤5 budget untouched).

*End — 6 Locked (scope-limited, no twin-transfer numbers) · 6 Unresolved theaters (named missing legs) · 0 Refuted · 9 R / 11 P / 5 H (H removed with notes). Synthesis input = LOCK-1..6 only.*
