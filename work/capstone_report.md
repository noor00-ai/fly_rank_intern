# Capstone Report — Ranking Signal Analysis

**Author:** Noor Ul Ain  
**Lane:** Ranking Signal Analysis  
**Repo:** <your GitHub repository link>  
**Date:** 2026-08-05  

> Copy this file to `work/capstone_report.md` and fill it in as you build. Sections 1–8
> mirror the Pass / Needs-Work rubric axes, so nothing here is optional. Sections 0 and 9
> are **paper sections**: your deployed research paper must carry both, and they're here so
> you never rebuild them from memory at ship time.

---

## 0. Abstract

This project investigates which content-related signals are associated with better ranking performance in the FlyRank ML Internship dataset. The analysis uses anonymized content performance data containing ranking, engagement, traffic, and content features. A supervised machine learning classification approach using a Random Forest model was applied to identify patterns between ranking signals and performance outcomes. The model achieved strong predictive performance on a client-grouped evaluation split, showing that multiple signals together can help identify ranking-related patterns. The final output is a ranking signal analysis report that helps editors and ranking teams understand which measurable features should be considered when improving content optimization decisions.

---

# 1. Problem framing

This project supports the decision of understanding which content signals are associated with better ranking performance.

The unit of analysis is an individual content item from the FlyRank ML Internship dataset.

The output of this analysis is a ranking signal report that identifies important features and patterns affecting content performance.

A human editor or ranking team can use these insights to understand which signals should be considered when improving content ranking decisions.

A wrong decision could lead to lower-quality ranking results, reduced search relevance, and inefficient content optimization.

Machine learning and data analysis help because they can identify relationships between multiple ranking signals and performance metrics that may not be obvious through manual analysis.

---

# 2. Data safety

This project uses the FlyRank ML Internship dataset:

`data/raw/content_refresh_anonymized.csv`

The dataset contains anonymized content performance records. Each row represents a content item with associated ranking, engagement, traffic, and content-related features.

Features considered for analysis include:

- search_volume
- competition
- competition_level
- cpc
- content_type
- main_intent
- word_count
- char_count
- impressions_90d
- clicks_90d
- pageviews_90d
- sessions_90d
- users_90d
- engaged_sessions_90d
- content_age_days
- days_since_last_update
- ctr
- avg_position
- engagement_rate
- scroll_rate
- ai_traffic_pct

Excluded columns:

- `content_id` and `client_id` were excluded as model features because they are identifiers. They were only used for grouping and validation purposes and do not represent meaningful ranking signals.

- `trend_direction` was excluded because it is a derived categorical field related to performance changes.

- `trend_pct` was included as a feature because it represents historical trend information available in the dataset. Potential leakage risks were considered during feature selection.

- Tier-based columns such as `age_tier`, `freshness_tier`, `word_count_tier`, `char_count_tier`, `impression_tier`, and `position_tier` were excluded when equivalent numerical features were already available to avoid redundant information.

Leakage risks considered:

- Identifier fields were not used as predictive features.
- Client information was only used for grouped evaluation.
- No client-identifying information was included in the `work/` directory.

All analysis was performed using anonymized data while maintaining data safety requirements.

---

# 3. Baseline

The baseline approach provides a simple reference point before applying machine learning.

The purpose of the baseline is to compare whether a machine learning model can identify ranking-related patterns beyond straightforward comparisons using existing performance signals.

The baseline considers observed ranking and engagement indicators such as:

- avg_position
- ctr
- engagement_rate

The baseline does not learn from historical examples. Instead, it provides an interpretable benchmark that helps evaluate whether the final model provides additional predictive value.

The baseline and machine learning model are evaluated using the same client-grouped test split to ensure a fair comparison.

---

# 4. Model / Analysis

This project uses a supervised machine learning classification approach to analyze which content signals are associated with ranking performance.

The target variable is `target`, which represents the outcome being predicted from historical content performance data.

A Random Forest classifier was selected because it can capture non-linear relationships between multiple ranking signals while also providing feature importance information for interpretation.

The model uses the following features:

- search_volume
- competition
- cpc
- word_count
- impressions_90d
- clicks_90d
- sessions_90d
- days_since_last_update
- ctr
- avg_position
- engagement_rate
- scroll_rate
- trend_pct

The dataset was split using `GroupShuffleSplit` with `client_id` as the grouping variable.

This approach prevents records from the same client appearing in both training and testing sets, reducing the risk of data leakage.

Dataset split:

- Training samples: 12,493
- Testing samples: 5,424

The analysis focuses on identifying relationships between available ranking signals and content performance outcomes rather than claiming to reproduce any external search engine ranking algorithm.

---

# 5. Evaluation

The evaluation uses a client-grouped test split created using `GroupShuffleSplit`.

The grouping strategy was selected because multiple records can belong to the same client. Keeping client groups separated between training and testing provides a more realistic evaluation of how the model performs on unseen content sources.

The Random Forest classifier was evaluated using classification metrics:

- Accuracy
- Precision
- Recall
- F1 Score

Model evaluation results:

| Model | Accuracy | Precision | Recall | F1 Score |
|---|---|---|---|---|
| Random Forest | 1.00 | 1.00 | 1.00 | 1.00 |

The results show that the model was able to classify the target outcomes accurately on the evaluation dataset.

Error analysis should still be considered because high performance metrics can sometimes indicate strong patterns in the dataset, including possible feature relationships that require additional validation.

---

# 6. Interpretation

The model identifies relationships between multiple ranking and engagement signals and the target performance outcome.

Feature importance analysis can be used to understand which signals contribute most to the model's decisions.

Important ranking signals include measurable content characteristics such as:

- search demand indicators
- engagement behaviour
- traffic performance
- ranking position
- content freshness signals

The analysis shows that ranking performance is influenced by multiple combined signals rather than a single feature.

A limitation is that the model identifies associations between signals and outcomes. It does not prove that any individual signal directly causes ranking improvement.

---

# 7. Recommendation

Based on the analysis, ranking teams can use measurable content signals as decision-support indicators when evaluating content performance.

Recommended actions:

1. Monitor engagement-related signals such as CTR and engagement rate when reviewing content quality.
2. Consider ranking position and traffic indicators together instead of relying on a single metric.
3. Use content freshness and update-related signals as supporting information.
4. Apply model insights as guidance for optimization decisions rather than automatic ranking decisions.

Confidence level:

The model provides useful evidence about patterns within the available dataset. However, further validation on additional data and future time periods is required before making operational ranking decisions.

---

# 8. Reproducibility

The project can be reproduced by cloning the repository and running the notebook containing the complete analysis workflow.

Environment requirements:

- Python
- pandas
- numpy
- scikit-learn
- matplotlib

The experiment uses:

- Random state: 42
- GroupShuffleSplit for evaluation
- Client-based grouping using `client_id`

The complete workflow includes:

1. Loading the anonymized dataset.
2. Selecting analysis features.
3. Removing missing values.
4. Creating grouped train/test splits.
5. Training the Random Forest classifier.
6. Evaluating model performance using classification metrics.

All reported results should match a fresh execution of the notebook.

---

# 9. Acknowledgments & data credit

This project was built using the FlyRank ML Internship dataset.

Dataset credit:

Built on the FlyRank ML Internship dataset: https://flyrank.ai

The dataset was used for educational research and analysis purposes while maintaining anonymization and data safety requirements.

---

> **Claims checklist before submitting:** observed / measured / directional / decision-support  
> **Metrics vs. base rate:** report your task's base rate (majority-class %) next to any
> precision@K or accuracy — a high score can just be a high base rate. AUC / lift over
> baseline are the honest discrimination numbers.  
> language everywhere · no causal claims without an experiment or causal design · no
> "predicted Google's algorithm" · no client-identifying details · numbers in this report
> match a fresh re-run.