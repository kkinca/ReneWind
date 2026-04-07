# Wind Turbine Failure Prediction

A neural network project for predictive maintenance that identifies wind turbines at risk of generator failure using sensor data.

## Project Overview

Unexpected generator failures in wind turbines are costly and disruptive. Missed failures can lead to emergency replacements, extended downtime, and higher maintenance costs.

This project builds a machine learning solution to predict generator failures early so maintenance teams can inspect or repair turbines before breakdown occurs. The goal is to shift operations from reactive maintenance to proactive intervention.

## Business Problem

In this use case, the cost of missing a generator failure is much higher than the cost of an unnecessary inspection.

That means the model should prioritize:
- detecting as many true failures as possible
- minimizing missed failures
- using threshold tuning to manage inspection volume

Because the business tradeoff is asymmetric, recall is more important than overall accuracy.

## Dataset

The project uses anonymized wind turbine sensor data to predict whether a generator failure will occur.

Key dataset characteristics:
- highly imbalanced target variable
- numeric, sensor-derived features
- no categorical variables or timestamps
- wide variation in feature scales and distributions
- weak signal from individual features, suggesting failures depend on multi-sensor interactions

## Exploratory Data Analysis

Key findings from EDA:
- generator failures are a small minority of observations, confirming severe class imbalance
- no single sensor feature clearly separates failures from non-failures
- missing values were minimal
- pairwise correlations were generally low to moderate
- failure prediction likely depends on complex nonlinear relationships across multiple variables

These findings supported the choice of neural networks and a recall-focused evaluation strategy.

## Data Preprocessing

The following preprocessing steps were applied:
- checked for duplicates
- handled a small number of missing values using median imputation
- avoided aggressive outlier removal to preserve potential failure-related signals
- split training data into training and validation sets using stratified sampling
- applied preprocessing consistently across training, validation, and test sets to avoid data leakage

## Modeling Approach

This project compared **seven neural network models** with variations in:
- number of hidden layers
- layer size
- optimizer choice
- dropout regularization
- class weighting

The experiments were designed to improve failure detection while maintaining generalization performance on new data.

## Evaluation Metric

The primary metric for model selection was:

- **Recall for the failure class**

Why recall mattered most:
- false negatives mean missed generator failures
- missed failures lead to downtime and costly replacements
- false positives only trigger inspections, which are cheaper and operationally manageable

Precision was considered secondarily and improved through threshold tuning.

## Model Comparison

Seven neural network models were evaluated.

Key observations:
- simpler models had lower recall
- deeper models with regularization performed better
- class weighting consistently improved failure detection
- Model 6 achieved the strongest validation recall with stable performance

### Best Validation Results

- **Best model:** Model 6
- **Validation Recall:** 0.882883
- **Validation Precision:** 0.692580
- **Validation F1 Score:** 0.776238

## Final Model Selection

Model 6 was selected as the final model because it:
- delivered the highest recall on the validation set
- handled class imbalance effectively
- showed stable performance without clear signs of overfitting
- aligned best with the business objective of early failure detection

### Final Model Characteristics

- multi-layer neural network
- dropout regularization
- class weighting to emphasize rare but costly failure cases

## Threshold Tuning

Threshold tuning was used to manage the tradeoff between:
- catching failures early
- limiting unnecessary inspections

### Selected Operating Threshold

- **Threshold = 0.35**

Why this threshold was selected:
- failure recall remained near 90%
- precision improved meaningfully compared with lower thresholds
- inspection volume stayed more manageable
- unnecessary inspections were reduced without increasing missed failures

## Test Performance

The final model was evaluated on a separate test set to estimate real-world performance.

### Key Test Results

- **Recall on test set:** approximately 90%
- **False Negatives:** 40
- **False Positives:** 110

This means the model successfully identified most failure cases while keeping inspection overhead within a reasonable range.

## Business Impact

This project demonstrates how machine learning can support predictive maintenance in industrial operations.

Potential business value includes:
- fewer emergency generator replacements
- reduced turbine downtime
- lower maintenance costs
- earlier intervention on high-risk equipment
- a shift from reactive to proactive maintenance planning

## Recommendations

Recommended next steps:
- deploy Model 6 as the production candidate
- use a probability threshold of 0.35 to trigger inspections
- integrate predictions into maintenance workflows as an early-warning signal
- monitor field performance and recalibrate thresholds as operating conditions change
- continue improving the model with additional data and failure history

## Tools and Techniques

- Python
- Neural Networks
- Machine Learning (ML)
- Predictive Maintenance
- Classification Modeling
- Threshold Tuning
- Class Imbalance Handling
- Recall Optimization
- Jupyter Notebooks

## Repository Contents

Suggested files for this repo:
- `README.md`
- `wind_turbine_failure_prediction_notebook.ipynb`
- `Wind_Turbine_Failure_Presentation.pdf`

## Key Takeaway

When the cost of missing a failure is much higher than the cost of a false alarm, model evaluation should follow the business objective. This project shows how recall-focused modeling and threshold tuning can make machine learning more practical and valuable for predictive maintenance.
