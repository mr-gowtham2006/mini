# Recommendations — Verdandi anomaly-twin-trace

Date: 2026-09-11 · Each block carries an explicit decision, a concrete tradeoff, verb-led next actions with artifacts+owners, and a numeric drop condition. No platitudes; no unlocked number cited as fact.

---

## Recommendation 1: Pre-register the M0b battery before running it — choose pre-registered PA-off twin-scoped bar over ad-hoc detector comparison

**Decision:** Choose a written, frozen M0b pre-registration (fault list 20–32 seeded, fixed-percentile calibration, raw point-wise F1 only, PA-on control arm, GNN≥0.60 / quantile<0.60 kill-wires) over running the battery first and interpreting later.

**Tradeoffs:** (1) Pre-registration costs 1–2 days of spec writing and forbids post-hoc fault subsetting — but without it, any GNN≤0.50 outcome is dismissible as battery-shopping, and GDN 0.81 SWaT (C-H2-1) already arms the skeptic. (2) Including the PA-on control arm doubles scoring work — but it is the only artifact that converts F1's established protocol bar into a demonstrated collapse gap (≥0.30) on Verdandi's own data.

**Next actions:**
- Write `m0b-preregistration.md` (fault seeds, thresholds, scoring code hash, kill-wires) — owner: detector lead.
- Freeze the quantile + ≥1 GNN baseline configs and log the PA-on control output to `m0b-runtable.csv` — owner: detector lead.
- Pin SOTA2 protocol-1 rows for the cited raw numbers (TranAD 49.51 / MTAD-GAT 41.69 / USAD 30.56) — owner: evidence curator.

**Drop condition:** If any GNN scores raw-F1 ≥0.60 on M0b, drop "learned-graph deprioritized" (keep the raw-F1 bar itself — F1 is Locked); if quantile scores <0.60, drop the quantile-detector choice and re-open detector selection.

## Recommendation 2: Re-scope H2/H3/H4 numbers to twin-battery form now — choose scoped-and-testable over universal-and-refuted

**Decision:** Choose the three scoped replacements in F5 (H2: M0b-conditional; H3: sim-scoped AC@1≥70% + ≥10pp gap; H4: template≥90% vs open+RAG, n≥50) over retaining any universal phrasing (GNN≤0.50 general, RCAEval-ceiling general, open≤65% general — all refuted).

**Tradeoffs:** (1) Scoped claims read weaker in a viva (0.80–0.81 "real-path on our battery" vs "beats SOTA") — but the universal versions are already falsified by Eadro 0.982 / SimpleRCA 0.93 / FIDES 92–94%, so retaining them risks a one-citation kill. (2) Relaxing H4 95%→90% concedes headline strength — but C-H4-4's forced-hallucination mechanism makes 95%+zero-escapes the riskier bet; 90% + escape taxonomy is auditable where 95% is brittle.

**Next actions:**
- Rewrite H2/H3/H4 hypothesis files to the scoped forms with the F5 tripwires verbatim — owner: synthesis lead.
- Purge universal phrasing (GNN≤0.50, ceiling, open≤65%) from all viva-facing docs — owner: synthesis lead.
- Pre-register the BARO-style graph-free ablation + DELAY/LOSS miss-tagging (OQ-3/OQ-6) alongside M0b — owner: ranking lead.

**Drop condition:** If the graph-free ablation gap lands <10pp, drop the topology-prior-causes-ranking leg and concede BARO parity (H3's registered alternative); if template+verifier lands <90%, drop template-superiority and reframe as agent+verifier (H4's alternative).

## Recommendation 3: Run the 50-alarm RAG-control audit with an escape taxonomy — choose open+RAG comparator + fallback-fire-rate honesty over pure-open strawman

**Decision:** Choose the audit design "template+verifier vs open+RAG on the same n≥50 alarm set, per-sentence provenance-ID check, escape taxonomy, fallback-fire rate reported" over "template vs pure open narration."

**Tradeoffs:** (1) Building the open+RAG control + taxonomy costs a full audit pass (50 alarms × 2 arms + adjudication) — but pure-open is already knocked down by FIDES/Grounded-Decoding, so beating it proves nothing and invites the strawman charge. (2) Publishing fallback-fire rate exposes how often templates contribute nothing — but if the verifier rejects constantly (H4 alternative (2): effectively fallback-only), hiding it converts a null result into a false 1.00-style headline.

**Next actions:**
- Assemble the 50-alarm set with frozen provenance-ID schema and verifier rules — owner: narration lead.
- Run both arms, log per-sentence verdicts + escape classes + fallback-fire rate to `alarm-audit.csv` — owner: narration lead.
- Fetch the full ProvenanceGuard paper + VeriTrail paper (H4-S4/S5 follow-ups owed) to harden the verifier design — owner: evidence curator.

**Drop condition:** Template+verifier <45/50 (90%) → drop template-superiority; open+RAG ≥80% → drop verifier-necessity; any non-fallback unlinked escape → drop "zero escapes" and ship only with the escape taxonomy disclosed.

## Recommendation 4: Bet the wedge on the provenance leg explicitly — choose provenance-first positioning over trace-only or replay-only differentiation

**Decision:** Choose to position Verdandi on per-sentence provenance + verifier (the leg Eadro/RCAEval/Cloud-OpsBench all lack) over ranked-trace quality or seeded replay alone (both matched: Eadro HR@1 0.982; RCAEval/Cloud-OpsBench reproducibility).

**Tradeoffs:** (1) Provenance is H4 — the riskiest technical leg (template≥95% pressured, C-H4-4) — so the market bet concentrates rather than diversifies risk. Mitigation: the bet is explicit and hedged (packaging-reposition fallback pre-agreed). (2) CPU-local <10min reads as a differentiator but is table stakes (LEAD-XNet 96.7% @22.5ms) — keep it as a logged constraint, never as the headline.

**Next actions:**
- Make the H4→H5 dependency explicit in the registry (H5 status flips with the OQ-4 outcome) — owner: synthesis lead.
- Start the quarterly 14-row feature-matrix re-audit cadence (`competitor-matrix-YYYYQn.csv`) — owner: evidence curator.
- Draft the packaging-fallback positioning ("pedagogy artifact") now, so H4-failure triggers a pivot, not a scramble — owner: synthesis lead.

**Drop condition:** H4 audit fails (R3 drop conditions) → downgrade H5 to packaging/pedagogy immediately; any quarterly re-audit with ≥1 verified full-row T+P+R hit → retire H5 outright.

## Recommendation 5: Speak per-alarm triage-time only — choose seconds-per-alarm framing over any fleet-MTTR or ROI language

**Decision:** Choose "cut per-alarm hand-triage (30–60 min norm) to ranked cause + replay in seconds" over every fleet-level variant (MTTR reduction, downtime-dollars saved, payback period).

**Tradeoffs:** (1) Per-alarm framing forfeits the biggest numbers (Davis ~90% MTTR on large deploys; PdM −50–75% downtime; ~$260k/hr) — but contradictions-map row 5 is Unresolved with neither side meeting controlled-measurement bar, and I-S17 (+41% MTTR vs runbooks, 22% vs 3% misdiagnosis) makes any fleet claim a live target. (2) Student/viva inversion (marks for MTTR-dollars, replay hash for CMMS audit) reads as scope-dodging to an industry examiner — pre-empt with the disclosed Gap-5 brownfield statement (OQ-5): transfer unmeasured, topology fixed per semester with drift flip-gates.

**Next actions:**
- Grep all viva/docs for "MTTR", "payback", "ROI", "$/hr" and rewrite each to per-alarm seconds or delete with justification — owner: synthesis lead.
- Log the <10min CPU run with wall-clock + seed hashes as the standing runtime artifact — owner: ranking lead.
- Record M3 limits (zero-marginal-cost non-transferable; fixed-per-semester topology; no safety-restart authority) in the artifact README — owner: synthesis lead.

**Drop condition:** If the timed per-alarm loop (detect→rank→narrate→replay) exceeds 60 s/alarm median on the logged CPU run, drop "in seconds" to the measured median; if full-pipeline runtime hits ≥10 min CPU, drop the CPU-local leg (H5 tripwire).
