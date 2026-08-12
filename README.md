# Gift Card Fraud Detection — Rule-Based Analysis

!(docs/cover_0.png)

A data challenge on gift-card checkout fraud. This project analyses ~98K checkout
events to characterise what drives fraud and to design an **interpretable,
cost-aware rule set** that a risk team could actually operate — one that captures
meaningful fraud volume at a **positive net business value**, rather than
maximising recall at any cost.

> The dataset is not included in this repository.

## Problem

Gift cards are a recurring target for payment fraud: they are near-cash, quick to
liquidate, and hard to recover once redeemed. For a risk team, the practical
question is not only *which* transactions look fraudulent, but **how strongly to
act** on each one — blocking a legitimate customer carries a real cost, so a
detection rule is only worthwhile if the value of the fraud it prevents outweighs
the cost of its false positives.

This project analyses **98,645 checkout events** (fraud rate **0.56%**) to:

1. Characterise the fraud population and its main drivers.
2. Define a KPI that reflects the business trade-off, not just statistical accuracy.
3. Derive a small, interpretable rule set mapped to concrete operational actions.

## Key Results

!(docs/executive_summary.png)

- **Fraud is concentrated in newly-created accounts** — median account age of
  **31 days** for fraudulent events vs. **1,265 days** for legitimate ones —
  amplified by geography and banking signals.
- A rule set of **4 rules captures ~30% of fraud** at a **positive net business value**.
- Proposed KPI: **Net Value of Detection** — the value of fraud prevented, minus the
  cost of false positives — so rules are judged on business impact rather than raw
  detection rate.
- **Rules are differentiated by action** (block → alert → monitor) according to
  their precision and stability, instead of applying a single blunt threshold to
  every case.

## Approach

## Approach

1. **Exploratory analysis** — profile fraud across account age, geography, banking
   attributes, and transaction features to identify the strongest signals.
2. **KPI definition** — formalise *Net Value of Detection* to weigh fraud prevented
   against false-positive cost. The cost of a false positive is estimated from a
   managerial standpoint, combining analyst review time, escalation overhead, and
   the expected churn from wrongly blocking a legitimate customer.
3. **Rule derivation** — fit a shallow decision tree to surface a small set of
   interpretable candidate rules, evaluated on precision, coverage, and net value.
4. **Stability analysis** — measure rule stability as the persistence of each rule
   under varying resampling size (sensitivity to resampling size), to avoid
   recommending rules that only hold on this specific sample.
5. **Action mapping** — map each retained rule to an operational action
   (block / alert / monitor) based on its precision and stability.
   
## Data

The dataset is **not included** (provided as part of a data challenge).

- **Rows:** 98,645 checkout events
- **Target:** binary fraud flag (fraud rate 0.56%)
- **Key features:** account age, geography, banking attributes, [altre feature chiave]

See (data/README.md) for the full schema.

## Project Structure

├── fraud-detection-analysis.ipynb # main analysis: EDA → KPI → rules → stability
├── data/
│ └── README.md # schema and data dictionary (no raw data)
├── docs/
│ ├── cover_0.png # cover
│ └── executive_summary.png # summary of findings
├── requirements.txt
├── LICENSE
└── README.md

The full analysis, with all output cells executed, lives in
[`fraud-detection-analysis.ipynb`](fraud-detection-analysis.ipynb). GitHub renders
it natively, so the results are readable without running anything.

## Reproduce

```bash
git clone https://github.com/<username>/gift-card-fraud-detection.git
cd gift-card-fraud-detection
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab   # open fraud-detection-analysis.ipynb
```

Python [3.11]. The notebook expects the dataset under `data/` — see [Data](#data).

## Decisions & Trade-offs

- **Net Value of Detection over raw recall/precision.** A model that maximises
  recall can still be a net loss if it blocks too many legitimate customers.
  Expressing the objective in monetary terms keeps the recommendation aligned with
  what a risk team actually optimises. The trade-off is sensitivity to the assumed
  false-positive cost;
- **Interpretable rules over a black-box classifier.** Decision-tree rules can be
  read, challenged, and shipped by a risk team, and mapped to differentiated actions.
  This gives up some predictive power in exchange for operability and auditability —
  the right trade-off in an anti-fraud context where decisions must be explainable.
- **Differentiated actions (block / alert / monitor) over a single threshold.** Not
  every fraud signal justifies blocking a customer. Mapping each rule to an action by
  its precision and stability limits customer friction while still acting decisively
  on the strongest signals.

## Limitations

- The rule set captures **~30% of fraud**, by design — it is a high-value,
  low-friction first layer, not a complete detection system.
- *Net Value of Detection* depends on assumed unit costs and values; the ranking of
  rules shifts if those assumptions change.
- Findings come from a single point-in-time sample; fraud patterns drift, so rules
  would need periodic recalibration.
- The analysis identifies associations, not causal effects.

## Future Work

- Recalibrate thresholds on a rolling window to handle concept drift.
- Benchmark the rule set against a supervised model to quantify the net-value gap.
- Enrich the feature set with velocity, device, and network signals.

## License

Released under the MIT License — see [LICENSE].
