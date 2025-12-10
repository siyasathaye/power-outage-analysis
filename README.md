### The Effects of Severe Weather on Power Outage Duration and Overall Impact
**Authors:** Risa Schloyer & Siya Sathaye  
*(DSC 80 – Fall 2025 Final Project)*

---

## Introduction
In this project, we analyze a dataset of major power outages in the United States which spans outages from January 2000 to July 2016, acquired from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure, at https://engineering.purdue.edu/LASCI/research-data/outages. Along with key information regarding each outage, the dataset includes information about weather conditions, the region and climate in which the outage occurred, and how each outage impacted customers, making it well suited for understanding how severe weather conditions can impact outage severity.

Our project's research question is:

> **How do severe weather events influence the duration and overall impact of major power outages across different regions in the United States?**

Major power outages can result in substantial economic, social, and safety costs. As severe weather events continue to become more and more common, power companies need to understand the correlation between these extreme weather events and the longer, more severe power outages. Understanding this is essential for informing prevention strategies, infrastructure investment, and emergency response planning.

To explore this question, we clean and preprocess the original dataset and focus our analysis on the columns most relevant to outage severity, cause, and weather information. Our working dataset has 1534 rows and we will focus on the following columns:

| Column Name | Description |
|-------------|-------------|
| **OUTAGE.DURATION.MIN** | Total duration of each outage in minutes. |
| **CAUSE.CATEGORY** | Categorical label describing the primary cause of the outage. |
| **IS_SEVERE** | A boolean column we created to indicate whether each outage was caused by severe weather. |
| **NERC.REGION** | The North American Electric Reliability Corporation region involved in each outage. |
| **CLIMATE.CATEGORY** | Category describing the climate of the affected area. |
| **CUSTOMERS.AFFECTED** | Number of customers affected by the outage. |

<!-- spacing fix -->

These variables form the foundation of our exploratory data analysis, missingness investigation, hypothesis testing, and predictive modeling. By examining patterns across causes, regions, and weather conditions, our goal is to understand how severe weather influences both the duration and overall impact of major outages.
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