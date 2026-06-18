# Error Analysis — Churn Model

## Overview
The final test set evaluation identified 75 classification errors:
- 56 false positives (predicted churn, but the customer did not churn)
- 19 false negatives (predicted no churn, but the customer churned)

### Business risk summary
- **False positives** can lead to wasted retention spend on customers who would have remained active without outreach.
- **False negatives** can cause missed intervention opportunities, lost revenue, and worse customer experience if churners are not prioritized for retention.

## Key test metrics
- Test accuracy: 0.7768
- Test precision: 0.7268
- Test recall: 0.8869
- Test F1-score: 0.7989
- Selected threshold: 0.31
- Test confusion matrix: [[112, 56], [19, 149]]

## False positive examples
The following customers were flagged as churn risk but remained active during the 60-day evaluation window. These examples illustrate how the model can overreact to low-engagement signals.

| customer_id | probability | recency_days | frequency_180d | monetary_180d | ticket_count_90d | sessions_30d | campaign_clicks_30d |
|---|---|---|---|---|---|---|---|
| CUST01370 | 0.9765 | 161 | 2 | 1246.04 | 0 | 2 | 0 |
| CUST01325 | 0.9571 | 186 | 0 | 0.00 | 0 | 1 | 0 |
| CUST00437 | 0.9448 | 151 | 1 | 729.22 | 0 | 0 | 0 |
| CUST01246 | 0.9386 | 262 | 0 | 0.00 | 0 | 1 | 3 |
| CUST01405 | 0.9030 | 140 | 1 | 1013.03 | 0 | 2 | 0 |
| CUST01614 | 0.8981 | 103 | 2 | 1352.11 | 0 | 4 | 0 |

### Business interpretation for false positives
- These customers appear inactive or low-value based on recency and recent spend, but they did not churn in the 60-day horizon.
- If retention outreach is costly, these cases increase the risk of inefficient campaign ROI.
- Recommended action: use the churn score as a first filter and layer in customer lifetime value, campaign cost, or likelihood-to-respond scores before allocating offers.

## False negative examples
These customers churned even though the model predicted they would stay. They often had recent activity or strong spend that made the model less likely to label them as churn risks.

| customer_id | probability | recency_days | frequency_180d | monetary_180d | ticket_count_90d | sessions_30d | campaign_clicks_30d |
|---|---|---|---|---|---|---|---|
| CUST00184 | 0.0327 | 14 | 3 | 2456.91 | 0 | 6 | 0 |
| CUST01990 | 0.0615 | 59 | 4 | 3877.77 | 0 | 11 | 1 |
| CUST01655 | 0.0809 | 13 | 2 | 1358.99 | 0 | 2 | 0 |
| CUST00592 | 0.0809 | 20 | 1 | 627.36 | 0 | 3 | 0 |
| CUST01303 | 0.0910 | 20 | 1 | 844.74 | 1 | 3 | 0 |
| CUST02072 | 0.1050 | 35 | 7 | 4340.19 | 0 | 4 | 0 |

### Business interpretation for false negatives
- These customers had signals consistent with engagement or spend, so the model underweighted their churn risk.
- Missing these customers means the retention team may fail to reach customers who were truly at risk.
- Recommended action: monitor false negatives over time and consider adding behavioral or campaign response features to improve detection.

## Risk tradeoffs
- The chosen threshold of 0.31 favors recall to reduce missed churners, which is appropriate when the cost of missing churn is higher than the cost of extra outreach.
- The model still makes a moderate number of false positive predictions, so business rules should filter the score before final outreach decisions.

## Conclusion
This error analysis shows the model is effective at catching the majority of churners but still produces identifiable business risks on both sides:
- false positives increase outreach cost,
- false negatives increase the chance of lost customers.

The retention team should treat the score as a prioritization signal and combine it with value-based decision rules for operational use.
