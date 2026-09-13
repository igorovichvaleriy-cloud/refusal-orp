# AFTER REFUSAL / ORP — Research Note v0.2

Date: 2026-09-13
Status: Early research / local synthetic evaluation / not evidence of a production vulnerability

## 1. Title

**AFTER REFUSAL / ORP**

ORP = Operation Risk Protocol.

## 2. Goal

To test, using a local synthetic model, whether a simple continuity-matching baseline can distinguish continuation of the same synthetic operation from similar but independent synthetic work, without treating a change in system boundary as automatic evidence of either continuity or a clean slate.

The initial idea was framed around Refusal Memory. During implementation review, it was established that the first executable baseline actually tested operation-continuity matching and did not contain a refusal-derived feature. Therefore, this experiment does not test the validity of refusal as a trigger.

## 3. Status

This is **early research** and a **local synthetic evaluation**.

The results:
- are not evidence of a production vulnerability;
- do not measure OpenAI, Anthropic, or any other provider's systems;
- are not an independent laboratory replication;
- do not establish real-world production precision, recall, or false-positive rates.

First similarity-based continuity baseline: **FALSIFIED under its synthetic test distribution**.

Refusal-trigger validity: **UNTESTED / UNVALIDATED**.

## 4. Test

A local adversarial test was conducted on **500 synthetic cases**, deliberately constructed so that the baseline had an opportunity to fail.

Composition:
- true continuation: 103;
- benign same-domain: 152;
- benign shared-template: 116;
- ambiguous: 129.

Binary ground truth:
- continuation: 103;
- non-continuation: 397.

Confusion matrix:
- TP = 88
- FP = 129
- TN = 268
- FN = 15

The purpose of the test was not to confirm the baseline, but to try to falsify it.

## 5. Results

**Precision: 40.6%**
**False-positive rate: 32.5%**

Additionally:
**Recall: 85.4%**

Narrow result:

> Similarity accumulation is not sufficient evidence of operation continuity.

The baseline produced 144 errors/counterexamples across 500 cases, of which 129 were false positives.

Important base-rate caveat:

> The 40.6% precision was measured on a synthetic dataset with roughly one continuation case per four non-continuation cases. It should not be interpreted as expected real-world precision; at much lower prevalence, precision would fall sharply unless the false-positive rate were dramatically lower.

The 85.4% recall was also obtained on continuation scenarios created within this synthetic design. It is not an estimate of recall on real harmful operations.

## 6. Exact Metric Definitions

### Precision

`Precision = TP / (TP + FP)`

In this test:

`88 / (88 + 129) = 0.4055 ≈ 40.6%`

This means that among cases the baseline labeled as continuation, 40.6% matched the synthetic ground truth as true continuation.

Precision characterizes only this baseline on this synthetic distribution.

### False-positive rate

`FPR = FP / (FP + TN)`

In this test:

`129 / (129 + 268) = 0.3249 ≈ 32.5%`

This means that among 397 synthetic non-continuation cases, the baseline incorrectly linked 129, or approximately 32.5%.

### Recall / sensitivity

`Recall = TP / (TP + FN)`

In this test:

`88 / (88 + 15) = 0.8544 ≈ 85.4%`

This recall characterizes only the synthetic continuation cases created within this experiment.

## 7. Baseline

The baseline was intentionally a simple synthetic continuity model, not a model of any specific provider's production system.

Inputs:
- `artifact_similarity`
- `task_progression`
- `temporal_proximity`
- `behavioral_similarity`
- `independent_corroboration`
- `evidence_current`

Synthetic weighted score:

`0.34 × artifact_similarity`
`+ 0.24 × task_progression`
`+ 0.18 × temporal_proximity`
`+ 0.14 × behavioral_similarity`
`+ 0.10 × independent_corroboration`

Binary link rule:

`score >= 0.58 AND evidence_current == True`

The weights and the 0.58 threshold are parameters of a toy/synthetic baseline. They are not recommended production thresholds and do not describe the internal system of any AI provider.

### Specification / implementation divergence

The initial document described **Refusal Memory** as the central idea.

The executable matcher did not contain `refusal` or any refusal-derived feature.

Therefore, this test actually evaluated **operation-continuity matching**, not whether refusal is a valid trigger.

This is a separate methodological finding: the specification and implementation had diverged, and the divergence was discovered while attempting to falsify the researcher's own assumption.

## 8. Method

1. 500 synthetic cases were created with predefined synthetic ground truth.
2. Cases included continuation, benign same-domain, benign shared-template, and ambiguous workflows.
3. Synthetic feature values were assigned to each case.
4. The baseline calculated a weighted continuity score.
5. If the score was at least 0.58 and the evidence was current, the baseline returned `linked`; otherwise it returned `not linked`.
6. The decision was compared with the synthetic ground truth.
7. TP, FP, TN, FN, precision, recall, and FPR were calculated.
8. Particular attention was paid to counterexamples, not only aggregate scores.

The expectation was falsifiable:

> If a simple combination of similarity signals is sufficient to reliably establish continuity, it should survive deliberately difficult benign and ambiguous cases.

The baseline did not.

### Expiry trade-off

All 15 false negatives in the preserved run occurred when the evidence was stale/expired.

This is recorded as a trade-off:
- shorter retention reduces the risk of permanent/stale suspicion but may lose genuine continuity;
- longer retention preserves continuity better but increases the risk of stale suspicion.

This is not automatically a “bug” that can be removed by choosing one correct threshold.

### Independent corroboration — working definition

> Independent corroboration is evidence that does not originate from the same similarity function, artifact fingerprint, upstream source, or inference chain that created the original continuity hypothesis.

This is a testable research rule, not a validated production standard.

## 9. Limitations

- The data are synthetic.
- Execution was local.
- The dataset and some assumptions were created within the research process itself.
- The synthetic class balance is not an estimate of real-world prevalence.
- There is no access to production logs, classifiers, matching systems, or ground truth from AI providers.
- There is no independent laboratory replication.
- The 40.6% precision and 32.5% FPR characterize only this specific baseline on this specific synthetic distribution.
- The 85.4% recall is not an estimate of real-world recall.
- Refusal-trigger validity remains untested.
- The synthetic generator may reflect assumptions introduced by the researcher and the AI-assisted workflow.

> These results characterize one synthetic baseline under one designed test distribution. They are not measurements of any production AI system and should not be interpreted as evidence of a vulnerability or real-world failure rate in any provider.

## 10. Reproducibility

For auditing and partial verification of the preserved result, the following are available/required:

- the 500-case dataset;
- synthetic ground truth;
- baseline inputs;
- exact weights;
- threshold 0.58;
- the `evidence_current` rule;
- seed `20260913`;
- confusion matrix;
- metric definitions;
- document versions;
- run date;
- SHA-256 hashes.

Main preserved research artifacts:
- `REFUSAL_MEMORY_v0.1.md`
- `REFUSAL_MEMORY_README_v0.1.md`
- `REFUSAL_MEMORY_ADVERSARIAL_REPORT_v0.1.md`
- `refusal_memory_adversarial_cases_v0.1.csv`
- `refusal_memory_adversarial_summary_v0.1.json`
- `REFUSAL_ABLATION_RESULT_v0.1.md`
- `REFUSAL_MEMORY_v0.1_SHA256.txt`

The presence of an artifact in this list does not mean that it is included in the first public repository version.

Known limitation:

The full original executable generator source was not preserved in `.py` form exactly as used for the initial run. Therefore, the preserved dataset, stored outputs, arithmetic, baseline parameters, and documented seed can be audited, but they do not support a claim of full reproducibility of the original execution.

**Full end-to-end reproduction of the original run has not been demonstrated.**

This limitation must not be described as full reproducibility or independent replication.

Status:

**Result artifacts preserved — full original-run reproducibility not demonstrated.**

## 11. Safety

The research is conducted only in:
- synthetic environments;
- local test environments;
- or explicitly authorized environments.

Safe to preserve/publish:
- safe code;
- synthetic examples;
- synthetic datasets;
- formulas;
- aggregate metrics;
- methodology;
- falsification criteria;
- non-operational architectural findings.

Do not include:
- API keys;
- passwords;
- credentials;
- private/personal data;
- real user accounts;
- full private archives;
- secret logs;
- non-public detector features;
- operational bypass recipes;
- step-by-step instructions for defeating real safeguards.

Safety rule:

> The research may model how a safety architecture can fail in order to improve it, but it must not turn those findings into operational instructions for defeating real systems.

---

## Current research interpretation

Three findings are currently separated:

1. **Similarity-based matching baseline: FALSIFIED under the synthetic test distribution.**
2. **Refusal as a primary/central trigger: UNTESTED and not implemented in the executable baseline.**
3. **Expiry vs continuity: a genuine safety/governance trade-off.**

No broader production claim is made.

