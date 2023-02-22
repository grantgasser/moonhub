# Moonhub Assigments

## Problem 1 - Diversity Search
[Code](https://github.com/grantgasser/moonhub/blob/master/Moonhub_Diversity_Search.ipynb)

### Goal
Predict Woman / Not Woman using LinkedIn profile data.

### Approach
### Manual labelling
Labelled ~400 examples via `label_api()`

### Modeling
Model 1: Use structured features
- `connection_count`
- `len_skills`
- etc.

Model 2: Use natural language features
- `first_name`
- `volunteering`
- `organizations`
- etc.

Ensemble: average outpus of 1 & 2 for predictions.

Final: Ended up just sticking with Model 2 (natural language features).

### Results
- Accuracy: 91.75%
- Precision: 1.0
- Recall: 0.71
- F1 Score: 0.83
- ROC AUC: 0.86

*Contingent of course on correct labels.

### Assumptions
- I have correctly labelled Woman/Not Woman by viewing pictures and first names (est. 2-3% error rate) 

### Improvements
- Label more records (Scale AI, Mechanical Turk, manually, etc.)
- Add new natural language features: `volunteering`, `organizations`, etc.
- Given enough examples, train a Transformer
- Ideal but unlikely: get ground truth labels, possibly by self-identification

---------------------------------------------------------------------------

## Problem 2 - Acronym Expansion
[Code](https://github.com/grantgasser/moonhub/blob/master/Moonhub_Acronym_Expansion.ipynb)


