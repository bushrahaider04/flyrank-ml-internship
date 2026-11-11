Capstone: Predicting Search Result Relevance
Abstract
This research investigates whether search result relevance can be predicted using behavioral signals (CTR, dwell time) and semantic similarity. Using a simulated dataset of 10,000 search interactions, we trained a Gradient Boosting Classifier and compared it against a rule-based heuristic baseline. The ML model achieved 74.0% accuracy, significantly outperforming the baseline's 55.2%. These findings suggest that behavioral signals are strong predictors of relevance and can serve as a directional decision-support tool for automated ranking, though further online testing is required.

Research Question: Can we predict search result relevance using behavioral signals?
Decision Supported: Automatically rank search results by predicted relevance to improve user engagement.

Note: To ensure this notebook runs efficiently for demonstration and reproducibility, a simulated dataset representing the production search interactions was generated.

2. Data
Total Rows: 10,000 (simulated)

Train/Test Split: 80% Training (8,000 rows), 20% Test (2,000 rows)

Exclusions (Public-Safe Rationale): User IP addresses, Query timestamps < 1ms duration (bot traffic), Queries with < 10 occurrences (insufficient signal), Results with zero interactions (cold-start items).

3. Methodology
Assumptions: Past user behavior predicts future relevance.
Features Used: Behavioral (ctr_7d, dwell_time_avg), Result-level (result_rank, domain_authority), Cross-features (query_result_similarity), Query-level (query_embedding_dim_1).

Label Definition
Relevance Score (binary) = 1 if: Click-through occurs AND dwell_time > 30 seconds.
Relevance Score = 0 if: No click within 10 seconds OR immediate bounce (< 5 seconds dwell time).

Baseline Model: Rule-based (Domain authority > 50 AND similarity > 0.5).
Performance: 55.2% accuracy on the test set.

Proposed Model: Gradient Boosting Classifier.
Hyperparameters: max_depth = 6, learning_rate = 0.1, n_estimators = 100.

Leakage Checks: Removed user IDs from features. Split data into distinct train/test sets.

4. Results (vs Baseline)
Performance Comparison (Test Set)

Metric	Baseline (Heuristic)	ML Model	Improvement
Accuracy	55.2%	74.0%	+18.8%
F1-Score	0.50	0.78	+0.28
Precision	0.55	0.77	+0.22
Recall	0.45	0.79	+0.34
AUC-ROC	0.65	0.82	+0.17
Feature Importance (Top 5)

Rank	Feature	Importance Score
1	dwell_time_avg	0.39
2	query_result_similarity	0.33
3	ctr_7d	0.14
4	query_embedding_dim_1	0.06
5	domain_authority	0.04
Key Findings: Behavioral signals (dwell time, CTR) are stronger predictors than text features. Semantic similarity between query and result is a top predictor. The ML model significantly outperforms the simple rule-based baseline.
