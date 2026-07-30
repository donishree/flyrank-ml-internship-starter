# Phoenix AI: An Explainable Search Intelligence System for Predicting and Reviving Declining Webpages

- **Author:** C. B. Donishree
- **Lane:** Freestyle
- **Repository:** https://github.com/donishree/flyrank-ml-internship-starter
- **Date:** 30 July 2026

# 1. Problem Framing
Phoenix AI is designed to support proactive SEO decision-making by identifying webpages that are at risk of declining Google Search visibility before significant traffic loss occurs.

**Decision Supported**
Website owners and SEO teams can decide which webpages should be updated, optimized, monitored, or prioritized to prevent future declines in search performance.

**Unit of Analysis**
The unit of analysis is an individual webpage.

**Output**
For every webpage, Phoenix AI generates:
- A risk prediction (High, Medium, or Low risk of decline)
- A webpage health score
- An explanation of the key factors influencing the prediction
- Recommended actions to improve search visibility

**Human Action**
Based on the prediction, users can update page content, improve metadata, optimize SEO elements, refresh outdated information, or continuously monitor high-risk webpages.

**Cost of a Wrong Call**
A false negative may cause a declining webpage to go unnoticed, resulting in reduced search traffic and business opportunities. A false positive may lead to unnecessary optimization effort on a webpage that was not actually at risk. Therefore, the system should balance early detection with reliable predictions.

**Why Machine Learning?**
Search performance is influenced by multiple interacting factors such as impressions, clicks, CTR, content characteristics, and historical trends. Machine learning can recognize complex patterns within these variables that are difficult to identify using simple rule-based methods, enabling earlier and more accurate predictions.

# 2. Data Safety
Phoenix AI uses the FlyRank Warehouse dataset provided as part of the FlyRank ML Internship. The analysis focuses on webpage-level search performance and content-related information that can be used to predict future changes in search visibility.

### Data Used

The project uses publicly safe, pseudonymized search intelligence data, including webpage content information and historical search performance features.

### Columns Excluded

The following columns were deliberately excluded from model training:

- `trend_direction` – Excluded because it directly represents the target outcome and would cause data leakage.
- `trend_pct` – Excluded because it contains future trend information that should not be available during prediction.
- Client identifiers and pseudonymous IDs – Used only for grouping or splitting data when required, never as model features.

### Leakage Prevention

To ensure a fair and realistic model, no feature that directly or indirectly reveals the future outcome is used during training. Label-derived columns are excluded, and the model only learns from information that would have been available at prediction time.

### Privacy and Data Safety

No client-identifying information is included anywhere in the `work/` directory. All analysis is performed using the anonymized FlyRank internship dataset, ensuring privacy and compliance with the internship requirements.

# 3. Baseline
Before training a machine learning model, Phoenix AI establishes a transparent baseline to measure whether the ML approach provides a meaningful improvement.

### Baseline Method

The baseline classifies webpages using a simple rule based on historical search performance. Pages showing a consistent decrease in impressions or clicks over the selected historical time window are marked as "At Risk," while all other pages are considered "Stable."

### Why This Is a Fair Baseline

This rule is simple, easy to understand, and does not require machine learning. It represents the type of manual decision-making that many website owners or SEO analysts currently perform when reviewing search performance reports.

### Model Comparison

The machine learning model will be evaluated on the same dataset, using the same train-test split and evaluation metrics as the baseline. This ensures that any improvement in prediction accuracy comes from the model's ability to learn complex patterns rather than differences in the evaluation process.

### Success Criterion

Phoenix AI will be considered successful if the machine learning model consistently outperforms the baseline in identifying webpages at risk of declining search visibility while maintaining reliable and interpretable predictions.

# 4. Model / Analysis
Phoenix AI uses a supervised machine learning approach to predict whether a webpage is at risk of declining search visibility. Since the objective is to classify webpages into different levels of risk, a classification model is appropriate for this task.

### Method

The initial implementation will use a Random Forest Classifier because it can capture non-linear relationships, handle mixed feature types, and provide feature importance for interpretation. Additional models may be explored for comparison during experimentation.

### Features Used

The model is designed to learn from webpage and search-performance characteristics, including:

- Impressions
- Clicks
- Click-Through Rate (CTR)
- Average Position
- Content Type
- Content Length
- Historical search-performance features
- Other engineered features created from the FlyRank dataset

### Features Excluded

The following fields are intentionally excluded:

- `trend_direction`
- `trend_pct`
- Client identifiers
- Pseudonymous IDs
- Any feature containing future information that would introduce data leakage

### Target Definition

The prediction target is whether a webpage is likely to experience a decline in search visibility during a future evaluation period based only on information available before that decline occurs.

# 5. Evaluation
Phoenix AI will be evaluated using a time-aware evaluation strategy to simulate real-world prediction. The model will be trained using historical webpage data and tested on a later time period to ensure that future information is never used during training.

### Evaluation Strategy

A time-based train-test split is used so that the model predicts future webpage performance from past observations. This reflects how the system would operate in a production environment.

### Evaluation Metrics

The following metrics will be used to evaluate model performance:

- Accuracy
- Precision
- Recall
- F1-Score
- Confusion Matrix

The same evaluation strategy and metrics will be applied to both the baseline method and the machine learning model to ensure a fair comparison.

### Error Analysis

Special attention will be given to false negatives, where Phoenix AI fails to identify a webpage that is actually at risk of declining search visibility. These errors are considered more costly because they may result in missed opportunities to prevent traffic loss. False positives will also be reviewed to understand cases where unnecessary optimization may have been recommended.

# 6. Interpretation
Phoenix AI is designed to identify the patterns that commonly appear before a webpage experiences a decline in search visibility. Rather than only producing a prediction, the system explains which factors contributed most to the risk assessment.

### Expected Insights

The model is expected to identify relationships between historical search-performance metrics and future webpage performance. Features such as impressions, clicks, click-through rate (CTR), average position, and content characteristics are expected to contribute significantly to the prediction.

### Feature Interpretation

Feature importance analysis will be used to explain the model's predictions. This helps users understand why a webpage has been classified as high, medium, or low risk instead of treating the model as a black box.

### Observations

Phoenix AI is intended as a decision-support tool rather than a replacement for human expertise. The recommendations generated by the system should be considered alongside SEO best practices and domain knowledge before taking action.

### Limitations

The quality of predictions depends on the quality and completeness of the available data. External factors such as search engine algorithm updates, seasonal trends, or competitor activity may also influence webpage performance and are not fully captured by the current model.

# 7. Recommendation
Phoenix AI provides ranked recommendations to help website owners and SEO teams take proactive action before a webpage experiences significant declines in search visibility.

### Ranked Recommendations

1. **High Priority** – Immediately review webpages classified as High Risk. Update outdated content, improve metadata, optimize keywords, and verify technical SEO issues.

2. **Medium Priority** – Monitor webpages showing early warning signs. Review content freshness, internal linking, and search performance trends over time.

3. **Low Priority** – Continue regular monitoring of stable webpages while maintaining good SEO practices and content quality.

### Practical Use

A FlyRank editor can use Phoenix AI to prioritize optimization efforts instead of manually reviewing every webpage. This allows teams to focus first on the pages most likely to benefit from intervention.

### Confidence

Phoenix AI is intended to provide decision support rather than certainty. The predictions should be interpreted alongside human expertise and current SEO best practices.

### Limitations

The recommendations are based on historical search-performance data and available webpage features. Unexpected events such as major search engine algorithm updates or sudden changes in user behavior may affect actual webpage performance.

# 8. Reproducibility
The Phoenix AI project is designed to be reproducible from a fresh clone of the repository.

### Project Setup

Clone the repository:

```bash
git clone https://github.com/donishree/flyrank-ml-internship-starter.git
cd flyrank-ml-internship-starter
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Run the complete project pipeline:

```bash
python scripts/run_all.py
```

### Random Seed

A fixed random seed is used wherever applicable to improve reproducibility of model training and evaluation.

### Environment

The project uses the dependencies listed in the repository's `requirements.txt` file. The implementation is intended to run using a standard Python environment with the required machine learning libraries installed.

### Reproducibility Notes

All reported results should be generated using the same dataset version, preprocessing pipeline, evaluation strategy, and model configuration to ensure consistency. Any future modifications to the dataset or feature engineering process should be documented before comparing results.
