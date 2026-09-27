# Findings — Verdandi anomaly-twin-trace synthesis

Date: 2026-09-11 · Scope: 01-background + 02-hypotheses (H1–H5) + 03-evidence (supporting + contradicting + evidence-log + claim-lock-annex). No new retrieval.
Rule honored: Locked claims appear as established; Unresolved stay open; refuted universals appear as refuted with scoped replacements. No unlocked numeric claim (F1~0.73, AC@1 0.80–0.81, flip 14.4%, 1.00@50) is presented as established.
Confidence rubric: High ≥85% requires L1–L2 support; Moderate 70–85% requires L3+ survived falsification; else Contested with no number.

---

## F1 [ESTABLISHED — Locked] Point-adjusted F1 is invalid as detector proof; raw point-wise F1 only is admissible

**Claim:** Any detector ranking computed under point-adjustment (PA) is inadmissible as evidence. Only raw point-wise F1 (fixed-percentile calibration, PA-off) counts. Verdandi's quantile-detector numbers must be read as conditional on the still-owed M0b battery (see F5), but the protocol bar itself is established.

**Confidence:** High, 85–90% (proof-level critique + independent replication + negative-result demonstration; L1–L2 base).

**Evidence strength:** GRADE High — 6 sources: Kim et al. AAAI 2022 (A-S8, L1, random-scores-become-SOTA under PA); Schmidl/Wenig/Papenbrock VLDB 2022 (L1, all-anomalous ≈0.43 point-wise); Quo Vadis TSAD position 2024 (L2); TCN-GAT 0.886→0.281 under fixed-percentile calibration, IEEE ICCI 2026 (A-S14, L2, 53× shift); NeurIPS 2024 "Elephant in the Room" (L1, point-wise Standard-F1 as the kept metric); MTAD benchmark arXiv:2401.06175 (C-H2-5, KNN beats deep models raw but loses post-PA — metric-dependence confirmed). Counter-search run: no defense of PA survived.

**Key sources:** https://arxiv.org/pdf/2109.05257 · https://www.vldb.org/pvldb/vol15/p1779-wenig.pdf · https://proceedings.neurips.cc/paper_files/paper/2024/file/c3f3c690b7a99fba16d0efd35cb83b2c-Paper-Datasets_and_Benchmarks_Track.pdf · https://www.sota2.com/research/sota/anomaly-detection-on-wadi (raw WADI: TranAD 49.51 / MTAD-GAT 41.69 / USAD 30.56)

**Remaining uncertainty:** Event-wise/composite metrics (Schmidl) may complement point-wise F1 for operational usefulness; locked claim covers admissibility only, not which raw metric is optimal. SOTA2 leaderboard rows are protocol-sensitive (TranAD 2023.08 row differs by protocol) — exact raw numbers need pinned protocol-1 rows on a fetch pass.

**Alternative explanations:** (1) A defender could argue PA approximates event-level utility (one detected segment = one caught incident); rebutted — PA lets random scores reach SOTA, so it cannot discriminate methods even on its own terms.

**Would-be-overturned-by:** A published proof or replication showing a PA-based ranking that (a) preserves method ordering under raw point-wise re-scoring (Spearman ρ ≥ 0.90 across ≥10 methods on ≥2 datasets) AND (b) random-score controls scoring below the 10th percentile under PA. Numeric tripwire: random-control PA-F1 < 0.20 while method ranking preserved — never observed; Kim's result shows the opposite.

---

## F2 [ESTABLISHED — Locked] Horizontal-platform twin framing is killed; the narrow sim-owned job is the documented exception path

**Claim:** Verdandi must never be framed as a platform/digital-twin ROI story. The defensible frame is the semester inversion: sim-owned ground truth, upkeep-free CPU-local bundle, narrow job (per-alarm ranked trace + replay in seconds). Fleet-MTTR or payback phrasing is prohibited.

**Confidence:** High, 85–90% (convergent evidence across peer-reviewed + postmortem + press + project ADR record; counter-search run).

**Evidence strength:** GRADE High — 7 sources: ICIS 2023 IIoT graveyard (I-S13, L1, 1st-wave failure ~2018); Predix postmortem $4–7B burn via horizontal overreach (I-S13/I-S14); Uptake stalled despite focus (I-S15, L4); Forbes Tech Council twins-failed-on-decision-web 2026 (I-S19, L3); Snatika reality-gap sim+15%→real−2% (I-S20, L4); IoTDT ROI-back-loaded fact-check (I-S21, L4); project ADR record (FactorySimPy rejection, hand-rolled seeded twin). ≥2 independent domains + primary artifacts.

**Key sources:** source-table I-S13–I-S15, I-S19–I-S21 · literature-review §M1/M3 · contradictions-map row 6 (Leaning-B for platform framing)

**Remaining uncertainty:** Siemens Insights Hub + Twin Composer (I-S11/I-S12, €6.8B DI SW revenue) shows asset-modeling twins can sustain license revenue — revenue ≠ deployment ROI, but a Siemens-academia bundle could theoretically occupy an adjacent slot (audited as miss on upkeep-free scope, H5-S4).

**Alternative explanations:** (1) Predix/Uptake failed on execution (pricing, sales motion), not on platform scope — a well-executed platform could still win; countered by the convergent decision-workflow-adoption failure (I-S19/S21), which is scope-intrinsic, not sales-intrinsic.

**Would-be-overturned-by:** A controlled deployment study showing a horizontal manufacturing twin with measured (non-vendor-hosted) payback ≤12 months across ≥5 brownfield sites with upkeep cost itemized. Numeric tripwire: independently audited ROI-positive at ≥5 sites with upkeep ≤20% of license cost — no such study surfaced in 11 industry queries.

---

## F3 [ESTABLISHED — Locked] No competitor covers ranked-trace + provenance + seeded-replay jointly at semester scope (0/14 through 2026-09)

**Claim:** Against the 14-entry M2 table (Datadog/Dynatrace/Splunk/PagerDuty/BigPanda, Siemens Insights Hub+Twin Composer, PyRCA, RCAEval ecosystem, FactorySimPy, Merlion, PyOD/ADBench/GDN/USAD, LLM-RCA startups), zero competitors jointly deliver (T) ranked upstream trace ≤3 steps + (P) per-sentence provenance narration + (R) seeded replay for factory-twin alarms on CPU-local. Nearest misses cover ≤2 legs. Vendor claims gated out; finding expires without re-audit cadence.

**Confidence:** High, 85–88% (14-entry audit + 5-query falsification counter-search across academic/tech/web; L2–L3 base for OSS rows, vendor rows correctly discounted — downgraded from higher only by 9/14 M2 rows still unaudited row-by-row).

**Evidence strength:** GRADE Moderate-High — 9 sources: PyRCA repo+docs (H5-S1, T✓ P✗ R✗); Datadog Watchdog/Bits (H5-S2, ≤1/3); Dynatrace Davis deterministic path + timeline-"replay" ≠ seeded replay, terminology guard recorded (H5-S3); Siemens academia kit (H5-S4, misses upkeep-free scope); FactorySimPy + Merlion (H5-S5, sim-side ≤2/3 from opposite direction); falsification nearest-miss table (H5-contra: Eadro T+P⚠️, RCAEval T+R, Groot T+P⚠️, Cloud-OpsBench T+R, DetTrace R-core) — every row fails the full conjunction.

**Key sources:** https://github.com/salesforce/pyrca · https://github.com/FactorySimPy/FactorySimPy · https://github.com/salesforce/Merlion · https://github.com/phamquiluan/RCAEval · Dynatrace Davis RCA pages · Siemens Insights Hub for Academia sheet

**Remaining uncertainty:** 9/14 M2 rows lack row-by-row re-audit; a post-2026-09 entrant or a PyRCA/RCAEval-adjacent combo could bundle all three. CPU-local <10min is table stakes, not a moat (LEAD-XNet 96.7% @22.5ms edge literature) — the moat is the conjunction only. H5 inherits H4's provenance-leg risk (see F6).

**Alternative explanations:** (1) The conjunction is unoccupied because it is valueless outside the viva (no brownfield pull — Gap 5); the wedge is packaging, not a gap. This is live and unresolved — F6 carries it.

**Would-be-overturned-by:** Any single product/repo demoing T+P+R jointly for factory alarms on CPU-local with verifiable artifact (repo/docs/paper). Numeric tripwire: ≥1 competitor with all 3 legs verified, OR Verdandi runtime ≥10 min CPU on the logged run — either falsifies the wedge outright.

---

## F4 [CONDITIONAL — H1 corroborated-conditional] Topology-masked PCMCI+ (tau=2) + flip-gate + seed sweep is the defensible causal layer, conditional on mask correctness; the 14.4% number is untested

**Claim:** Masked PCMCI+ with flip-gate/seed-sweep (n≥800, ≥5 seeds) survives as Verdandi's causal-discovery posture: no same-battery evidence trips masked-flip >14.4% or blind ≤19.4%. BUT the numeric tripwire is untested anywhere (Gap 1: no published 32-node flip-gate study), and prior-fragility literature weakens the mechanism — a wrong mask can hurt vs blind. Project RQ2 flip 14.4% and retention ≥80% are project observations, not established facts. Status per annex: corroborated-conditional.

**Confidence:** Moderate, 70–78% for the posture (L1–L2 mechanism support, wire untripped across 5 falsification queries); Contested, no number, for the 14.4%/19.4% numeric values themselves.

**Evidence strength:** GRADE Moderate — supporting 5 (Runge 2019 Sci Adv + Runge 2020 UAI/PMLR L1 for the tau=2 base; Debeire Bagged-PCMCI+ PMLR 2024 L2 for the seed-vote precedent — precision+recall gain; IECR 2024 L1 + COKE arXiv:2407.12254 L2 for topology-prior precedent; Sandia OSTI 1991387 for spurious-link framing) vs contradicting 5 (defeasible-prior failure arXiv:2609.03442; imperfect-prior sharp degradation arXiv:2511.068xx/2511.06790; KCRL/KGS "helps ONLY when prior completely true"; Debeire base-instability implying credit may belong to averaging, not the mask). Authors of S3–S5 unverified (snippet-only) — fetch pass owed.

**Key sources:** https://proceedings.mlr.press/v236/debeire24a.html · https://proceedings.mlr.press/v124/runge20a/runge20a.pdf · https://doi.org/10.1021/acs.iecr.4c01155 · https://doi.org/10.48550/arxiv.2407.12254 · https://doi.org/10.2172/1991387

**Remaining uncertainty:** Everything numeric: masked flip ≤14.4%, blind-vs-masked gap ≥10pp, retention ≥80% — all await the same-battery masked-vs-blind run. Whether stability credit belongs to the mask vs tau/n/averaging is unseparated. Mask-correctness sensitivity unmeasured (10%-corruption ablation owed).

**Alternative explanations:** (1) H1's own registered alternative: blind discovery suffices at this scale; stability comes from tau/n choice + sample length + seed-averaging, mask is cosmetic → demote mask to optional preprocessing. (2) Prior-fragility: the P&ID mask contains imperfect edges and actively suppresses true links (C-H1-1/2 mechanism), so masked flip EXCEEDS blind — the mask is load-bearing in the wrong direction.

**Would-be-overturned-by:** Same-battery run (≥5 seeds, n≥800, seed hashes): masked flip >14.4% → H1 false; blind flip ≤19.4% (within ±5pp) → prior adds nothing → H1 false. Secondary tripwire: 10%-mask-corruption ablation degrading masked flip by <3pp (mask carries no signal) or improving it (mask was harmful) — either demotes the mask per alternative (1)/(2).

---

## F5 [CONTESTED — H2/H3/H4 joint verdict] All three universal numeric bars are refuted as generals; their twin-scoped replacements are unresolved and must be re-scoped before any battery runs

**Claim:** H2, H3, H4 each survive ONLY in re-scoped twin-battery form. Their universal readings were provisionally falsified by the contradicting sweep, and presenting any of them as established is prohibited. Each scoped replacement below names its missing leg.

### F5a. H2 detector bar — universal "GNN ≤0.50 raw" REFUTED; twin-scoped form unresolved (M0b owed)
Refutation: GDN F1 0.81 on SWaT computed point-wise over test ground truth (Deng & Hooi AAAI 2021, Table 2, C-H2-1) trips the GNN≥0.60 wire — universality does not survive. (WADI 0.57 conforms; MTAD-GAT/GTAD 0.80–0.95 numbers are PA-inflated per Kim and do not count either way.) Scoped replacement: "On the M0b twin battery (20–32 seeded faults, fixed-percentile calibration, PA-off pre-registered): GNN raw-F1 ≤0.50 AND quantile raw-F1 ≥0.65 AND PA-on control flips/compresses the gap." The quantile ≈0.73 (M0b) figure is conditional, not established.
Evidence: GRADE Moderate — supporting 4 (Kim PA-invalid L1; SOTA2 raw WADI L4; Schmidl VLDB L1; NeurIPS-2024 benchmark L1) vs refuting 1 (GDN raw 0.81 L1). Confidence: Contested, no number for any H2 numeric until M0b.
Alternative explanations: (1) Ranking is dataset-specific, not protocol-driven — a calibrated deep detector transfers to the twin series and beats quantile on raw F1; then the raw bar stays but "learned-graph deprioritized" is revoked. (2) The twin's fault mix is WADI-easy for GNNs (GDN 0.81 precedent) rather than collapse-regime — battery mix decides.
Would-be-overturned-by (scoped): any GNN raw-F1 ≥0.60 on M0b under identical calibration → bar unnecessary → H2 false; quantile raw-F1 <0.60 → H2 false. Collapse-gap check: PA-on must flip ranking or compress gap ≥0.30, else protocol-neutrality fails.

### F5b. H3 AC@1 bar — general "RCAEval 0.46–0.54 ceiling" REFUTED; sim-scoped ≥70% + ≥10pp ablation gap unresolved (battery + graph-free ablation owed)
Refutation: Eadro HR@1 0.982 (ICSE 2023), Nezha 0.87, SimpleRCA 0.93 on Nezha-TT, Groot top-1 78% on 952 production incidents (C-H3-1/2/5) clear 70%+ top-1 elsewhere — the ceiling holds for RCAEval-style data only (C-H3-4: CIRCA 0.46/RCD 0.54 Avg@5, BARO AC@1 rows 0.0–0.33 on network faults). Ablation-gap premise separately threatened: SimpleRCA (3-sigma/P95, NO topology prior) matches/beats 11 SOTA models; ASE'24 causal≈Dummy finding. Scoped replacement: "Ranked trace depth≤3 over topology prior on OUR seeded battery (20–32 faults, CPU <10min): AC@1 ≥70% (target 0.80–0.8125 real-path) AND graph-free (BARO-style) ablation gap ≥10pp, all-seeds-counted, DELAY/LOSS misses disclosed." The 0.80–0.8125 real-path figure is a project observation, not established.
Evidence: GRADE Moderate — supporting 5 (RCAEval L1 ceiling comparator; BARO FSE 2024 L1 as ablation control; RCAEval repo DELAY/LOSS miss pattern L3; CIRCA-graph split L2; KRCA 0.88 achievability precedent L3) vs refuting 3 (Eadro/SimpleRCA/Groot). Confidence: Contested, no number until battery + ablation.
Alternative explanations: (1) Sim labels flatter ranking; graph-free BARO matches topology-prior trace → concede BARO parity, scope down to Avg@K. (2) Twin faults are Nezha-easy, so ≥70% is unambitious and proves nothing about the prior — the ablation gap, not the bar, is the real test.
Would-be-overturned-by (scoped): AC@1 <0.70 full battery (<14/20 or <23/32 top-1, all seeds, no subsetting) → H3 false; ablation gap <10pp → topology-prior leg revoked (alternative (1) triggers); runtime ≥10 min CPU → CPU-local leg false.

### F5c. H4 grounding posture — universal "open narration ≤65%" REFUTED; template ≥95% pressured; mechanism (not numbers) is Locked
Refutations: FIDES 92–94% context fidelity (+14–28pts over standard RAG, arXiv:2606.05644); Grounded Decoding best-FActScore/citation-F1 (arXiv:2606.00432); TAMO-FoA 76.3% production RCA accuracy (IEEE CCWC 2026) — open+RAG narration demonstrably exceeds 65% (C-H4-1/2/3). Other direction: constrained decoding guarantees SHAPE not truth — forced-hallucination-to-satisfy-grammar mechanism (BoundaryML 2025-12, TianPan 2026-04, TDS 2026) predicts verifier-passing-but-wrong IDs, pressuring template ≥95% + "zero unlinked escapes" (C-H4-4). LOCKED (established) sub-claim: the verifier-loop mechanism is sound — ReAct grounding cuts correct-retrieval hallucinations from 26% to <1–6% (arXiv:2403.04123). Scoped replacement: "On the same n≥50 factory-alarm set with open+RAG (not pure-open) control: template+verifier ≥90% (relaxed from 95%) with escape taxonomy AND zero unlinked escapes; comparator is open+RAG." Template-level 1.00@50 is a project observation, not established.
Evidence: GRADE Moderate — supporting 5 (OpenRCA 2.0 ungrounded-diagnosis band L2; 12-pitfall taxonomy L2; TAMO tool-grounding L1; ProvenanceGuard per-claim verifier precedent L2; VeriTrail L5 low-weight) vs refuting 4 (FIDES/Grounded-Decoding/TAMO-FoA + false-confidence triad). Confidence: Contested, no number until the 50-alarm RAG-control audit.
Alternative explanations: (1) A KRCA/TAMO-style tool-agent matches template grounding without templates → reframe as agent+verifier posture. (2) The verifier rejects so often the system is effectively fallback-only → templates add no coverage; report fallback-fire rate honestly.
Would-be-overturned-by (scoped): template+verifier <90% (<45/50 fully provenance-linked) → H4 false; open+RAG control ≥80% on same set → verifier unnecessary → H4 false; any single unlinked sentence escaping the verifier (non-fallback) → "zero escapes" leg false.

---

## F6 [CONDITIONAL — H5 contested-conditional] The wedge survives conditionally; its fate is tied to H4's provenance audit

**Claim:** Zero full-row (T+P+R) hits after 5 falsification queries across academic/tech/web — H5 not falsified. But survival is conditional twice over: (a) on H4's 50-alarm audit (Eadro + RCAEval jointly cover T+R without P — the wedge rests entirely on the provenance leg, itself provisionally falsified); (b) on re-audit cadence (9/14 M2 rows unaudited, post-2026-09 entrants unmonitored). CPU-local <10min is table stakes (edge-twin literature), not a moat — the conjunction is the moat. Status per annex: contested-conditional.

**Confidence:** Contested, no number (all-grey L3/L5 evidence base, expected for a market wedge; inherits H4 risk).

**Evidence strength:** GRADE Low-Moderate — supporting 5 grey (H5-S1–S5, all L3/L5, high-relevance but vendor-discounted) + falsification nearest-miss table (8 competitors, best ≤2 legs) + gap-pass (no new post-2026-09 full-row entrant). Strongest legs: PyRCA and FactorySimPy/Merlion misses from opposite sides (≤2/3 each).

**Key sources:** H5 supporting note S1–S5 URLs · H5-contra nearest-miss table (Eadro ICSE'23 HR@1 0.982; RCAEval WWW'25 735 cases; Groot ASE'21 952 incidents top-1 78%; Cloud-OpsBench 2026 452 faults frozen-twin; DetTrace deterministic replay; LEAD-XNet/VARADE++/LiZAD edge twins)

**Remaining uncertainty:** Whether the empty niche signals opportunity or valuelessness (Gap 5: zero brownfield-transfer evidence — topology drift, sensor noise, operator-trust curve all unmeasured). Per-alarm triage-time cut (seconds vs 30–60 min hand-triage) is the honest claim; any fleet-MTTR phrasing fails audit (contradictions-map row 5: unresolved, neither side meets controlled-measurement bar).

**Alternative explanations:** (1) A Siemens/PyRCA/RCAEval-adjacent combo already bundles all three (wedge is packaging). (2) The niche is empty because it is valueless outside the viva → reposition as pedagogy artifact, not market wedge.

**Would-be-overturned-by:** ≥1 competitor verified with all 3 legs jointly → wedge false; H4 audit failing (template <90% or open+RAG ≥80%) → provenance leg collapses → wedge downgraded to packaging per alternative (2); runtime ≥10 min CPU → CPU-local leg false. Re-audit tripwire: any quarterly re-audit finding a full-row hit retires H5 immediately.

---

*End of findings — 6 findings: F1–F3 established (Locked), F4 conditional, F5 contested (3 sub-verdicts incl. 3 refuted universals with scoped replacements), F6 conditional. All findings carry ≥1 alternative + numeric overturn tripwire.*
