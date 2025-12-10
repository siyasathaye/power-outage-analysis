### The Effects of Severe Weather on Power Outage Duration and Overall Impact
**Authors:** Risa Schloyer & Siya Sathaye  
*(DSC 80 – Fall 2025 Final Project)*

---

## Introduction
In this project, we analyze a dataset of major power outages in the United States which spans outages from January 2000 to July 2016, acquired from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure, at https://engineering.purdue.edu/LASCI/research-data/outages. Along with key information regarding each outage, the dataset includes information about weather conditions, the region and climate in which the outage occurred, and how each outage impacted customers, making it well suited for understanding how severe weather conditions can impact outage severity.

Our project's research question is:

> **How do severe weather events influence the duration and overall impact of major power outages across different regions in the United States?**

Major power outages can result in substantial economic, social, and safety costs. As severe weather events continue to become more and more common, power companies need to understand the correlation between these extreme weather events and the longer, more severe power outages. Understanding this is essential for informing prevention strategies, infrastructure investment, and emergency response planning.

To explore this question, we clean and preprocess the original dataset and focus our analysis on the columns most relevant to outage severity, cause, and weather information. Our working dataset has 1534 rows and we will focus our analysis on the following columns:

| **Column Name**         | **Description**                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------------------ |
| **OUTAGE.DURATION.MIN** | Total duration of the outage in minutes.                                                                     |
| **CUSTOMERS.AFFECTED**  | Number of customers impacted by the outage.                                                                  |
| **CAUSE.CATEGORY**      | Primary cause category assigned to the outage (e.g., severe weather, equipment failure, intentional attack). |
| **ANOMALY.LEVEL**       | Oceanic Niño Index (ONI) value representing El Niño/La Niña conditions for the season.      |
| **CLIMATE.REGION**      | High-level climate classification for the affected area (e.g., humid continental, subtropical).              |
| **CLIMATE.CATEGORY**    | Local climate category associated with the outage location.                      |
| **NERC.REGION**         | The North American Electric Reliability Corporation region where the outage occurred.                              |
| **U.S._STATE**          | The U.S. state in which the outage occurred.                                                                 |
| **YEAR**                | Calendar year in which the outage occurred.                                                                  |
| **MONTH**               | Calendar month in which the outage occurred.                                                                 |
| **TOTAL.PRICE**         | Average monthly electricity price in the U.S. state where outage occured.                    |
| **TOTAL.SALES**         | Total electricity consumption in the U.S. state (megawatt-hour).                                                     |
| **TOTAL.CUSTOMERS**     | Annual number of total customers served in the U.S. state.                                                 |
| **PC.REALGSP.STATE**    | Per capita real gross state product (GSP) in the U.S. state (measured in 2009 chained U.S. dollars).                             |
| **UTIL.REALGSP**        | Real GSP contributed by Utility industry (measured in 2009 chained U.S. dollars)             |
| **POPULATION**          | Population in the U.S. state in a year                                                                      |
| **POPPCT_URBAN**        | Percentage of the state's population living in urban areas.                                                  |
| **POPDEN_URBAN**        | Population density (persons per square mile) in urban areas.                                                 |
| **POPDEN_RURAL**        | Population density in rural areas.                                                                           |
| **AREAPCT_URBAN**       | Percentage of land area classified as urban.                                                                 |



---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
- Describe initial issues with the raw dataset.
- Explain your cleaning steps (handling missing values, fixing datatypes, filtering rows, engineering duration, etc.).
- Include a small table preview of the cleaned dataset (5 rows).

### Univariate Analysis
The distribution of outage durations is extremely right-skewed, with most outages lasting under a few thousand minutes while a small number last tens of thousands of minutes. This indicates that long, severe outages are rare but disproportionately impactful.

**Plot:**  
<iframe
  src="assets/outage_duration_hist.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Bivariate Analysis
Both severe and non-severe outages show highly right-skewed duration distributions, but outages caused by severe weather tend to have longer typical durations, with a higher median and a larger interquartile range (IQR) than outages from other causes. However, the single most extreme outage durations in the dataset are associated with non-severe causes, which produce a few very long events.

**Plot:**  
<iframe
  src="assets/outage_duration_vs_severe_weather.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

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