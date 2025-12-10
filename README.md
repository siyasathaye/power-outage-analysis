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
| `'OUTAGE.DURATION.MIN'` | Total duration of the outage in minutes.                                                                     |
| `'CUSTOMERS.AFFECTED'`  | Number of customers impacted by the outage.                                                                  |
| `'CAUSE.CATEGORY'`      | Primary cause category assigned to the outage (e.g., severe weather, equipment failure, intentional attack). |
| `'ANOMALY.LEVEL'`       | Oceanic Niño Index (ONI) value representing El Niño/La Niña conditions for the season.      |
| `'CLIMATE.REGION'`      | High-level climate classification for the affected area (e.g., humid continental, subtropical).              |
| `'CLIMATE.CATEGORY'`    | Local climate category associated with the outage location.                      |
| `'NERC.REGION'`         | The North American Electric Reliability Corporation region where the outage occurred.                              |
| `'U.S._STATE'`          | The U.S. state in which the outage occurred.                                                                 |
| `'YEAR'`                | Calendar year in which the outage occurred.                                                                  |
| `'MONTH'`               | Calendar month in which the outage occurred.                                                                 |
| `'TOTAL.PRICE'`         | Average monthly electricity price in the U.S. state where outage occured.                    |
| `'TOTAL.SALES'`         | Total electricity consumption in the U.S. state (megawatt-hour).                                                     |
| `'TOTAL.CUSTOMERS'`     | Annual number of total customers served in the U.S. state.                                                 |
| `'PC.REALGSP.STATE'`    | Per capita real gross state product (GSP) in the U.S. state (measured in 2009 chained U.S. dollars).                             |
| `'UTIL.REALGSP'`        | Real GSP contributed by Utility industry (measured in 2009 chained U.S. dollars)             |
| `'POPULATION'`          | Population in the U.S. state in a year                                                                      |
| `'POPPCT_URBAN'`        | Percentage of the state's population living in urban areas.                                                  |
| `'POPDEN_URBAN'`        | Population density (persons per square mile) in urban areas.                                                 |
| `'POPDEN_RURAL'`        | Population density in rural areas.                                                                           |
| `'AREAPCT_URBAN'`       | Percentage of land area classified as urban.                                                                 |



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
  height="600"
  frameborder="0"
></iframe>
### Bivariate Analysis
Both severe and non-severe outages show highly right-skewed duration distributions, but outages caused by severe weather tend to have longer typical durations, with a higher median and a larger interquartile range (IQR) than outages from other causes. However, the single most extreme outage durations in the dataset are associated with non-severe causes, which produce a few very long events.

**Plot:**  
<iframe
  src="assets/outage_duration_vs_severe_weather.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Grouped Table
The table below summarizes outage duration statistics by cause category. Severe weather stands out as both **the most frequent cause of major outages** and one of the causes with a **higher median duration**, indicating that weather-driven outages are not only common but also relatively long-lasting.

| Cause Category               | Mean Duration (min) | Median Duration (min) | Count |
|-----------------------------|----------------------|------------------------|-------|
| equipment failure           | 1816.91              | 221.0                  | 55    |
| fuel supply emergency       | 13484.03             | 3960.0                 | 38    |
| intentional attack          | 429.98               | 56.0                   | 403   |
| islanding                   | 200.55               | 77.5                   | 44    |
| public appeal               | 1468.45              | 455.0                  | 69    |
| severe weather              | 3883.99              | 2460.0                 | 744   |
| system operability disruption | 728.87             | 215.0                  | 123   |

---

## Assessment of Missingness

### NMAR Discussion
I believe the CUSTOMERS.AFFECTED column is NMAR. The likelihood that this value is missing may depend on whether the number of customers actually affected is very large. Power companies may fail to collect customer counts for very small outages (because they are considered insignificant). 

To make this missingness MAR... NOT FINISHED

### Missingness Dependency Test
We focused on the missingness of CUSTOMERS.AFFECTED (how many customers were impacted by an outage). We wanted to see whether missing values in this column depend on other variables in the dataset.

##### Does missingness depend on outage cause?

To explore whether the missingness of CUSTOMERS.AFFECTED depends on CAUSE.CATEGORY, we first compared missingness proportions among every different outage cause.

Several patterns stand out. For example, intentional attack and fuel supply emergency outages have a noticeably higher proportion of missing customer counts, while categories like severe weather tend to have more complete data. This suggested that missingness may not be random but in fact related to certain causes.

<iframe
  src="assets/missing_by_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe> 

Next, we actually tested whether the missingness of CUSTOMERS.AFFECTED depends on CAUSE.CATEGORY using a permutation test. We encoded each cause category as a numeric code and computed our test statistic: the difference in the mean encoded cause category between rows where CUSTOMERS.AFFECTED is missing and rows where it is not.

We calculated an observed test statistic of (~-1.57) and then we generated 5,000 permutations of the missingness labels to simulate a null distribution where missingness is independent of cause category. The histogram below shows this null distribution, with the observed statistic marked by a red vertical line.

Our observed value lies far outside the range of permuted statistics, producing a p-value of 0.0. This means none of the 5,000 shuffled datasets produced a difference as extreme as the real one. Therefore, we have strong evidence that missingness in CUSTOMERS.AFFECTED does depend on outage cause and is not random. 

<iframe
  src="assets/missingness_dep_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
##### Does missingness depend on U.S. state?

To investigate whether the missingness of CUSTOMERS.AFFECTED depends on U.S._STATE, we again used CA_MISSING as an indicator for whether the value is missing. We then compared missingness proportions among every different state label we had.

The plot below shows, for each state, the proportion of outages in which CUSTOMERS.AFFECTED is missing (blue) or not missing (purple). If state were related to missingness, we would expect certain states to have consistently higher missing proportions than others.

However, the proportions vary without a clear pattern or consistent separation between the “missing” and “not missing” groups. No particular state stands out as having a disproportionately high share of missing values.

<iframe
  src="assets/customers_missing_by_state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next, we tested this relationship using a permutation test. We again encoded U.S. states as numeric state codes, calculated the difference in mean state code between missing and non-missing observations, and compared this observed statistic to a null distribution created using 5,000 random label shuffles.

The observed difference was approximately 0.53, and the null distribution (shown below) is centered near zero. The observed statistic falls comfortably within the null distribution, corresponding to a permutation p-value of about 0.52.

Because the p-value is large, we do not have evidence that the missingness of CUSTOMERS.AFFECTED depends on U.S._STATE. In other words, any variation in missingness across states appears consistent with random chance rather than a systematic pattern.

<iframe
  src="assets/missingness_state_null.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
---

## Hypothesis Testing
**Research question.**  
Do outages marked as *severe* last longer, on average, than outages that are *not* severe?

### Hypotheses

- **Null hypothesis (H₀):**  
  The average outage duration is the same for severe and non-severe outages.  
  \[
  \mu_{\text{severe}} = \mu_{\text{non-severe}}
  \]

- **Alternative hypothesis (Hₐ):**  
  Severe outages have a **longer** average duration than non-severe outages.  
  \[
  \mu_{\text{severe}} > \mu_{\text{non-severe}}
  \]

This is a **one-sided** test because I am specifically interested in whether severe outages last *longer*, not just whether the means are different.

### Test statistic and significance level

I used the **difference in sample means** as my test statistic:

\[
T = \text{mean duration for severe outages} - \text{mean duration for non-severe outages}.
\]

From the data, the observed value of this statistic was

\[
T_{\text{obs}} \approx 2538 \text{ minutes}.
\]

I used a significance level of **α = 0.05**.

### Permutation test

To approximate the null distribution of \(T\), I ran a **permutation test**:

1. Kept the outage durations fixed.
2. Randomly shuffled the `"IS_SEVERE"` labels to break any association between severity and duration.
3. For each shuffle, recomputed
   \[
   T^{(b)} = \text{mean duration (labelled severe)} - \text{mean duration (labelled non-severe)}.
   \]
4. Repeated this **N = 5,000** times to build the null distribution of \(T\).

The resulting null distribution of \(T\) is centered near 0 (what we would expect if severity and duration were unrelated). None of the 5,000 permuted statistics were as large as the observed value \(T_{\text{obs}}\).

This gives an empirical one-sided p-value of

\[
p \approx \frac{0 + 1}{5000 + 1} \approx 0.0002 \quad (\text{effectively } p < 0.001).
\]

### Conclusion

Because the p-value is far below our significance level of 0.05, we **reject the null hypothesis**.

There is **strong statistical evidence** in this dataset that outages marked as *severe* last **longer on average** than outages that are not severe.

However, this is an **observational** dataset, not a randomized experiment. Therefore, we **cannot prove** that severity *causes* longer outages—we can only say that severity is **strongly associated** with longer outage duration in this data.

### Visualization

The figure below shows the **null distribution** of the difference in mean outage duration from the permutation test (blue bars), with the **observed statistic** \(T_{\text{obs}}\) drawn as a vertical red line. The observed value lies far in the right tail of the null distribution, which is consistent with the very small p-value.

<iframe
  src="assets/weather_null.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

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