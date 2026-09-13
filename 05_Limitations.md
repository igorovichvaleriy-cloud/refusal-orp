# Limitations

## 1. Synthetic evaluation

The executable work uses synthetic data and local prototype logic. It does not test production provider infrastructure.

The 500-case adversarial experiment was designed to challenge a synthetic operation-continuity baseline.

The result was:

Falsified under this synthetic test distribution.

This result must not be interpreted as evidence that production AI safety systems are vulnerable.

## 2. Internal benchmark design

Earlier ORP benchmarks were derived substantially from the project's own invariants and assumptions.

High or 100% synthetic pass rates in those experiments demonstrate internal consistency under modeled conditions, not independent validation or external effectiveness.

## 3. Synthetic ground truth

The 500-case dataset uses synthetic ground truth created within the research process.

The reported precision, recall, and false-positive rate therefore characterize this designed synthetic distribution only.

They do not estimate performance on real harmful operations or production traffic.

## 4. Base-rate limitation

The synthetic dataset contains 103 continuation cases and 397 non-continuation cases.

This continuation prevalence should not be assumed to represent real deployment conditions.

At substantially lower real-world prevalence, precision could be much lower unless the false-positive rate were also substantially lower.

## 5. Evidence scope

Public provider reports establish selected real-world patterns relevant to the research question, but they do not prove the complete AFTER REFUSAL / ORP threat model.

External factual claims require verification against their cited primary sources.

## 6. Thresholds

Prototype weights, thresholds, evidence gates, and quorum values are synthetic experimental parameters.

They are not deployment recommendations and do not represent the internal safeguards of any AI provider.

## 7. Refusal trigger

The original research specification emphasized Refusal Memory.

However, the executable 500-case continuity matcher did not contain a refusal-derived feature.

Therefore, refusal as a primary trigger remains untested and unvalidated by this experiment.

## 8. Attribution

The research intentionally separates capability safety from actor attribution.

Geography, language, IP address, VPN use, topic similarity, code similarity, a previous refusal, or a single classifier output should not by themselves be treated as sufficient evidence of malicious identity or operation continuity.

## 9. Privacy

The proposed privacy-minimization architecture has not undergone formal privacy engineering review or jurisdiction-specific legal review.

## 10. Cross-provider governance

No production cross-provider interoperability implementation has been tested.

The research does not demonstrate that providers currently share or preserve operation-level risk state in the manner proposed here.

## 11. Independent replication

No independent laboratory replication or authorized real-provider adversarial evaluation is included.

## 12. Reproducibility

The synthetic dataset, stored results, baseline parameters, metric definitions, seed, documentation, and integrity hashes have been preserved.

However, the exact original executable generator source used to produce the initial 500-case dataset was not preserved in the same complete form.

Therefore:

Result artifacts are preserved, but full end-to-end reproduction of the original run has not yet been demonstrated.

## 13. Effectiveness

The research has not demonstrated improvement over existing provider systems in:

- precision;
- recall;
- abuse prevention;
- false-positive reduction;
- appeal outcomes;
- privacy;
- operational cost.

## 14. Research status

AFTER REFUSAL / ORP should currently be treated as early research and a local synthetic evaluation, not as a validated production safety architecture.

The strongest current conclusion is deliberately narrow:

The first similarity-based continuity baseline was falsified under this synthetic test distribution.


