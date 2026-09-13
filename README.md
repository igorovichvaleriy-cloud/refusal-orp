AFTER REFUSAL / ORP

Status: Early research / local synthetic evaluation
Result: Falsified under this synthetic test distribution.

This work does not demonstrate a vulnerability in any production AI system.

Goal

AFTER REFUSAL / Operation Risk Protocol (ORP) explores whether relevant operation-level risk context can be preserved across related interactions without treating similarity, identity, or a previous refusal as automatic proof that two interactions belong to the same operation.

The first executable baseline tested operation-continuity matching on synthetic data.

It did not test any production system.

Synthetic Test

The baseline was evaluated on 500 synthetic cases designed to challenge and potentially falsify it.

Dataset:

* 103 true-continuation cases
* 152 benign same-domain cases
* 116 benign shared-template cases
* 129 ambiguous cases

Results:

* TP: 88
* FP: 129
* TN: 268
* FN: 15

Precision

Precision = TP / (TP + FP)

88 / (88 + 129) = 40.6%

Among cases classified by the baseline as continuations, 40.6% matched the synthetic ground truth.

False-positive rate

FPR = FP / (FP + TN)

129 / (129 + 268) = 32.5%

Among synthetic non-continuation cases, 32.5% were incorrectly classified as continuations.

These metrics describe only this baseline on this synthetic test distribution. They are not estimates of production AI-system performance.

Baseline

The synthetic baseline used:

* artifact similarity
* task progression
* temporal proximity
* behavioral similarity
* independent corroboration
* evidence freshness

Synthetic score:

0.34 × artifact_similarity
+ 0.24 × task_progression
+ 0.18 × temporal_proximity
+ 0.14 × behavioral_similarity
+ 0.10 × independent_corroboration

A case was linked when:

score >= 0.58 AND evidence_current == True

The weights and 0.58 threshold are experimental parameters only. They are not recommended production thresholds and do not represent the internal safeguards of any AI provider.

What failed

The baseline produced 129 false positives.

The main falsification result is:

Similarity accumulation is not sufficient evidence of operation continuity.

This means the first baseline should not be treated as a viable production design.

An additional methodological finding was that the original specification emphasized Refusal Memory, while the executable baseline contained no refusal-derived feature.

Therefore, refusal as a primary trigger remains untested and unvalidated.

Limitations

This experiment used synthetic data and local execution.

There has been:

* no testing of a production AI provider;
* no independent laboratory replication;
* no measurement of real-world prevalence;
* no validation of the synthetic ground-truth generator;
* no demonstration of real-world precision, recall, or false-positive rates.

The synthetic dataset also contained a much higher continuation prevalence than might exist in a real deployment. Therefore, the reported 40.6% precision must not be interpreted as expected real-world precision.

Reproducibility

The preserved research artifacts include the synthetic dataset, stored results, baseline parameters, metric definitions, seed (20260913), documentation, and SHA-256 hashes.

However, the exact original executable generator source was not preserved in the same form used for the initial run.

Therefore:

Result artifacts are preserved, but full end-to-end reproduction of the original run has not yet been demonstrated.

This limitation is intentional to disclose rather than hide.

Safety Boundaries

This research is limited to synthetic, local, or explicitly authorized environments.

The repository should not contain:

* API keys or passwords;
* credentials;
* private or personal data;
* real user accounts;
* secret logs;
* non-public detector details;
* operational bypass recipes;
* step-by-step instructions for defeating real safeguards.

Safe synthetic examples, aggregate results, methodology, falsification criteria, and non-operational defensive code may be included after review.

The research may model how a safety architecture can fail in order to improve it, but it must not turn those findings into operational instructions for defeating real systems.

Current Conclusion

The current result is deliberately narrow:

The first similarity-based continuity baseline was falsified under this synthetic test distribution.

It does not show that “AI safety systems are vulnerable.”

It shows that this particular proposed baseline was not reliable enough under the synthetic adversarial conditions used to test it.