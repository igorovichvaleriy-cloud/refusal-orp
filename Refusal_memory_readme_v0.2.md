# OPERATION CONTINUITY / REFUSAL MEMORY — README v0.2

Date: 2026-09-13
Status: LOCAL SYNTHETIC FALSIFICATION WORK — NOT PRODUCTION VALIDATION

## 1. Research question

The original document framed the work around **Refusal Memory**: whether bounded risk context after a refusal should persist across later related activity.

The first executable experiment did **not** operationalize refusal. Its matcher used only continuity-style features. Therefore, the executable result should be interpreted as a test of a simple **operation-continuity matching baseline**, not as a test of refusal-trigger validity.

Current narrow question:

> Can a simple similarity-based continuity matcher distinguish continuation of the same synthetic operation from unrelated benign or ambiguous work?

## 2. Dataset

Total cases: **500**

Class composition:
- true continuation: **103**
- benign same-domain: **152**
- benign shared-template: **116**
- ambiguous: **129**

Binary ground truth:
- continuation: **103**
- non-continuation: **397**

Approximate class ratio in this synthetic dataset:
- 1 continuation case per **3.85** non-continuation cases.

This balance was designed for adversarial evaluation. It is **not** an estimate of real-world prevalence.

## 3. Baseline

Stored baseline inputs:

- `artifact_similarity`
- `task_progression`
- `temporal_proximity`
- `behavioral_similarity`
- `independent_corroboration`
- `evidence_current`

Synthetic score:

`0.34 * artifact_similarity`
`+ 0.24 * task_progression`
`+ 0.18 * temporal_proximity`
`+ 0.14 * behavioral_similarity`
`+ 0.10 * independent_corroboration`

Stored baseline link condition:

> `score >= 0.58` and `evidence_current == True`

The weights and `0.58` threshold are synthetic baseline parameters only. They are **not** recommended production thresholds and do not represent any provider's internal system.

Important implementation finding:

> The executable matcher contains **no refusal-derived feature or refusal flag**.

The specification described Refusal Memory as central, but the executable test did not test refusal as a trigger. This is a specification/implementation divergence and is itself a research finding.

## 4. Confusion matrix

- True positives (TP): **88**
- False positives (FP): **129**
- True negatives (TN): **268**
- False negatives (FN): **15**

Total: **500**

## 5. Metric definitions

### Precision

`Precision = TP / (TP + FP)`

For this run:

`88 / (88 + 129) = 0.4055`

**Precision = 40.55%**

Interpretation: among cases this baseline labeled as continuation, 40.55% were continuation according to the synthetic ground truth.

### Recall / sensitivity

`Recall = TP / (TP + FN)`

For this run:

`88 / (88 + 15) = 0.8544`

**Recall = 85.44%**

Important limitation: the continuation cases were generated within our own synthetic design. This recall describes performance on this designed distribution only. It does **not** estimate recall on real harmful operations.

### False-positive rate

`FPR = FP / (FP + TN)`

For this run:

`129 / (129 + 268) = 0.3249`

**False-positive rate = 32.49%**

Interpretation: among synthetic non-continuation cases, 32.49% were incorrectly linked by this baseline.

## 6. Base-rate warning

The **40.6% precision was measured on a synthetic dataset with roughly one continuation case per four non-continuation cases. It should not be interpreted as expected real-world precision; at much lower prevalence, precision would fall sharply unless the false-positive rate were dramatically lower.**

No real-world prevalence is asserted here.

Illustrative scenario analysis, holding this synthetic sensitivity and FPR fixed:
- prevalence 1 / 1,000 -> expected precision ≈ 0.26%
- prevalence 1 / 10,000 -> expected precision ≈ 0.026%

These are mathematical illustrations, not real-world estimates, because the real prevalence and real-world operating characteristics are unknown.

## 7. Main falsification result

The baseline produced:

- **129 false positives**
- **15 false negatives**
- **144 total counterexamples / errors**

The dominant failure mode was **false continuity**.

Legitimate or ambiguous synthetic workflows could accumulate enough similarity across artifact, task progression, time, and behavior to be linked even when the synthetic ground truth marked them as unrelated.

Narrow conclusion:

> **Similarity accumulation is not sufficient evidence of operation continuity.**

This conclusion applies only to this baseline under this synthetic test distribution.

## 8. Expiry trade-off

All 15 false negatives in the stored run occurred when evidence was no longer current.

This is recorded as a trade-off, not automatically as a bug:

- shorter-lived risk state can reduce stale or permanent suspicion;
- shorter-lived risk state can also lose genuine continuity;
- longer-lived state may preserve continuity better;
- longer-lived state can increase the risk of stale suspicion.

No universal retention period or threshold is claimed.

## 9. Independent corroboration

Working definition:

> Independent corroboration is evidence that does not originate from the same similarity function, artifact fingerprint, upstream source, or inference chain that created the original continuity hypothesis.

Multiple correlated signals from one provenance path should not be counted as multiple independent confirmations.

This definition is a research rule to be tested, not a validated production standard.

## 10. Refusal trigger status

**Refusal centrality in the v0.1 executable model: NOT SUPPORTED.**

Reason: refusal was never an input to the executable matcher.

**Refusal trigger validity: UNTESTED / UNVALIDATED.**

This experiment cannot establish whether refusal correlates with dangerous operation continuation.

## 11. Specification vs implementation

A key finding from this work is methodological:

> The document described one system, while the executable test implemented another.

The written specification centered Refusal Memory. The actual code/data path tested similarity-based operation continuity without a refusal feature.

This mismatch was only discovered when the research attempted to remove/refute the refusal assumption.

This matters especially in AI-assisted research: coherent prose and coherent code can still describe different experiments. Claims must therefore be traced back to the exact implementation and stored outputs.

## 12. Reproducibility note

Stored research artifacts include:
- `refusal_memory_adversarial_cases_v0.1.csv`
- `refusal_memory_adversarial_summary_v0.1.json`
- `REFUSAL_MEMORY_ADVERSARIAL_REPORT_v0.1.md`
- `REFUSAL_MEMORY_v0.1.md`
- `REFUSAL_ABLATION_RESULT_v0.1.md`

The presence of an artifact in this list does not mean that it belongs in the first public repository version.

The full original executable generator source was **not preserved** in `.py` form exactly as used for the initial run. The preserved dataset, stored outputs, arithmetic, baseline parameters, and documented seed can therefore be audited, but they do not establish full reproducibility of the original execution.

**Full end-to-end reproduction of the original run has not been demonstrated.**

This limitation must not be described as full reproducibility or independent replication.

## 13. What this does NOT show

These results characterize **one synthetic baseline under one designed test distribution**.

They do **not**:
- measure any production AI system;
- demonstrate a vulnerability in Anthropic, OpenAI, Google DeepMind, xAI, or another provider;
- estimate real provider precision, recall, or false-positive rate;
- establish real-world prevalence of dangerous continuation;
- validate the synthetic ground-truth generator;
- prove refusal is a useful or useless production trigger;
- show that labs lack internal continuity mechanisms;
- justify cross-provider identity tracking;
- validate a production threshold, retention period, or matching method;
- constitute independent replication.

> **These results characterize one synthetic baseline under one designed test distribution. They are not measurements of any production AI system and should not be interpreted as evidence of a vulnerability or real-world failure rate in any provider.**

## 14. Current status

- first similarity-based continuity baseline: **FALSIFIED under its synthetic test distribution**
- refusal as central executable trigger: **NOT IMPLEMENTED**
- refusal trigger validity: **UNTESTED**
- expiry vs continuity: **REAL DESIGN / GOVERNANCE TRADE-OFF**
- production relevance: **UNKNOWN**
- independent validation: **NONE**

