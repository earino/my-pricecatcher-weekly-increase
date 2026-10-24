<!-- dataset-factory:links -->
**Dataset Factory · v2026.10**

[Dataset website](https://earino.github.io/my-pricecatcher-weekly-increase/versions/v2026.10/) · [GitHub release](https://github.com/earino/my-pricecatcher-weekly-increase/releases/tag/v2026.10) · [Hugging Face](https://huggingface.co/datasets/earino/my-pricecatcher-weekly-increase/tree/v2026.10) · [Catalogue](https://earino.github.io/dataset-factory-site/)

Artifact: 06c5cd6f8a74aef9d1fab048ab88f86e311385e68c0d25416b7ac5c0281a23cf

### Suitability and evaluation requirements

Retained for retrospective model-development research with documented timing limitations. Later evaluation rows expose earlier outcomes; predictions require restricted access.

**Supported uses:** Retrospective train/validation model-development experiments under the documented timing and selection limitations; Final evaluation only under a protocol that isolates holdout data from development and prevents later rows influencing earlier predictions

**Not supported:** Claims of verified real-time or operational forecasting validity; Treating development/validation scores as untouched final-test results; Forecasting scores from unrestricted cross-row access to this artifact

**Evaluation access:** chronological. The labeled eval split is development/validation data. Keep the final holdout outside agent development; prevent later rows from influencing earlier predictions.

**Historical availability:** assumed. **Training-label timing:** unverified. **Vintage:** current snapshot. **Selection:** unverified.

**Cross-row audit:** recovery demonstrated. Independent operator-commissioned audit; predictions frozen before labels were opened; holdouts sealed; two AUC implementations in exact agreement

Structural qualification and baseline scores do not certify operational forecasting validity.

- join (premise, item, T) -> (same pair, T+7 days): the next row's prices are the operands of T's label
- The labelled holdout is presumed to carry the same self-answering structure by construction; it was never opened by the audit and must be treated as compromisable by the same join class.
- Historical feature/label availability at the declared instant is assumed from current snapshots, not verified against release vintages.
- The current scorer passes the complete feature frame to predict_proba; prediction-time isolation is instructed but not mechanically enforced, so any framework score on these files is gameable.
- Observing same-day rows in one current bulk file does not verify historical weekly completeness or revision timing. Missing next-week outcomes restrict the scored population.

**Corrections to the historical documentation:**

- Public site page, Hub card and repository README must state the restricted suitability once rendered; no frozen package bytes change.

Review: 2026-10-24; applies to v2026.10 / `06c5cd6f8a74aef9d1fab048ab88f86e311385e68c0d25416b7ac5c0281a23cf`. Frozen data and tags are unchanged.
<!-- /dataset-factory:links -->

# Malaysian retail price increases, week ahead

A binary prediction task built from Malaysia's PriceCatcher daily price-monitoring panel (KPDN and
DOSM, published through data.gov.my). **Derived research dataset, not an official Malaysian
government product.**

For an item-premise pair with at least three price observations in ISO week W, predict whether the
pair's weekly modal price is **higher in week W+1 than in week W**.

- **Prediction instant:** end of ISO week W (Sunday 23:59 MYT). Every feature is computed from
  observations dated on or before that instant. The publisher's bulk storage carries same-day rows,
  so no publication lag is modelled.
- **Universe:** Premises in the state of **Johor** - the artifact is state-scoped, and other states
  can be released later under the same construction.
- **Rows:** 397,982 train (target weeks 2023-11 .. 2024-12), 280,468 eval (2025), 154,279 holdout
  (2026 through 2026-10-19). Positive rate 14.9% / 17.5% / 20.4%.
- **Eligibility:** at least 3 observations in week W. Pairs whose week W+1 has fewer than 3
  observations have an *unobserved outcome* (143,978 of them) and are counted, not silently excluded.

## Baseline and references

| Predictor | Eval AUC |
| --- | ---: |
| Runner-contract XGBoost (shipped) | **0.7520** |
| Item prior (train-only share of weeks that rose) | 0.5354 |
| Momentum (`delta_1w`) | 0.5001 |
| Reversal (`price_vs_mean4`) | 0.4974 |
| Observation count | 0.4769 |

The baseline clears every naive reference by a wide margin: the signal is real, not a persistence
artefact.

## Files

| Path | Contents |
| --- | --- |
| `public/train.csv` | 397,982 rows |
| `public/eval.csv` | 280,468 rows |
| `private/holdout.csv` | 154,279 rows |
| `meta.json` | task contract, columns, split boundaries |
| `quality.json` | feature documentation, measurement frames, timing declaration |
| `baseline/` | the harness runner, unmodified |
| `code/` | the build that produced this artifact |

The holdout is labelled and ships with the release; keep its labels outside an evaluated agent's
workspace.
