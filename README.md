### The Effects of Severe Weather on Power Outage Duration and Overall Impact
**Authors:** Risa Schloyer & Siya Sathaye  
*(DSC 80 – Fall 2025 Final Project)*

---

## Introduction
- What dataset we used (Power Outage dataset).
- What our main question is.
- Why this question matters.
- How many rows are in the cleaned dataset.
- The columns relevant to our question and what they represent.

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
- Describe initial issues with the raw dataset.
- Explain your cleaning steps (handling missing values, fixing datatypes, filtering rows, engineering duration, etc.).
- Include a small table preview of the cleaned dataset (5 rows).

### Univariate Analysis
Briefly describe trends observed in one key column.

**Plot:**  
<iframe src="assets/univariate.html" width="800" height="600" frameborder="0"></iframe>

### Bivariate Analysis
Describe one meaningful relationship relevant to the question.

**Plot:**  
<iframe src="assets/bivariate.html" width="800" height="600" frameborder="0"></iframe>

### Interesting Grouped Table
Include a Markdown table generated from a groupby or pivot.

---

## Assessment of Missingness

### NMAR Discussion
Explain which column might be NMAR and why.  
Explain what additional data would make it MAR.

### Missingness Dependency Test
Describe your permutation test on missingness.

**Visualization:**  
<iframe src="assets/missingness.html" width="800" height="600" frameborder="0"></iframe>

State two findings:
- One column where missingness **depends** on another variable  
- One where missingness **does not** depend on another variable

---

## Hypothesis Testing
- State null and alternative hypotheses in words.
- Define the test statistic.
- Explain how the null distribution was generated.
- Report the observed statistic, p-value, and conclusion in plain English.
- (Optional) Include plot of null distribution.

---

## Framing a Prediction Problem
- Define the response variable.
- Whether it's classification or regression.
- Features that are allowed at prediction time.
- Metric you will use and why it’s appropriate.

---

## Baseline Model
- Describe the pipeline (model + preprocessing).
- List features used and encoding choices.
- Report performance on the test set.
- Brief interpretation of results.

---

## Final Model
- Discuss additional features engineered and why they help.
- Explain model selection (e.g., Random Forest vs Linear Regression).
- Describe hyperparameter tuning.
- Report final performance and compare to baseline.
- Briefly discuss limitations.

---

## Fairness Analysis
- Define Group X and Group Y.
- State fairness hypothesis being tested.
- Show metric for each group.
- Describe permutation test setup.
- Report p-value.
- Conclude whether there is evidence of unfairness.

**(Optional Plot)**  
<iframe src="assets/fairness.html" width="800" height="600" frameborder="0"></iframe>

---

## References
List any external sources, documentation, or citations used.

---