# H2-contra — Devil's advocate: deep CLEARS the classical wire (≥ +3pp raw)

Date: 2026-09-13 · Scope: contradict H2-gate-survives (`H2-classical-tripwire-gate.md`).
Falsification target: find published `F1(deep) − max(F1(KNN), F1(PCA)) ≥ 0.03` precedent transferable to the twin M0b battery (raw, PA-off).
Method: 5 falsification queries (all 2026-09-13), 0 fetches (relied on full-text snippets + prior verified TranAD fetch in-session). TEXT ONLY; no invented sources.

## Query log (5/5 — quota met before verdict)
| # | Query | Engine | Yield |
|---|---|---|---|
| Q1 | TranAD GDN MTAD-GAT multivariate TSAD F1 SWaT WADI beating classical baselines | academic | TranAD +17.06% F1; GDN WADI +54% F-measure over next baseline; MTAD-GAT +1/+1/+9pp |
| Q2 | "Sign of the Times" TSAD simple baseline beats deep learning | academic | KTH thesis (deep progress "illusory") — SUPPORTING-side hit, disclosed below |
| Q3 | Anomaly Transformer DCdetector OmniAnomaly USAD F1 vs iForest LOF OC-SVM | academic | USAD +0.096 avg over SOTA; DCdetector SOTA on 8 benchmarks; MEMTO vs LOF/OC-SVM/iForest |
| Q4 | deep learning AD manufacturing predictive maintenance outperforms classical bearing vibration | academic | CNN-Transformer SSL ROC-AUC 0.878 vs classical; DIDAD +16.5pp/+10.2pp over OC-NN/DSVDD/ANOGAN; ICA-LSTM 99.4% F1 |
| Q5 | Wu Keogh current TSAD benchmarks flawed trivial one-liner matrix profile UCR | academic | Illusion-of-progress paper (TKDE'23/ICDE'22) — SUPPORTING-side hit, disclosed below |

## Contra evidence (fires at gate-holds)
- **C1 — TranAD, VLDB 2022 (`https://doi.org/10.48550/arxiv.2201.07284`):** avg F1 0.8802, *"improvement of up to 17.06% in F1"* over SOTA baselines; beats all baselines on all datasets except MSL (GDN 0.9591 there). Margin dwarfs +3pp. Weight: HIGH on magnitude, DISCOUNTED on protocol (F1 = PA-on; F1* PA-off variant also reported: avg F1* 0.8012 — still SOTA-leading but the PA-off classical delta is not isolated in the paper).
- **C2 — GDN, AAAI 2021 (`https://bhooi.github.io/papers/gdn_aaai2021.pdf`):** SWaT precision 0.99; WADI F-measure *"+54% higher than the next best baseline."* Weight: MEDIUM-HIGH, same PA discount as C1.
- **C3 — USAD (Audibert et al.):** *"outperforms all methods on SWaT, MSL, SMAP and WADI"* and *"exceeding by 0.096 the current state-of-the-art"* on average (`https://www.eurecom.fr/publication/6271/download/data-publi-6271_1.pdf`). Weight: MEDIUM, same PA discount.
- **C4 — MTAD-GAT (`https://doi.org/10.48550/arxiv.2009.02040`):** +1/+1/+9pp over best SOTA with hypothesis-test significance. Weight: MEDIUM, same PA discount.
- **C5 — Bearing/manufacturing deep-vs-classical (Q4):** DIDAD AUC 99.5–99.8%, +16.5pp over OC-NN on bearing 3, +2.5–5.1pp over DSVDD/ANOGAN/OC-NN; CNN-Transformer SSL *"substantially outperforms classical feature-engineering approaches"* (F1 0.590 ± 0.040, ROC-AUC 0.878). Adjacent-domain precedent that learned representations clear classical wires on vibration-grade signals. Weight: MEDIUM-LOW (supervised fault-diagnosis, not unsupervised TSAD protocol).

## Supporting-side hits found (disclosed, not suppressed — they are strong)
- S1: Wu & Keogh, "Current Time Series Anomaly Detection Benchmarks are Flawed…" (TKDE 2023, `https://arxiv.org/abs/2009.13807`): majority of Yahoo/Numenta/NASA/OMNI exemplars suffer triviality / unrealistic density / mislabeling / run-to-failure bias — voids C1–C4 as transfer evidence to ANY new battery, twin included.
- S2: "Sign of the Times" KTH thesis (`http://urn.kb.se/resolve?urn=urn%3Anbn%3Ase%3Akth%3Adiva-341879`): deep TSAD performance "misleading," progress "illusory" vs classical.
- S3: Schmidl et al. General Finding 2 + 4 (VERIFIED `https://timeeval.github.io/evaluation-paper/`): supervised not superior; *"no single algorithm achieves perfect scores… no clear winner"* across families; KNN / Sub-LOF among the few robust+effective implementations — and these are threshold-agnostic (PA-proof) results.
- S4: TranAD's own table: MERLIN (parameter-free classical) competitive on univariate sets; LSTM-NDT good on MSL/SMD — classical within striking distance on several sets even PA-on.

## Verdict: NOT FALSIFIED after 5 (protocol-voided contra; gate stands for the twin battery)
Every deep-clears-wire hit (C1–C5) is measured under point-adjustment or supervised-diagnosis protocols the F1 lock explicitly rejects; none isolates a PA-off raw-F1 deep-vs-KNN/PCA delta on caricature-rectangular data. The PA-proof evidence (S3) and the benchmark-validity evidence (S1) both cut toward gate-holds. Devil's-advocate best shot: TranAD's PA-off F1* (0.8012 avg) still leads — but without a same-table classical F1* delta ≥ 3pp it cannot fire H2's tripwire. Gate stands until the M0b read-off; no registry change.
