# H4 — Template+verifier 95% grounding posture

Status: proposed
Priority: P1 (load-bearing for narration trust)

## Claim
X produces Y under Z: Template narration with mandatory per-sentence provenance IDs + verifier gate and fallback on the twin alarm set (n≥50 alarms) produces grounding rate ≥95% (target template-level 1.00@50), where open-ended LLM narration on the same alarms produces grounding ≤65%.

## Origin
Source contradiction C3 (LLM-RCA deployable [A-S19 KRCA 0.88/0.79; A-S18 TAMO] vs open narration systematically ungrounded 38.5% [A-S21], 12 pitfalls/1,675 runs [A-S22], simplest-cases-only [A-S20]; leaning-B scoped → K3 fallback posture). Gap 3: template+verifier 1.00@50 has no direct comparator — report as template-level, not LLM-level.

## Falsification
Would be disproven by: template+verifier grounding <95% on n≥50 alarms (fewer than 48/50 fully provenance-linked sentences passing the verifier), OR open narration grounding ≥80% on the same set (gap closed without the verifier). Numeric tripwire: template grounding <0.95 → H4 false; open grounding ≥0.80 → verifier unnecessary → H4 false.

## Predictions
1. Template+verifier: ≥48/50 alarms fully grounded (every sentence carries a valid provenance ID, verifier passes or fallback fires).
2. Open narration control: grounding ≤65% (≈ A-S21 38.5% ungrounded rate reproduced or worse).
3. Every verifier rejection terminates in the deterministic fallback, never in an unlinked sentence.

## Alternatives
Most likely if false: a tool-grounded agent (KRCA/TAMO-style) matches template grounding without templates, or the verifier rejects so often the system is effectively fallback-only (templates add no coverage). Then reframe as agent+verifier posture and report fallback-fire rate honestly.

## Verification-method
Retrieval/evidence type: experimental alarm-set evidence: run template+verifier vs open-narration control on the same ≥50 alarms; decide by provenance-ID audit counts. Literature (OpenRCA ungrounded rate, KRCA/TAMO tool-grounding) supplies comparators only.

## Expected-outcome
If holds, MUST observe: template grounding ≥95% AND open-control grounding ≤65% AND zero unlinked sentences escaping the verifier on the same alarm set.

## Priority / Status
Priority P1. Status: proposed (awaiting alarm-set audit; no retrieval in this phase).
