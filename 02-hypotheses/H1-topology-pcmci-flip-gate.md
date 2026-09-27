# H1 — Topology-constrained PCMCI+flip-gate stability claim

Status: proposed
Priority: P0 (load-bearing for RQ2 / causal layer)

## Claim
X produces Y under Z: Topology-masked PCMCI+ (tau=2) with flip-gate + seed sweep on the 32-machine twin (n≥800 samples per run) produces a stable causal edge set with flip rate ≤14.4% and ≥80% edge retention across ≥5 seeds, whereas blind (unmasked) PCMCI+ on the same data produces flip rate ≥30% or spurious-link count ≥2× masked.

## Origin
Source contradiction C1 (blind discovery scales [A-S23 0.90–0.94 F1] vs blind discovery unreliable on 32 nodes/short runs [A-S1/S2 contract, A-S4/A-S5/A-S6 spurious-link evidence, project RQ2 flip 14.4%], leaning-B). Gap 1: no published topology-constrained PCMCI + flip-gate study at 32-node factory scale with n≈800 seed sweeps.

## Falsification
Would be disproven by: masked PCMCI+ run on the twin battery showing flip rate >14.4% (edge presence/absence across ≥5 seeds) OR blind PCMCI+ matching masked stability within ±5pp flip rate on the same battery. Numeric tripwire: masked flip >14.4% → H1 false; blind flip ≤19.4% → topology prior adds nothing → H1 false.

## Predictions
1. Masked edge set: flip rate ≤14.4% across ≥5 seeds (n≥800, tau=2).
2. Blind edge set: flip rate ≥30% or spurious edges ≥2× masked count (spurious = edges absent from P&ID topology mask).
3. Removing the mask while holding tau/n fixed degrades retention below 80%.

## Alternatives
Most likely if false: A-S23-style blind discovery is sufficient at this scale and the mask/flip-gate is cosmetic (stability comes from tau/n choice or sample length, not the topology prior). Then demote mask to optional preprocessing and credit bagging/seed-averaging alone.

## Verification-method
Retrieval/evidence type: experimental twin-battery evidence (seeded replay, M0b-style): run masked vs blind PCMCI+ on identical seeded fault series, ≥5 seeds, n≥800; decide by flip-rate/retention counts. Secondary: targeted literature retrieval on bagged-PCMCI+ stability (Debeire 2024) as comparator only — cannot confirm H1 alone.

## Expected-outcome
If holds, MUST observe: masked flip ≤14.4% AND blind-vs-masked flip gap ≥10pp on the same battery, recorded with seed hashes.

## Priority / Status
Priority P0. Status: proposed (awaiting battery runs; no retrieval in this phase).
