# Understanding the Causes of U.S. Power Outages
**Authors:** Risa Schloyer & Siya Sathaye  
*(DSC 80 – Fall 2025 Final Project)*

---

## Introduction
In this project, we analyze a dataset of major power outages in the United States which spans outages from January 2000 to July 2016, acquired from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure, at https://engineering.purdue.edu/LASCI/research-data/outages. Along with key information regarding each outage, the dataset includes information about weather conditions, the region and climate in which the outage occurred, and how each outage impacted customers, making it well suited for understanding the different factors which can impact outage severity.

Our project's research question is:

> **What factors and characteristics contribute to long-lasting power outages, and can we predict whether an outage will last more than 24 hours using information available at the time the outage begins?**

Major, long-lasting power outages can result in substantial economic, social, and safety costs. Understanding what causes these severe outages is essential for power companies so they're better able to inform prevention strategies, infrastructure investment, and emergency response planning.

To explore this question, we clean and preprocess the original dataset and focus our analysis on the columns most relevant to outage severity, cause, and relevant characteristics of each outage. Our working dataset has 1534 rows and we will focus our analysis on the following columns:

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

We started by removing irrelevant or empty columns (such as the OBS and columns that contained only missing values). 

Next, we combined each date time pair (OUTAGE.START.DATE + OUTAGE.START.TIME, and the same for restoration time and date) into singular columns containing the time and date, so that the dataset was less complex and more succinct. We then dropped the original date/time columns since their information was preserved and they were redundant.

We also converted multiple columns such as YEAR, MONTH, ANOMALY.LEVEL, OUTAGE.DURATION, and CUSTOMERS.AFFECTED to numeric types incase they were not already, so they could be used reliably in calculations. We also renamed OUTAGE.DURATION to OUTAGE.DURATION.MIN to make the unit explicit, as that was not initially clear to us.

To focus the analysis on variables relevant to our project only, we kept a select subset of columns (listed above) and dropped all columns we didn't feel were relevant to our project. Finally, we created an IS_SEVERE column which flags outages caused by severe weather, as we intended to do some exploration of the differences between outages caused by severe weather and those not.

The first few rows of the cleaned DataFrame are shown below, with a few columns selected for display.

|   YEAR |   MONTH | CAUSE.CATEGORY     | U.S._STATE   | NERC.REGION   |
|-------:|--------:|:-------------------|:-------------|:--------------|
|   2011 |       7 | severe weather     | Minnesota    | MRO           |
|   2014 |       5 | intentional attack | Minnesota    | MRO           |
|   2010 |      10 | severe weather     | Minnesota    | MRO           |
|   2012 |       6 | severe weather     | Minnesota    | MRO           |
|   2015 |       7 | severe weather     | Minnesota    | MRO           |

### Univariate Analysis
We wanted to investigate the distribution of outage duration to get a better understanding of how common extremely long outages were. The distribution of outage durations is extremely right-skewed, with most outages lasting under a few thousand minutes while a small number last tens of thousands of minutes. This indicates that long, severe outages are rare but disproportionately impactful.

<iframe
  src="assets/outage_duration_hist.html"
  width="800"
  height="400"
  frameborder="0"
  style="display:block;"
></iframe>


### Bivariate Analysis
We also wanted to explore how outages caused by severe weather differed in duration from outages caused by other causes, because we hypothesized that severe weather would likely be the cause of many of the more serious outages. Both severe and non-severe outages show highly right-skewed duration distributions, but outages caused by severe weather tend to have longer typical durations, with a higher median and a larger interquartile range (IQR) than outages from other causes. However, the single most extreme outage durations in the dataset are associated with non-severe causes, which produce a few very long events.

<iframe
  src="assets/outage_duration_vs_severe_weather.html"
  width="800"
  height="400"
  frameborder="0"
  style="display:block;"
></iframe>


### Grouped Table
The table below summarizes outage duration statistics by cause category. Severe weather stands out as both **the most frequent cause of outages** and one of the causes with a **higher median duration**, indicating that weather-driven outages are not only common but also relatively long-lasting.

| CAUSE.CATEGORY                |     mean |   median |   count |
|:------------------------------|---------:|---------:|--------:|
| equipment failure             |  1816.91 |    221   |      55 |
| fuel supply emergency         | 13484    |   3960   |      38 |
| intentional attack            |   429.98 |     56   |     403 |
| islanding                     |   200.55 |     77.5 |      44 |
| public appeal                 |  1468.45 |    455   |      69 |
| severe weather                |  3883.99 |   2460   |     744 |
| system operability disruption |   728.87 |    215   |     123 |

---

## Assessment of Missingness

### NMAR Discussion
One column I believe to be NMAR is `'OUTAGE.DURATION.MIN'`. I believe this column could be NMAR because the missingness may be due to the fact that power companies may not want to report excessively long outage durations, as it could imply slow response time. As a result, companies may be less inclined to report outage duration when the true value is extremely high, thus making these missing values NMAR.

To determine whether `'OUTAGE.DURATION.MIN'` is actually MAR, I would want to collect information about each power company’s typical response times and determine whether companies which tend to have slower response times typically have missing outage durations. If outages occured in places where certain companies managed electricity, then the missingness could be explained by an observed variable, which would mean it is MAR rather than NMAR.

### Missingness Dependency Test
We focused on the missingness of `'CUSTOMERS.AFFECTED'` (how many customers were impacted by an outage). We wanted to see whether missing values in this column depend on other variables in the dataset.

##### Does missingness depend on outage cause?

To explore whether the missingness of `'CUSTOMERS.AFFECTED'` depends on `'CAUSE.CATEGORY'`, we first compared missingness proportions among every different outage cause.

Several patterns stand out. For example, intentional attack and fuel supply emergency outages have a noticeably higher proportion of missing customer counts, while categories like severe weather tend to have more complete data. This suggested that missingness may not be random but in fact related to certain causes.

<iframe
  src="assets/missing_by_cause.html"
  width="800"
  height="400"
  frameborder="0"
  style="display:block;"
></iframe> 


Next, we actually tested whether the missingness of `'CUSTOMERS.AFFECTED'` depends on `'CAUSE.CATEGORY'` using a permutation test. We encoded each cause category as a numeric code and computed our test statistic: the difference in the mean encoded cause category between rows where `'CUSTOMERS.AFFECTED'` is missing and rows where it is not.

We calculated an observed test statistic of (~-1.57) and then we generated 5,000 permutations of the missingness labels to simulate a null distribution where missingness is independent of cause category. The histogram below shows this null distribution, with the observed statistic marked by a red vertical line.

Our observed value lies far outside the range of permuted statistics, producing a p-value of 0.0. This means none of the 5,000 shuffled datasets produced a difference as extreme as the real one. Therefore, we have strong evidence that missingness in `'CUSTOMERS.AFFECTED'` does depend on outage cause and is not random. 

<iframe
  src="assets/missingness_dep_cause.html"
  width="800"
  height="400"
  frameborder="0"
  style="display:block;"
></iframe>


##### Does missingness depend on U.S. state?

To investigate whether the missingness of `'CUSTOMERS.AFFECTED'` depends on `'U.S._STATE'`, we again used `'CA_MISSING'` as an indicator for whether the value is missing. We then compared missingness proportions among every different state label we had.

The plot below shows, for each state, the proportion of outages in which `'CUSTOMERS.AFFECTED'` is missing (blue) or not missing (purple). If state were related to missingness, we would expect certain states to have consistently higher missing proportions than others.

However, the proportions vary without a clear pattern or consistent separation between the “missing” and “not missing” groups. No particular state stands out as having a disproportionately high share of missing values.

<iframe
  src="assets/customers_missing_by_state.html"
  width="800"
  height="400"
  frameborder="0"
  style="display:block;"
></iframe>


Next, we tested this relationship using a permutation test. We again encoded U.S. states as numeric state codes, calculated the difference in mean state code between missing and non-missing observations, and compared this observed statistic to a null distribution created using 5,000 random label shuffles.

The observed difference was approximately 0.53, and the null distribution (shown below) is centered near zero. The observed statistic falls comfortably within the null distribution, corresponding to a permutation p-value of about 0.52.

Because the p-value is large, we do not have evidence that the missingness of `'CUSTOMERS.AFFECTED'` depends on U.S._STATE. In other words, any variation in missingness across states appears consistent with random chance rather than a systematic pattern.

<iframe
  src="assets/missingness_state_null.html"
  width="800"
  height="400"
  frameborder="0"
  style="display:block;"
></iframe>
---

## Hypothesis Testing
#### **Research question:**  
Do outages caused by severe weather last longer, on average, than outages that are caused by reasons other than severe weather?

### Hypotheses

- **Null hypothesis (H₀):**  
  The average outage duration is the same for severe weather and non-severe  weather outages  
  (mean duration for severe weather outages = mean duration for non-severe weather outages).

- **Alternative hypothesis (Hₐ):**  
  Severe weather outages have a **longer** average duration than non-severe weather outages  
  (mean duration for severe weather outages > mean duration for non-severe weather outages).

We do a one sided test because we are specifically interested in whether severe weather outages last **longer**, not just whether the means are different.

### Test statistic and significance level

We used the **difference in sample means** as our test statistic:

> test statistic = (mean duration for severe weather outages) − (mean duration for non-severe weather outages)

From the data, the observed value of this statistic was approximately **2,538 minutes**.

We used a **significance level of α = 0.05**, the standard choice which balances the risk of false positives with the ability to detect meaningful differences.

### Permutation test

To approximate the null distribution of this test statistic, we ran a **permutation test**:

1. Kept the outage durations fixed.
2. Randomly shuffled the `'IS_SEVERE'` labels to break any association between severity and duration.
3. For each shuffle, recomputed the difference in means:  
   (mean duration for outages labelled severe) − (mean duration for outages labelled non-severe).
4. Repeated this **N = 5,000** times to build the null distribution of the test statistic.

From these 5,000 permutations, **none** of the simulated test statistics were as large as the observed statistic.  
The empirical p-value returned by the simulation was **0.0**, meaning that in all 5,000 shuffles, the difference in means was never as extreme as the observed value.


The figure below shows the **null distribution** (blue bars) with the **observed statistic** shown as a vertical red line. The observed value lies far in the right tail, which matches the extremely small p-value.

<iframe
  src="assets/weather_null.html"
  width="800"
  height="400"
  frameborder="0"
  style="display:block;"
></iframe>


### Conclusion

Because the p-value is far below our significance level of 0.05, we **reject the null hypothesis**.

There is **strong statistical evidence** in this dataset that outages caused by severe weather last **longer on average** than outages that are not caused by severe weather.

However, this is an **observational** dataset, not a randomized experiment. Therefore, we **cannot prove** that severe weather *causes* longer outages, we can only say that severe weather is **strongly associated** with longer outage duration in this data.


---

## Framing a Prediction Problem
Our prediction problem is:

> **Can we predict whether a power outage will last more than 24 hours (1440 minutes) using information available at the start of the outage?**

### Type of prediction problem

This is a **binary classification** problem.

We define:

- **1 = long outage**: outage duration > 1440 minutes (severe outage)  
- **0 = short outage**: outage duration ≤ 1440 minutes (non-severe outage)

### Response variable

To capture this, we create a new binary variable:

- **Response variable:** `'LONG_OUTAGE'`  
  - `'LONG_OUTAGE'` = 1 if `'OUTAGE.DURATION.MIN'` > 1440  
  - `'LONG_OUTAGE'` = 0 otherwise  

This response is meaningful for power companies because being able to predict whether an outage is likely to exceed 24 hours helps with planning repairs, communicating with customers, and allocating resources to those affected.

### Features used (information at time of prediction)

We will only use features that would be known **at the start of the outage**, when a prediction would realistically be made. These include:

- `MONTH`  
- `YEAR`  
- `CLIMATE.CATEGORY`  
- `CLIMATE.REGION`  
- `NERC.REGION`  
- `ANOMALY.LEVEL`  
- `TOTAL.PRICE`  
- `TOTAL.SALES`  
- `TOTAL.CUSTOMERS`  
- `PC.REALGSP.STATE`  
- `UTIL.REALGSP`  
- `POPULATION`  
- `POPPCT_URBAN`  
- `POPDEN_URBAN`  
- `POPDEN_RURAL`  
- `AREAPCT_URBAN`


### Evaluation metric and justification

We evaluate our classifier using **F1-score**.

The classes are likely **imbalanced** (long, severe outages are less common than short outages), so plain accuracy could be misleading: a model that almost always predicts “short outage” could still have high accuracy but be useless for identifying truly long outages.

F1-score is a good choice because:

- It combines **precision** and **recall** into a single number.  
- It especially penalizes **false negatives** (cases where we incorrectly predict an outage will be short when it actually becomes long), which are important to avoid in this context.  
- It is commonly used for **imbalanced binary classification** problems like this one.

Because of these reasons, F1-score is more informative than accuracy for this prediction problem.

---

## Baseline Model
For our baseline model, we use a **Logistic Regression** classifier to predict whether an outage lasts more than 24 hours. Logistic Regression is a simple, interpretable linear model, which makes it a good baseline before trying more complex models.

### Features in the baseline model

Our baseline model uses four features:

- `'YEAR'` — numerical (quantitative)  
- `'ANOMALY.LEVEL'` — numerical (quantitative)  
- `'CLIMATE.CATEGORY'` — categorical (nominal)  
- `'NERC.REGION'` — categorical (nominal)

**Note on `'CLIMATE.CATEGORY'`:**  
Although “Cold”, “Normal”, and “Warm” come from numeric ONI thresholds, the dataset only gives these broad categories and not the actual numeric values or exact distances between them. Because the spacing between categories isn’t defined, we treated `'CLIMATE.CATEGORY'` as a nominal variable and one-hot encoded it. This avoids assuming a linear or evenly spaced relationship that may not actually exist.

### Encodings and preprocessing

Since the model contains both numeric and categorical variables, we apply the following preprocessing steps inside a single `sklearn` Pipeline:

- **Numeric features** (`'YEAR'`, `'ANOMALY.LEVEL'`):  
  - Missing values imputed with the **median**

- **Categorical features** (`'CLIMATE.CATEGORY'`, `'NERC.REGION'`):  
  - Missing values imputed with the **most frequent** category  
  - Encoded using **One-Hot Encoding** to convert categories into numerical indicator columns  
  - `handle_unknown="ignore"` avoids issues with unseen categories during testing

Using a `ColumnTransformer` ensures each type of feature receives the correct preprocessing automatically.

### Baseline model performance

After training the baseline model on the training data and evaluating it on the held-out test set, we obtain:

- **F1-score:** **0.426**

### Is this model “good”?

Overall, the baseline model performs **moderately**, but **not well**:

- An F1-score of ~0.43 indicates the model struggles to correctly identify long outages.  
- This is expected: the relationship between these features and outage duration is likely complex and nonlinear, and Logistic Regression may not capture those patterns.  
- The baseline serves as a reasonable **starting point**, but leaves substantial room for improvement.

In the next step (Final Model), we will explore a more flexible model that may better capture interactions and nonlinearities in the data.

---

## Final Model

To improve upon our baseline model, we use a **Decision Tree Classifier** to predict whether an outage becomes a long outage (lasting more than 24 hours).  
Our target variable is:

- **`'LONG_OUTAGE'`** — equal to 1 if `'OUTAGE.DURATION.MIN'` > 1440, and 0 otherwise.

---

### Engineered Features

We created two new features, both of which are known at the start of an outage:

- **`SALES_PER_CUSTOMER`** = `TOTAL.SALES / TOTAL.CUSTOMERS`  
  Reflects average electrical load per customer. Higher load may complicate restoration.

- **`CUSTOMERS_PER_AREA`** = `TOTAL.CUSTOMERS / AREAPCT_URBAN`  
  Measures how concentrated customers are relative to the amount of urban land in the state.

---

### Features in the Final Model

Our final model uses a larger feature set than the baseline, including:

- **Temporal:**  
  - `YEAR`, `MONTH
  `
  Capture long-term infrastructure trends and strong seasonal patterns (e.g., hurricane season, winter storms). Certain months are systematically more likely to produce long outages due to climate cycles.

- **Climate:**  
  - `ANOMALY.LEVEL`, `CLIMATE.REGION`
  
  Outage severity is strongly influenced by weather anomalies and regional climate patterns. These features help the model distinguish states routinely impacted by severe storms from those with more stable climates.

- **Grid region:**  
  - `NERC.REGION`
  
  Different NERC regions operate under different grid architectures and reliability standards. These structural differences influence how quickly restoration can occur after a major outage.

- **Economic & infrastructure:**  
  - `TOTAL.PRICE`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`  
  - `PC.REALGSP.STATE`, `UTIL.REALGSP`
  
  These capture economic capacity, grid size, demand load, and reinvestment levels. States with higher consumption or larger grids may face more complex restoration processes after severe outages.

- **Urbanization & demographic:**  
  - `POPULATION`, `POPPCT_URBAN`, `POPDEN_URBAN`, `POPDEN_RURAL`, `AREAPCT_URBAN`
  
  Restoration difficulty depends on how populations are distributed. Dense urban areas can slow repairs due to infrastructure congestion, while sparse rural areas often require longer travel and repair times.

- **Engineered features:**  
  - `SALES_PER_CUSTOMER`, `CUSTOMERS_PER_AREA`
  
  Designed to capture average grid load per customer and concentration of customers relative to available urban land, both of which affect outage severity and restoration complexity.

These variables offer a more complete view of the conditions present at outage onset.

---

### Modeling Algorithm

We use a **Decision Tree Classifier** inside an `sklearn` **Pipeline**, which performs:

- **Scaling** of numeric features  
- **One-Hot Encoding** of categorical features  
- **Balanced class weighting** (`class_weight="balanced"`) to address the imbalance between long and short outages

Decision Trees can model **nonlinear relationships** and **feature interactions**, both of which are likely present in outage duration patterns.

---

### Hyperparameter Tuning

We tune the model using **GridSearchCV** with 4-fold cross-validation and optimize for **F1-score**, selecting the hyperparameters that achieve the highest average F1-score across the folds.

The following hyperparameters were searched:

- **`max_depth`** — controls model complexity  
- **`min_samples_split`** — minimum samples required to split  
- **`min_samples_leaf`** — minimum samples per leaf to reduce overfitting  

**Best hyperparameters found:**

```
max_depth = 14
min_samples_split = 10
min_samples_leaf = 4
```

---

### Final Model Performance

On the held-out test set, our final model achieves:

- **F1-score: 0.662**

Compared to the baseline:

- **Baseline Logistic Regression F1:** 0.426  
- **Final Decision Tree F1:** 0.662  

This is a **substantial improvement** in predictive performance.

---

### Why the Final Model Performs Better

- Outage duration is influenced by **nonlinear interactions** between climate, infrastructure, and population structure. The Decision Tree captures these interactions, while Logistic Regression cannot.  
- The **expanded feature set** provides more information about conditions and demographics.  
- The **engineered features** explicitly encode grid load and customer density, both of which logically affect restoration difficulty.

Together, these improvements enable the final model to identify long outages far more accurately than the baseline.

---

## Fairness Analysis
- In this step, we evaluate whether our final model behaves fairly across different groups: **urban-dominant states** and **rural-dominant states**.

### Group Definitions

- **Urban Group (Group X):** States where more than 70% of the population lives in urban areas (`POPPCT_URBAN > 70`)
- **Rural Group (Group Y):** States where 70% or less of the population lives in urban areas (`POPPCT_URBAN <= 70`)

This grouping is meaningful because restoration difficulty and infrastructure quality can differ substantially between urban and rural regions.

### Evaluation Metric: Precision

We evaluate fairness using **precision**, since long outages (the positive class) are relatively rare and precision measures how often the model's positive predictions are correct.

### Hypotheses

- **Null Hypothesis (H0):**  
  The model is fair. Precision for urban and rural outages is the same, and any observed difference is due to chance.

- **Alternative Hypothesis (H1):**  
  The model is unfair. Precision for rural outages is lower than precision for urban outages.

### Test Statistic

We use the difference in precision between the groups:

Test statistic T = (Urban precision) - (Rural precision)

From our model predictions:

- Urban precision = 0.6133  
- Rural precision = 0.6429  

Observed T = -0.0295

### Permutation Test

We run a permutation test with **N = 5000** permutations.  
In each iteration, we shuffle the group labels and recompute the precision difference.  
This represents a world where group membership does not affect model performance (the null scenario).

### Significance Level

We use a **one-sided** test at **alpha = 0.05**, testing whether rural precision is lower than urban precision.

<iframe
  src="assets/fairness_analysis.html"
  width="800"
  height="400"
  frameborder="0"
  style="display:block;"
></iframe>


### p-value

p-value = 0.6144

### Conclusion

Because the p-value (0.6144) is much larger than 0.05, we **fail to reject the null hypothesis**.  
There is **no statistical evidence** that the model performs worse for rural outages than for urban outages.



---