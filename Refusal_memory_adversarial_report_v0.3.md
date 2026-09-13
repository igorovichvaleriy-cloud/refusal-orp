# OPERATION CONTINUITY — Adversarial Falsification Report v0.3

Date: 2026-09-13
Seed: 20260913
Cases: 500
Status: LOCAL SYNTHETIC ADVERSARIAL TEST — NOT PRODUCTION VALIDATION

## 1. Question

Can deliberately difficult benign or ambiguous synthetic workflows cause a simple continuity-matching baseline to infer operation continuity incorrectly?

Important correction: the surrounding research was originally framed around **Refusal Memory**, but the executable matcher contained no refusal-derived feature. This run therefore evaluates a simple **operation-continuity matching baseline**, not refusal-trigger validity.

## 2. Dataset composition

- true continuation: 103
- benign same-domain: 152
- benign shared-template: 116
- ambiguous: 129

Binary totals:
- continuation: 103
- non-continuation: 397

The class balance is synthetic and intentionally much denser in continuation cases than may occur in real deployment. It is not an estimate of real-world prevalence.

## 3. Baseline

Inputs:
- `artifact_similarity`
- `task_progression`
- `temporal_proximity`
- `behavioral_similarity`
- `independent_corroboration`
- `evidence_current`

Synthetic score:

`0.34 * artifact_similarity + 0.24 * task_progression + 0.18 * temporal_proximity + 0.14 * behavioral_similarity + 0.10 * independent_corroboration`

Link condition:

`score >= 0.58 AND evidence_current == True`

The weights and the `0.58` threshold are toy/synthetic baseline parameters. They are not production recommendations and do not describe the internal system of any AI provider.

## 4. Result

- TP: 88
- FP: 129
- TN: 268
- FN: 15
- Total errors/counterexamples: 144/500

Precision:

`TP / (TP + FP) = 88 / (88 + 129) = 0.4055`

**Precision = 40.55%**

Recall:

`TP / (TP + FN) = 88 / (88 + 15) = 0.8544`

**Recall = 85.44%**

False-positive rate:

`FP / (FP + TN) = 129 / (129 + 268) = 0.3249`

**False-positive rate = 32.49%**

## 5. Interpretation

This specific baseline produced concrete counterexamples and was **falsified under this designed synthetic test distribution**.

The dominant failure mode was **false continuity**: artifact, task-progression, temporal, and behavioral similarity could combine into a continuity decision even when the synthetic ground truth marked the workflow as unrelated.

Narrow conclusion:

> **Similarity accumulation is not sufficient evidence of operation continuity for this baseline under this designed synthetic distribution.**

This result does not falsify Operation Continuity as a broader research concept.

## 6. Base-rate limitation

The 40.6% precision was measured on a synthetic dataset with roughly one continuation case per four non-continuation cases. It should not be interpreted as expected real-world precision; at much lower prevalence, precision would fall sharply unless the false-positive rate were dramatically lower.

No real-world prevalence is asserted.

The 85.4% recall also comes from continuation scenarios generated within the synthetic design. It characterizes this designed distribution only and does not estimate recall on real harmful operations.

## 7. Expiry trade-off

All 15 false negatives in the preserved run occurred when evidence was stale or expired.

This is recorded as a trade-off rather than automatically as a defect:

- shorter-lived risk state can reduce stale or permanent suspicion but can also lose genuine continuity;
- longer-lived risk state may preserve continuity better but can increase stale-suspicion risk.

No universal retention period or threshold is established by this experiment.

## 8. Independent corroboration

Working definition:

> **Independent corroboration is evidence that does not originate from the same similarity function, artifact fingerprint, upstream source, or inference chain that created the original continuity hypothesis.**

Multiple correlated signals from one provenance path should not be counted as multiple independent confirmations.

This is a **working research definition for future testing**, not a result established by the 500-case run and not a validated production standard.

## 9. Specification / implementation divergence

The written v0.1 concept centered **Refusal Memory**.

The executable matcher did not use refusal or any refusal-derived feature.

The mismatch was discovered while attempting to test whether removing refusal would change the executable model. There was no refusal-derived input to remove.

Therefore:

- the 40.6% precision result cannot support or refute refusal as a useful trigger;
- refusal centrality in the executable v0.1 model is not supported;
- refusal-trigger validity remains **UNTESTED / UNVALIDATED**;
- the experiment is better described as an **operation-continuity matching test**.

This is also a methodological finding: a written specification and an executable implementation can remain individually coherent while describing different experiments. Claims should therefore be traced to the actual implementation and stored outputs.

## 10. What this does NOT show

These results do **not**:

- show a vulnerability in any named AI provider;
- measure any production AI system;
- estimate real provider precision, recall, false-positive rate, or prevalence;
- validate the synthetic ground-truth generator;
- establish whether refusal is a useful or useless production trigger;
- show that current labs lack internal continuity mechanisms;
- prove that Operation Risk Continuity as a broader concept is correct;
- validate a production threshold, retention period, or matching method;
- constitute independent replication.

> **These results characterize one synthetic baseline under one designed test distribution. They are not measurements of any production AI system and should not be interpreted as evidence of a vulnerability or real-world failure rate in any provider.**

## 11. Reproducibility limitation

The stored CSV and summary preserve the dataset outputs and analysis results associated with the run, together with the documented baseline parameters and seed.

However, the complete original executable generator source was **not preserved** in `.py` form exactly as used for the initial run. The current `refusal_memory_adversarial_harness_v0.1.py` does not contain that complete source.

Therefore, the stored dataset, outputs, arithmetic, parameters, and documented seed can be audited, but:

> **Full end-to-end reproduction of the original run has not been demonstrated.**

This must not be described as full reproducibility or independent replication.

## 12. AI-assistance disclosure

This research was developed with substantial AI assistance.

The human researcher defined the research direction and safety boundaries, reviewed and challenged the outputs, required corrections when claims or implementation diverged, and chose which hypotheses to retain, reject, narrow, or test.

AI tools were used to help organize research material, draft and revise research language, generate synthetic test material and code, summarize outputs, and identify contradictions and edge cases.

Synthetic results produced within this workflow are not independent validation.

## 13. Next scientific step

The following are **proposed next steps**, not findings from the current 500-case run.

Do not patch the threshold merely to improve the score.

Before testing a revised matcher:

1. define independent corroboration in provenance terms, not as another similarity score;
2. preserve the base-rate warning;
3. evaluate on data not designed around the same synthetic assumptions where possible;
4. keep refusal-trigger validity as a separate, currently unvalidated question;
5. preserve failures and counterexamples rather than optimizing them away after inspection.

## 14. Current status

- similarity-based continuity baseline: **FALSIFIED under this designed synthetic test distribution**
- refusal as central executable trigger: **NOT IMPLEMENTED**
- refusal-trigger validity: **UNTESTED / UNVALIDATED**
- expiry vs continuity: **DESIGN / GOVERNANCE TRADE-OFF**
- production relevance: **UNKNOWN**
- independent validation: **NONE**
- full original-run reproducibility: **NOT DEMONSTRATED**
