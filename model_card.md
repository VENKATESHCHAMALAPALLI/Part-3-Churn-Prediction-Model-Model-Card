# Model Card — Churn Prediction

## Model summary
- **Name:** 60-day churn risk model
- **Artifact:** `model.pkl`
- **Purpose:** Predict which customers are likely to churn within 60 days after the snapshot date so retention and marketing teams can prioritize outreach.
- **Type:** Scikit-learn pipeline with a tree-based gradient boosting classifier.

## What it predicts
- The model produces a churn probability score for each customer.
- A probability above the selected threshold is treated as a churn-risk recommendation.
- The final business threshold used for this model is **0.31**.

## Intended business use
- Prioritize customers for retention offers, service outreach, or loyalty interventions.
- Combine the churn score with customer value, campaign cost, and channel suitability before taking action.
- Use the score as a prioritization signal, not as the sole decision rule.

## Data and features
- Training source: `rfm_modeling_snapshot.csv`
- Target: `churn_next_60d` (whether the customer churned in the following 60 days).
- Features used include:
  - customer activity and purchase behavior: `recency_days`, `frequency_180d`, `monetary_180d`
  - engagement signals: `sessions_30d`, `campaign_clicks_30d`, `product_views_30d`
  - support interaction: `ticket_count_90d`
  - customer lifecycle indicators: `days_since_signup`, `age_group`, `marketing_consent`
- The model is trained on a single snapshot cohort and does not use future behavior beyond the snapshot cutoff.

## Performance summary
- **Test accuracy:** 0.7768
- **Test precision:** 0.7268
- **Test recall:** 0.8869
- **Test F1-score:** 0.7989
- **ROC-AUC:** 0.8635
- **PR-AUC:** 0.8439
- **Selected threshold:** 0.31
- **Test confusion matrix:** [[112, 56], [19, 149]]

## Business implications
- The model is tuned to catch most customers who will churn, which helps the retention team avoid missed opportunities.
- Because recall is high, there will still be false positives: some customers flagged as churn risk will remain active.
- False positives imply extra retention cost; false negatives imply missed chance to save customers.

## Key driver signals
- `recency_days`: longer inactivity is the strongest churn signal.
- `monetary_180d`: higher recent spend is associated with lower churn risk.
- `frequency_180d`: frequent purchases indicate stronger retention.
- `sessions_30d` and `campaign_clicks_30d`: recent digital engagement helps separate active customers from churn risk.
- `age_group` and `marketing_consent`: customer segment signals that affect engagement and retention patterns.

## Recommended use guidance
- Use this model to rank customers for retention outreach.
- Apply business filters such as expected lifetime value, offer eligibility, and campaign capacity before spending budget.
- Update the model before applying it to customers acquired after the original snapshot date.

## Limitations
- Designed for short-term 60-day churn only.
- Not validated for customers who joined after the snapshot date.
- May underperform when customer behavior or product mix changes materially.
- Does not account for future planned campaigns or promotions after the snapshot.

## Maintenance
- Track actual churn outcomes against predicted probabilities.
- Monitor drift in principal features like recency, spend, and engagement.
- Rebuild the model on new snapshot cohorts regularly.
- Review precision and recall tradeoffs if retention costs or campaign goals change.

## When not to use
- Do not use this model for long-term customer lifetime forecasting.
- Do not use the score alone to deny access, remove customers from offers, or make one-time eligibility decisions.
- Do not apply the existing model to new customer segments without retraining.
