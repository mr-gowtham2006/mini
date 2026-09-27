# Contradicting evidence — H5 (semester-niche wedge: 0/14 competitors cover all three)

Date: 2026-09-11 · Agent: falsification-agent · Target: H5-semester-niche-wedge.md
Tripwire: falsified by ≥1 competitor with all 3 jointly (ranked-trace + provenance + seeded replay) OR runtime ≥10min CPU.
(Three = ranked-trace localization + provenance-grounded reporting + seeded-replay evaluation.)

## Queries (5, cascade academic→tech→web)
1. (tech) "Eadro Nezha end-to-end troubleshooting traces metrics logs root cause open source"
2. (tech) "open source AIOps root cause analysis deterministic replay reproducible seeded evaluation RCAEval"
3. (web) "Dynatrace Davis Datadog Watchdog root cause analysis provenance explainability incident"
4. (academic) "Groot event graph root cause analysis industrial deployment eBay provenance explainable"
5. (web) "digital twin lightweight anomaly detection edge CPU real-time factory manufacturing small scale"

## Relevance gate
Kept competitors evidencing ≥1 wedge leg with verifiable artifacts (repo, paper numbers, docs). Dropped pure-marketing pages without feature specifics and microcontroller vibration toys (different scope).

## Nearest-miss table (legs: T=ranked trace, P=provenance, R=seeded replay)
| Competitor | T | P | R | Full row? |
|---|---|---|---|---|
| Eadro (ICSE'23, open code+data, interpretable event patterns, HR@1 0.982) | ✅ traces+logs+KPIs end-to-end | ⚠️ interpretable patterns, no ID-linked provenance audit | ⚠️ released data, no seeded-replay battery | NO |
| RCAEval (WWW'25, 735 cases, 15 baselines, CI reproduction) | ✅ 15 methods incl. trace-based | ❌ no reporting/provenance leg | ✅ reproducible + CI (closest to R) | NO |
| Groot (ASE'21, eBay prod, 952 incidents, top-1 78%) | ✅ event causality graph | ⚠️ rule-customizable, SRE-trusted, no ID audit | ❌ production incidents, no replay | NO |
| Dynatrace Davis / Datadog Watchdog RCA | ✅ topology+trace causal RCA at scale | ⚠️ "every reasoning step explained/proven" (vendor claim, unverified) | ❌ closed-source, no replay | NO |
| DetTrace (deterministic replay forensics, 0.93 confidence, 0 FP) | ⚠️ execution-divergence, not ranked-trace RCA | ⚠️ blast-radius forensics | ✅ deterministic replay core | NO |
| Cloud-OpsBench (2026, 452 faults, frozen twin, 100% reproducible) | ✅ agentic RCA | ❌ | ✅ frozen-context reproducibility | NO |
| TAMO-FoA (prod 76.3%, token-efficient) | ✅ | ❌ | ❌ | NO |
| Edge twins (LEAD-XNet 96.7%, 22.5ms; VARADE++; LiZAD) | ❌ (detection only) | ❌ | ❌ | NO — but voids "CPU-local <10min" as differentiator: lightweight local inference is commoditized |

## Gap-pass
M2's 14-entry table was not re-audited row-by-row here (confirmation agent's job); no NEW post-2026-09 competitor with all three legs surfaced. The conjunction holds for now. Runtime leg unchallenged (no evidence the twin pipeline needs ≥10min).

## Field-level citation check (R/P/H)
No competitor claims all three legs in one verifiable artifact → no Hit. Nearest misses (Eadro, RCAEval, Cloud-OpsBench) each cover ≤2 legs. Vendor provenance claims (Dynatrace) are unverified marketing — correctly gated OUT as non-evidence either way.

## Verdict: NOT FALSIFIED
Zero full-row hits after 5 queries across academic/tech/web cascades. The wedge's differentiator analysis sharpens, though: (a) CPU-local <10min is table stakes, not a moat (edge-twin literature); the moat is the CONJUNCTION. (b) Eadro + RCAEval jointly cover T+R without P — H5's survival rests entirely on the provenance leg, which is also H4 (Provisionally falsified). H5 inherits H4's risk: if provenance grounding disappoints, the wedge collapses to packaging. Recommendation: H5 survives conditionally; explicitly tie its fate to H4's audit outcome in the registry.
