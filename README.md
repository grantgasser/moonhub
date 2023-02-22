# Moonhub Assignment

## Problem 1 - Diversity Search
[Code](https://github.com/grantgasser/moonhub/blob/master/Moonhub_Diversity_Search.ipynb)

NOTE: Apologies for the long outputs, formatting is lost when pushing a notebook to Git. Feel free to scroll right past!

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

Model 2: Use natural language features (**OpenAI embeddings**) and a simple linear model (**Logistic Regression**)
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

[System Design](https://github.com/grantgasser/moonhub/blob/master/Acronym%20Expansion.drawio.pdf)

### Components

#### Initial Data Collection
Here we enter and store the most common job titles. For example we can reason from experience and common sense that "software engineer" is more common than "swe", "software eng", etc. Thus, we initially enter the most common jobs we care about (either manually or by grabbing from the web) like so:

```
{
  most_common_title: [variation1, variation2, ...],
  ...
}
```

For example,
```
{
  "software engineer": ["swe", "software eng", ...],
  ...
}
```

Note in the code there is a slight variation of this where we _also store the embedding_ so we don't have to query the API every time we access the key.
```
{
  "software engineer": [[0.1, -0.2 , ...], ["swe", "software eng", ...]],
  ...
}
```

#### Data Storage
Because our dataset will be small, we are flexible in how and where we store this data. Per this design, a key-value store would be ideal such as Cassandra or we could simply store the mapping in a pickle file.

#### Query
A query comes in string format like so: "nlp scientist"

#### Variation Function 
This is the core functionality of the system which does the following:
1. Performs a lookup from **Data Storage**
2. Checks the cosine similarity of the `title` embedding to existing `most_common_title` embeddings. Time complexity: O(num_jobs)
3. After finding the most similar, it returns the variations! 

Case: if `title` is not in our list of variations, we add it for further use. Thus the knowledge base grows organically with each query.

**Time and Space complexity note:** We expect the dataset to be relatively small given that there are only so many unique jobs that we will care about and only so many variations of that job title. That said, space and time complexity for this functionality can be thought of as O(num_jobs * num_variations_per_job).

#### Variations Output
The set of variations like so:
```
get_variations("mle") => 

{'machine learning eng',
  'machine learning engineer',
  'ml eng',
  'ml engineer'
 }
  ```

### Limitations / Tradeoffs / Assumptions
- Does not handle completely new job titles that are unrelated to existing ones well (i.e. if we ask for "truck driver")
- Likely a good idea to abstract the mapping into a class with methods (OOP)
