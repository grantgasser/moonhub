# Moonhub Assigments

## Problem 1 - Diversity Search
[Code](https://github.com/grantgasser/moonhub/blob/master/Moonhub_Diversity_Search.ipynb)

### Goal
Predict Woman / Not Woman using LinkedIn profile data.

### Approach
### Manual labelling
Sample dataset and labelled ~500 examples via `label_api()`.

**Limitation:** Roughly ~39% of records from the original dataset did not have profile pictures. Our sample _only_ includes profiles with pictures and thus the sample may be slightly biased. For example, there may be a correlation between examples that _don't have profile pictures_ and predicting Woman / Not Woman.

### Modeling
Model 1: Use structured features
- `connection_count`
- `len_skills`
- etc.

*Limited/unimpressive results w/ this approach

Model 2: Use natural language features (OpenAI embeddings)
- `first_name`
- `volunteering`
- `organizations`
- etc.

*Promising results with this approach

Ensemble: average outputs of 1 & 2 for predictions.

**Final:** Ended up just sticking with Model 2 (natural language features, specifically: `first_name`).

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


### Comments
Though `first_name` is quite limiting and we should add other features time-permitting, I think the breadth of OpenAI's training data allows us to get pretty far simply with first names. The [referenced document](https://help.seekout.com/help/360056161191-How-SeekOut-Diversity-Classifiers-Work) boasts precision & recall > .9 by using a bank of the 1 million most common names. Here we have solid results by simply using OpenAI embeddings. 

---------------------------------------------------------------------------

## Problem 2 - Acronym Expansion
[Code](https://github.com/grantgasser/moonhub/blob/master/Moonhub_Acronym_Expansion.ipynb)

[System Design]()

Fun stuff
