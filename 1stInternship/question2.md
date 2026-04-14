# 🎓 Viva Preparation Guide
## Flight Cancellation Analysis and Prediction
### Adabala Pavan | Roll No: 122210701103 | B.Tech CSE (AI & DS) | The Apollo University

---

> **⭐ HOW TO USE THIS GUIDE:**  
> Read chapter-by-chapter. Focus on bold **keywords**. Practise saying answers out loud — viva is spoken, not written. Aim for 2–4 sentences per answer.

---

# 📋 TABLE OF CONTENTS

1. [Chapter 1 — Introduction](#chapter-1)
2. [Chapter 2 — Literature Review](#chapter-2)
3. [Chapter 3 — Dataset Description](#chapter-3)
4. [Chapter 4 — Data Preprocessing](#chapter-4)
5. [Chapter 5 — Exploratory Data Analysis](#chapter-5)
6. [Chapter 6 — Feature Engineering](#chapter-6)
7. [Chapter 7 — Model Development & Evaluation](#chapter-7)
8. [Chapter 8 — Discussion](#chapter-8)
9. [Chapter 9 — Future Enhancements](#chapter-9)
10. [Chapter 10 — Conclusion](#chapter-10)
11. [Top 20 Most Important Viva Questions](#top-20)
12. [Quick Revision Notes](#quick-revision)

---

<a name="chapter-1"></a>
# Chapter 1: Introduction

## 1.1 Background and Motivation

### Q1. What is the main problem your project addresses?
**Answer:** Flight cancellations are one of the most disruptive events in the aviation industry, causing financial losses to airlines and severe inconvenience to passengers. With millions of domestic US flights each year, being able to predict whether a flight will be cancelled **before it departs** has immense commercial value. This project builds a data-driven system to make such predictions using historical flight data.

### Q2. ⭐ Why is flight cancellation prediction important?
**Answer:** Airlines can use cancellation predictions to pre-position spare aircraft and crew, reducing operational disruption. Passengers benefit by receiving advance alerts (24–48 hours before departure) so they can rebook proactively. Revenue management systems can also adjust pricing based on cancellation risk, and airports can optimise ground staffing accordingly.

### Q3. What is the scope of your project?
**Answer:** The project covers the complete end-to-end data science pipeline: data acquisition, preprocessing, exploratory data analysis (EDA), feature engineering, machine learning model development, and evaluation. It uses domestic US flight data from 2018–2019 (approximately 12 million records), focuses exclusively on **pre-departure features** to simulate real-world prediction conditions, and evaluates four ML classifiers.

### Q4. What are the primary objectives of your project?
**Answer:** The objectives are: (1) Perform comprehensive EDA to understand cancellation patterns, (2) Engineer predictive features from raw flight attributes, (3) Handle severe class imbalance using SMOTE, (4) Train and compare multiple ML classifiers, and (5) Identify the best-performing model for deployment in airline operational systems.

---

<a name="chapter-2"></a>
# Chapter 2: Literature Review

## 2.2 Early Rule-Based and Statistical Approaches

### Q5. What were the early approaches to flight prediction?
**Answer:** Early approaches used rule-based systems and statistical models like **logistic regression** and linear regression. These models were interpretable but could not capture non-linear relationships between flight attributes and cancellation risk. They also struggled with the large number of categorical variables (airlines, airports) common in aviation datasets.

## 2.3 Machine Learning Approaches

### Q6. What did Chakrabarty (2019) contribute?
**Answer:** Chakrabarty (2019) applied a **data mining approach** for flight arrival delay prediction for American Airlines. The study demonstrated that machine learning can extract useful predictive patterns from historical BTS flight records, laying groundwork for applying similar techniques to cancellation prediction.

### Q7. What did Manna et al. (2017) find about ensemble methods?
**Answer:** Manna et al. (2017) evaluated Decision Trees, Random Forests, and Gradient Boosting for delay prediction and found that **ensemble methods consistently outperformed single estimators**. They also highlighted the importance of feature engineering — specifically time-of-day and day-of-week indicators — in improving model accuracy.

### Q8. What did Rajeev (2021) show about cancellation-specific prediction?
**Answer:** Rajeev (2021) applied Logistic Regression and Random Forest to the 2015 BTS dataset and demonstrated that **Precision is a superior metric to Accuracy** for imbalanced cancellation datasets. Their work achieved F1-Scores of up to 0.82 using balanced class weights.

## 2.4 Addressing Class Imbalance

### Q9. ⭐ What is SMOTE and who introduced it?
**Answer:** **SMOTE (Synthetic Minority Over-sampling Technique)** was introduced by Chawla et al. (2002). It generates synthetic minority-class samples by interpolating between existing minority examples using a k-nearest-neighbours approach. This creates new, artificial "cancelled flight" examples that balance the training set without simply copying existing ones, which reduces overfitting compared to naive oversampling.

## 2.5 Deep Learning Approaches

### Q10. What are RNNs and LSTMs in the context of this domain?
**Answer:** **RNNs (Recurrent Neural Networks)** and **LSTMs (Long Short-Term Memory)** networks have been applied to capture temporal dependencies in sequential flight data. While promising on large datasets, they have higher computational cost and reduced interpretability compared to tree-based ensembles, making them less practical for operational deployment in smaller organisations.

## 2.6 Research Gap

### Q11. What research gap does your project fill?
**Answer:** Most existing research focuses on **delay prediction** rather than cancellation prediction, or conflates the two. Studies targeting cancellation are often restricted to a single airline or small airport subset. Very few provide a complete, reproducible end-to-end pipeline covering all stages from data acquisition to model evaluation. This project fills that gap by providing a comprehensive, well-documented, multi-airline, multi-airport pipeline.

---

<a name="chapter-3"></a>
# Chapter 3: Dataset Description

## Dataset Overview Diagram

```
Bureau of Transportation Statistics (BTS)
           |
           v
+----------------------------------+
|  US Domestic Flight Records      |
|  Period: Jan 2018 – Dec 2019     |
|  Records: ~12 Million flights    |
|  Format: Monthly CSV files       |
|  110 raw columns per record      |
+----------------------------------+
           |
     After Selection
           v
+----------------------------------+
|  28 Pre-departure Features       |
|  ~22 Airlines | ~352 Airports    |
|  Target: CANCELLED (0 or 1)      |
+----------------------------------+
```

### Q12. ⭐ Where is your dataset from and what does it contain?
**Answer:** The dataset is from the **Bureau of Transportation Statistics (BTS) On-Time Performance Database**, sourced via Kaggle. It covers US domestic flights from **January 2018 to December 2019**, containing approximately **12 million flight records**. Each row represents one scheduled flight, with 110 raw columns covering scheduling, carrier, airport, and operational details.

### Q13. What are the key features in your dataset?
**Answer:** Key features include:
- **FL_DATE** – Date of the flight
- **OP_UNIQUE_CARRIER** – Airline code (e.g., AA, DL, WN)
- **ORIGIN / DEST** – Origin and destination airport codes
- **CRS_DEP_TIME / CRS_ARR_TIME** – Scheduled departure and arrival times
- **DISTANCE** – Route distance in miles
- **CANCELLED** – Target variable (1=Cancelled, 0=Not)
- **CANCELLATION_CODE** – Reason: A=Carrier, B=Weather, C=NAS, D=Security

### Q14. What are the dataset statistics?

| Statistic | Value |
|---|---|
| Total Records | ~12,000,000 |
| Raw Features | 110 |
| Features Used | 28 |
| Cancelled Flights | ~180,000 (~1.5%) |
| Non-Cancelled | ~11,820,000 (~98.5%) |
| Airlines | 22 |
| Airports | 352 |

## 3.5 Class Imbalance

### Q15. ⭐ What is class imbalance and why is it a problem here?
**Answer:** **Class imbalance** occurs when one class in the target variable is far more frequent than another. Here, only ~1.5% of flights are cancelled and 98.5% are not. This is a critical problem because a naive model that always predicts "not cancelled" would achieve **98.5% accuracy while being completely useless** — it would never identify a single cancellation. This is why accuracy alone is not a suitable metric here.

## 3.6 Cancellation Reasons

### Q16. What are the four cancellation reason codes?
**Answer:**

| Code | Reason | Percentage |
|---|---|---|
| B | Weather | ~54% |
| A | Carrier (Airline) | ~28% |
| C | National Air System (NAS) | ~17% |
| D | Security | ~1% |

Weather is the dominant cause, responsible for more than half of all cancellations.

---

<a name="chapter-4"></a>
# Chapter 4: Data Preprocessing

## Preprocessing Pipeline Diagram

```
Raw BTS CSV Files (Monthly, 110 columns)
           |
           v
+---------------------------+
| Step 1: Data Loading      |
| Concatenate 24 CSVs into  |
| single Pandas DataFrame   |
+---------------------------+
           |
           v
+---------------------------+
| Step 2: Column Selection  |
| Drop 82 post-departure,   |
| leaky, and sparse columns |
| Keep 28 relevant features |
+---------------------------+
           |
           v
+---------------------------+
| Step 3: Missing Values    |
| Impute CRS_ELAPSED_TIME   |
| Drop DEP/ARR_DELAY rows   |
| Impute DISTANCE by route  |
+---------------------------+
           |
           v
+---------------------------+
| Step 4: Type Conversion   |
| HHMM → minutes since      |
| midnight; FL_DATE → MONTH,|
| DAY_OF_WEEK, DAY_OF_MONTH |
+---------------------------+
           |
           v
+---------------------------+
| Step 5: Outlier Removal   |
| IQR / Box-whisker method  |
| Remove CRS_ELAPSED_TIME   |
| < 30 or > 600 minutes     |
+---------------------------+
           |
           v
+---------------------------+
| Step 6: Encoding          |
| Low-cardinality → label   |
| High-cardinality → freq   |
| encoding (carrier, airport|
+---------------------------+
           |
           v
+---------------------------+
| Step 7: Train/Test Split  |
| 80/20, stratified on      |
| CANCELLED target variable |
| 2018 train → 2019 test    |
+---------------------------+
           |
           v
  Clean Dataset (~11.8M records)
```

### Q17. ⭐ Why did you drop post-departure columns?
**Answer:** Post-departure columns like WHEELS_OFF, AIR_TIME, DEP_DELAY, and ARR_DELAY are **not available at prediction time** — they are recorded only after a flight has departed or landed. Including them would cause **data leakage**, where the model learns from information it would never have in a real deployment scenario, leading to falsely inflated performance.

### Q18. What is data leakage and how did you prevent it?
**Answer:** **Data leakage** is when information from outside the training window — or information unavailable at prediction time — is used to train a model, leading to unrealistically good performance that collapses in deployment. We prevented leakage by (1) dropping all post-departure columns, (2) applying SMOTE only on the training set, (3) computing rolling carrier cancellation rates only from past data, and (4) using 2019 data exclusively for testing.

### Q19. How did you handle missing values?
**Answer:**
- **CRS_ELAPSED_TIME (0.02% missing):** Imputed with the **median** value computed for each (ORIGIN, DEST) route pair, as flight duration is strongly route-dependent.
- **CANCELLATION_CODE (~98.5% missing):** Kept only for EDA; excluded from modelling features since it's mostly missing for non-cancelled flights.
- **DEP_DELAY and ARR_DELAY (~3–4%):** Rows dropped since these are post-departure features not used in the prediction model.
- **DISTANCE (<0.01%):** Imputed with median by route.

### Q20. ⭐ How did you handle high-cardinality categorical features?
**Answer:** Features with **high cardinality** (many unique values) — like OP_UNIQUE_CARRIER (22 airlines) and ORIGIN/DEST (352 airports each) — were **frequency-encoded** using the training set's carrier cancellation rate and airport departure frequency. This preserves predictive signal while avoiding the dimensionality explosion that would result from one-hot encoding 352 airports.

### Q21. How did you convert time features?
**Answer:** CRS_DEP_TIME and CRS_ARR_TIME were stored as integers in HHMM format (e.g., 1345 = 1:45 PM). These were converted to **total minutes since midnight** (e.g., 1345 → 815 minutes) to create a continuous numerical representation that also enables cyclical encoding. FL_DATE was parsed into a Pandas datetime object to extract MONTH, DAY_OF_WEEK, and DAY_OF_MONTH.

### Q22. What memory optimisation did you apply?
**Answer:** Numerical columns were **downcast** to smaller data types — float64 → float32, int64 → int16 — reducing the total memory footprint from approximately **~9 GB to ~3 GB**. This was critical for processing 12 million records without running out of RAM.

### Q23. How was the train/test split performed?
**Answer:** The dataset was split **80/20** into training and test sets, **stratified** on the CANCELLED target variable to ensure both splits contain the same ~1.5% cancellation rate. Crucially, 2019 data was used for testing and 2018 data primarily for training, **preserving the temporal order** of the data and preventing future data from leaking into the training set.

---

<a name="chapter-5"></a>
# Chapter 5: Exploratory Data Analysis (EDA)

## EDA Overview

```
EDA focuses on answering:
"What patterns predict flight cancellation?"

     +----------+     +----------+     +----------+     +----------+
     | Temporal |     | Carrier  |     | Airport  |     |  Route   |
     | Analysis |     | Analysis |     | Analysis |     | Analysis |
     +----------+     +----------+     +----------+     +----------+
         |                 |               |                 |
    Monthly/DOW/        Rate per       Top airports      Distance
    Time-of-day         carrier        by total &        category
    trends              airline        rate              effects
         |                 |               |                 |
         +--------+--------+-------+-------+
                           |
                    +-------------+
                    | Correlation |
                    | Heatmap     |
                    +-------------+
                           |
                    +-------------+
                    | Key EDA     |
                    | Findings &  |
                    | Feature     |
                    | Decisions   |
                    +-------------+
```

### Q24. What was the overall cancellation rate in your dataset?
**Answer:** The overall cancellation rate was **1.47%** across 2018–2019. Weather (Code B) was the single largest contributor at 54% of all cancellations. Cancellation rates varied substantially by carrier — some regional airlines had rates above 3%, while major hub carriers maintained rates below 1%.

## 5.3 Temporal Analysis

### Q25. ⭐ What seasonal trends did you find?
**Answer:** Strong monthly patterns were observed. **January and February** had the highest cancellation rates (2.8% and 3.1% respectively), driven by peak winter weather disruptions. **June and July** also showed elevated rates (~1.9%) due to summer thunderstorm activity. **October and November** had the lowest rates (~0.7%), consistent with stable autumn weather — making Autumn the safest season to fly.

### Monthly Cancellation Pattern (ASCII Chart)

```
Cancellation Rate (%) by Month:

3.5% |
3.0% |   ██
2.5% |   ██           ██ ██
2.0% |   ██  █        ██ ██
1.5% |   ██  █  █  █  ██ ██  █  █
1.0% |   ██  █  █  █  ██ ██  █  █  █  █
0.5% |   ██  █  █  █  ██ ██  █  █  █  █  ██ ██
     +---+---+--+--+--+--+--+--+--+--+--+--+---
     J   F   M  A  M  J  J  A  S  O  N  D
     [-----Winter------][--Summer-][Autumn][W]
```

### Q26. What did day-of-week analysis show?
**Answer:** **Monday and Friday** had higher cancellation rates (1.7% and 1.6%) compared to mid-week days, likely because these are peak travel days with higher demand and operational stress. **Saturday** had the lowest cancellation rate at 1.1%, as lower passenger volumes reduce operational strain.

### Q27. ⭐ What did the time-of-day analysis reveal?
**Answer:** **Early morning flights (5:00 AM – 7:00 AM)** had the lowest cancellation rates because these are the first departures of the day using aircraft that overnighted at the airport — they have no accumulated delay. **Late-evening flights (after 9:00 PM)** had the highest cancellation rates because they accumulate cascading delays from all preceding flights throughout the day.

### Time-of-Day Risk Diagram

```
Time-of-Day vs Cancellation Risk:

RISK:   LOW       MEDIUM    MEDIUM    HIGH      HIGHEST
        |---------|---------|---------|---------|---------|
TIME:  4AM-8AM  8AM-12PM  12PM-4PM  4PM-8PM  8PM-4AM
        |                                         |
     First flight of day              Delay cascade
     = overnight aircraft             accumulates here
```

## 5.4 Carrier Analysis

### Q28. ⭐ Which airlines had the most cancellations, and why is rate more important than count?
**Answer:** **Southwest Airlines (WN) and American Airlines (AA)** had the highest absolute numbers of cancellations due to their enormous scale of operations. However, absolute count is misleading — on a **rate-adjusted basis**, some regional carriers had cancellation rates 3–4 times higher than major carriers. **Delta Airlines (DL)** consistently maintained among the lowest cancellation rates across both years, demonstrating superior operational reliability.

## 5.5 Airport Analysis

### Q29. Which airports had the highest cancellation impact?
**Answer:** **Chicago O'Hare (ORD)** and **Dallas/Fort Worth (DFW)** ranked among the highest for total cancellations due to their enormous traffic volume and exposure to severe weather. **LaGuardia (LGA)** showed disproportionately high cancellation rates relative to its traffic volume, attributed to its congested airspace and limited runway capacity.

## 5.6 Route Analysis

### Q30. What did route-level analysis reveal?
**Answer:** **Short-haul routes under 300 miles** had significantly higher cancellation rates than medium and long-haul routes. Two reasons explain this: (1) short routes are operated by smaller regional aircraft more susceptible to weather, and (2) many short routes have fewer flights per day, making recovery from disruption harder — there are fewer opportunities to rebook passengers onto later flights.

## 5.7 Correlation Analysis

### Q31. What did the correlation heatmap show?
**Answer:** The heatmap revealed that **DISTANCE and CRS_ELAPSED_TIME were strongly correlated (r = 0.93)**, indicating potential collinearity that was addressed during feature selection by keeping one. No single numerical feature correlated above **r = 0.15** with the CANCELLED target, which confirms that no single variable is sufficient on its own and that **ensemble modelling is necessary** to combine multiple weak predictive signals.

---

<a name="chapter-6"></a>
# Chapter 6: Feature Engineering

## Feature Engineering Overview Diagram

```
Raw Features (28)
        |
        +---> Temporal Engineering
        |         - CRS_DEP_TIME → Time Bins (5 categories)
        |         - MONTH → SEASON (4 seasons)
        |         - DAY_OF_WEEK → IS_WEEKEND (binary)
        |         - Cyclical Encoding: MONTH_SIN, MONTH_COS
        |                              DOW_SIN,   DOW_COS
        |
        +---> Carrier-Level Engineering
        |         - CARRIER_CANCELLATION_RATE (30-day rolling)
        |         - CARRIER_SEASON_INTERACTION (carrier × season)
        |
        +---> Airport-Level Engineering
        |         - ORIGIN_CANCELLATION_RATE
        |         - ROUTE_FREQUENCY (avg daily flights per route)
        |         - HUB_ORIGIN (binary flag, top-20 airports)
        |
        +---> Route Engineering
                  - DISTANCE_CATEGORY (Short/Medium/Long)
                  - ROUTE_PAIR features

Final Feature Set: 38 features
```

### Q32. What is feature engineering and why is it important?
**Answer:** **Feature engineering** is the process of creating new input variables from raw data using domain knowledge, with the goal of better capturing predictive signals for the model. It is important because raw features alone may not expose the patterns a model needs — for example, the raw month number doesn't capture the *cyclical* nature of seasons or the *seasonal risk* specific to each carrier. Engineered features consistently improved model F1-Score in this project.

## 6.2 Temporal Features

### Q33. ⭐ What are the 5 departure time bins and their rationale?

| Bin Name | Time Range | Rationale |
|---|---|---|
| Early Morning | 04:00–07:59 | Lowest risk; first departure of day |
| Morning | 08:00–11:59 | Moderate risk; weather developing |
| Midday | 12:00–15:59 | Moderate risk; afternoon thunderstorms |
| Evening | 16:00–19:59 | Higher risk; delay accumulation |
| Late Night | 20:00–03:59 | Highest risk; maximum delay cascade |

### Q34. What is cyclical encoding and why is it used?
**Answer:** **Cyclical encoding** addresses the problem that calendar variables like MONTH and DAY_OF_WEEK are inherently circular — December is adjacent to January, and Sunday is adjacent to Monday. If we encode months as 1–12, the model sees months 1 and 12 as far apart, which is wrong. Cyclical encoding applies **sine and cosine transformations**:

```
MONTH_SIN = sin(2π × MONTH / 12)
MONTH_COS = cos(2π × MONTH / 12)
```

This places December and January close together in the feature space, correctly representing their temporal adjacency.

### Q35. What is the SEASON feature?
**Answer:** The SEASON feature was derived from MONTH, grouping months into four categories: **Winter (Dec–Feb), Spring (Mar–May), Summer (Jun–Aug), and Autumn (Sep–Nov)**. This captures the dominant seasonal weather pattern driving cancellations at a coarser, more interpretable granularity than raw month numbers.

## 6.3 Carrier-Level Features

### Q36. ⭐ What is the rolling carrier cancellation rate?
**Answer:** For each carrier, the **average cancellation rate over the previous 30 days** was computed as a rolling feature. For example, if Southwest had a cancellation rate of 3.2% in the past month due to a labour dispute, that elevated rate becomes a feature for predicting future cancellations. This is critical because it captures **recent operational reliability trends** that static carrier identity cannot express. Importantly, it was computed only from the training set to prevent data leakage.

### Q37. What is the carrier-season interaction feature?
**Answer:** This is an **interaction feature** that combines carrier identity and season. It captures the fact that some carriers are disproportionately affected by particular seasonal weather patterns — for example, a carrier with heavy operations in Midwest hubs (Chicago, Detroit) may be more affected by winter storms than a carrier primarily serving Southern routes. Combining these two signals creates a richer predictive variable than either alone.

## 6.4 Airport-Level Features

### Q38. What is the Hub Indicator feature?
**Answer:** A binary **HUB_ORIGIN flag** was created for the top 20 highest-traffic airports (e.g., ORD, DFW, ATL). Hub airports exhibit **distinct cancellation dynamics** — they are highly susceptible to ripple effects since a disruption at a hub can cascade across hundreds of connecting flights. Flagging hub airports separately allows the model to account for this heightened risk.

## 6.6 Cyclical Encoding

### Q39. ⭐ Explain the formula for cyclical encoding with an example.
**Answer:** For MONTH (values 1–12), the encoding is:
```
MONTH_SIN = sin(2π × MONTH / 12)
MONTH_COS = cos(2π × MONTH / 12)
```
Example:
- January (1):  SIN = sin(π/6) ≈ 0.50,  COS = cos(π/6) ≈ 0.87
- December (12): SIN = sin(2π) ≈ 0.00,  COS = cos(2π) ≈ 1.00

The sine and cosine pair together encode position on a circle, so December and January are geometrically close. The same logic applies to DAY_OF_WEEK and departure time.

## 6.7 Final Feature Set

### Q40. What is the final feature set used for modelling?
**Answer:** After feature engineering, the final dataset comprised **38 features**:
- 9 original features (MONTH, DAY_OF_WEEK, DISTANCE, etc.)
- 5 temporal engineered features (time bins, season, weekend flag, cyclical encodings)
- 4 carrier-level features (rolling cancellation rate, carrier-season interaction)
- 6 airport-level features (origin cancellation rate, hub flag, route frequency)
- 3 route features (distance category, route pairs)
- 11 encoded categorical features (frequency and label encodings)

---

<a name="chapter-7"></a>
# Chapter 7: Model Development and Evaluation

## Full ML Pipeline Diagram

```
Training Data (2018, ~9.4M records)
         |
         v
+---------------------------+
|   SMOTE Oversampling      |
|   k=5 nearest neighbours  |
|   Balances 1.5% → 50:50  |
+---------------------------+
         |
    BALANCED TRAINING SET
         |
    +----+----+----+----+
    |    |    |    |    |
    v    v    v    v    v
   LR   DT   RF  XGB  (Compare)
         |
    5-Fold Cross-Validation
    Optimise: F1-Score (minority)
         |
    Best Hyperparameters
         |
+---------------------------+
|   Evaluate on Test Set    |
|   (2019, ~2.4M records)   |
|   Natural imbalance kept  |
+---------------------------+
         |
  Precision | Recall | F1 | AUC-ROC
  Confusion Matrix | Feature Importance
```

## 7.2 SMOTE

### Q41. ⭐ How does SMOTE work step by step?
**Answer:** SMOTE (Synthetic Minority Over-sampling Technique) works as follows:
1. For each minority-class sample (cancelled flight), find its **k nearest neighbours** (k=5 in this project) in the feature space.
2. **Randomly select** one of those neighbours.
3. **Interpolate** a new synthetic sample on the line segment between the original sample and the selected neighbour using a random fraction between 0 and 1.
4. Repeat until the desired class balance is achieved.

The result is new, synthetic cancelled-flight examples that are plausible variations of real examples, **not exact duplicates**.

```
SMOTE Interpolation:
  
  Sample A ●---------●---------● Neighbour B
            ^    Synthetic
            |    point created
            |    here (at random
            |    point on line)
```

### Q42. ⭐ Why was SMOTE applied only to the training set?
**Answer:** SMOTE must only be applied to the **training set** because applying it to the test set would create artificial cancelled-flight examples in the evaluation data, making the test set no longer representative of real-world conditions. The test set must reflect the **natural 1.5% cancellation rate** to measure how the model performs in actual deployment scenarios.

## 7.3 Models Evaluated

### Q43. ⭐ Explain Logistic Regression and its role in your project.
**Answer:** **Logistic Regression** models the log-odds of the binary target (cancelled vs. not) as a linear combination of input features. It produces a probability output using the **sigmoid function**. In this project, it served as the **baseline model** because it is fast, interpretable, and provides calibrated probabilities. However, it cannot capture non-linear relationships, which limits its performance — it achieved F1 = 0.70 and AUC-ROC = 0.82.

### Q44. ⭐ Explain Random Forest and why it performed best.
**Answer:** **Random Forest** is a **bagging ensemble** method that trains multiple decision trees on different bootstrap samples of the training data. At each split, it considers only a random subset of features (`max_features='sqrt'`). Final predictions are made by majority voting across all trees. It performed best (F1 = 0.87, AUC-ROC = 0.91) because:
- Bagging reduces variance and overfitting
- Feature randomness decorrelates trees, increasing diversity
- It handles mixed feature types (numerical + categorical) without scaling
- It naturally provides feature importance rankings

```
Random Forest Architecture:

Training Data (Bootstrapped Subsets)
   |         |         |         |
 Tree 1    Tree 2    Tree 3  ... Tree 200
   |         |         |         |
  Pred1    Pred2    Pred3  ...  Pred200
   |_________|_________|___________|
                    |
            Majority Vote
                    |
             Final Prediction
```

### Q45. ⭐ Explain XGBoost and how it differs from Random Forest.
**Answer:** **XGBoost (Extreme Gradient Boosting)** is a **boosting ensemble** method. Unlike Random Forest (which trains all trees independently in parallel), XGBoost trains trees **sequentially** — each new tree specifically focuses on correcting the errors made by the previous trees. It also implements **L1 and L2 regularisation** to prevent overfitting. In this project, XGBoost achieved F1 = 0.85 and AUC-ROC = 0.90, slightly below Random Forest.

```
Bagging vs Boosting:

RANDOM FOREST (Bagging):
Data → [Tree1] → Prediction1
Data → [Tree2] → Prediction2   → VOTE → Final
Data → [Tree3] → Prediction3

XGBOOST (Boosting):
Data → [Tree1] → Residuals1
             ↓
         [Tree2] → Residuals2
                 ↓
             [Tree3] → ... → Final (sum of trees)
```

### Q46. What is a Decision Tree and what are its limitations?
**Answer:** A **Decision Tree** splits data based on feature thresholds to create a tree of if-then decision rules. It is highly interpretable but prone to **overfitting** — it memorises training data. In this project, max_depth=15 and min_samples_leaf=100 were used to limit overfitting. The Decision Tree achieved F1 = 0.75, better than Logistic Regression but significantly below ensemble methods.

## 7.4 Hyperparameter Tuning

### Q47. ⭐ What is hyperparameter tuning and what method did you use?
**Answer:** **Hyperparameter tuning** is the process of finding the best configuration settings for a model (e.g., number of trees, maximum depth). The project used **RandomizedSearchCV** with 5-fold stratified cross-validation. Rather than exhaustively testing all combinations (GridSearchCV), RandomizedSearchCV randomly samples from the parameter space, making it faster for large datasets. The **F1-Score of the minority (cancelled) class** was used as the optimisation metric.

## 7.5 Evaluation Metrics

### Q48. ⭐ Why not use Accuracy as the evaluation metric?
**Answer:** With 98.5% non-cancelled flights, a model that always predicts "not cancelled" achieves **98.5% accuracy but catches zero cancellations**. This is completely useless operationally. Instead, the project used:
- **Precision:** Of predicted cancellations, how many were real? (Avoid false alarms)
- **Recall:** Of actual cancellations, how many were caught? (Avoid missing real ones)
- **F1-Score:** Harmonic mean of Precision and Recall — the primary ranking metric
- **AUC-ROC:** Area Under the ROC Curve, measuring overall discriminative power

### Q49. ⭐ Explain Precision, Recall, and F1 with examples from your project.
**Answer:**
```
Confusion Matrix Definitions:

                   Predicted: NOT   Predicted: CANCELLED
Actual: NOT           TN              FP (False Alarm)
Actual: CANCELLED     FN (Missed)     TP (Correct Alert)

For Random Forest:
TN = 2,334,102   FP = 22,450
FN = 4,920       TP = 29,780

Precision = TP / (TP + FP) = 29,780 / (29,780 + 22,450) = 0.882
           → When we alert a passenger, we're correct 88.2% of the time

Recall    = TP / (TP + FN) = 29,780 / (29,780 + 4,920)  = 0.858
           → We catch 85.8% of all actual cancellations

F1-Score  = 2 × (Precision × Recall) / (Precision + Recall) = 0.87
```

### Q50. What is AUC-ROC?
**Answer:** **AUC-ROC** (Area Under the Receiver Operating Characteristic Curve) measures how well the model distinguishes between cancelled and non-cancelled flights across all classification thresholds. The ROC curve plots **True Positive Rate (Recall) vs. False Positive Rate** at different probability cutoffs. An AUC of **0.91** means the model has a 91% probability of ranking a randomly selected cancelled flight higher than a randomly selected non-cancelled flight.

```
AUC-ROC Comparison:

True Positive Rate (Recall)
1.0  |           /‾‾‾‾‾‾‾‾‾‾‾‾‾‾
     |         /  RF:0.91
     |        /   XGB:0.90
0.5  |      /     DT:0.83
     |    /       LR:0.82
     |  / (Random)
0.0  |/___________________
     0.0    0.5    1.0
          False Positive Rate
```

## 7.6 Results

### Q51. ⭐ Summarise the model comparison results.

| Model | Precision | Recall | F1-Score | AUC-ROC |
|---|---|---|---|---|
| Logistic Regression | 0.71 | 0.68 | 0.70 | 0.82 |
| Decision Tree | 0.76 | 0.74 | 0.75 | 0.83 |
| **Random Forest** | **0.88** | **0.86** | **0.87** | **0.91** |
| XGBoost | 0.86 | 0.84 | 0.85 | 0.90 |

**Random Forest is the best-performing model across all metrics.**

## 7.7 Feature Importance

### Q52. ⭐ What were the top features and what does this tell us?
**Answer:** The top 10 features by Random Forest importance (mean decrease in impurity) were:

| Rank | Feature | Importance |
|---|---|---|
| 1 | CARRIER_CANCELLATION_RATE (30-day rolling) | 0.142 |
| 2 | ORIGIN_CANCELLATION_RATE | 0.118 |
| 3 | MONTH (cyclical) | 0.109 |
| 4 | DEPARTURE_TIME_BIN | 0.097 |
| 5 | DISTANCE_CATEGORY | 0.088 |

The most important insight is that **engineered carrier and airport features outrank all raw features**, confirming that a flight's cancellation risk is substantially determined by the operational reliability of its carrier and origin airport.

## 7.8 Model Calibration

### Q53. What is model calibration and why does it matter?
**Answer:** **Model calibration** refers to how well a model's predicted probabilities align with actual outcomes. For example, if a model predicts 70% cancellation probability for 1,000 flights, ideally about 700 of them should actually be cancelled. The Random Forest showed **slight overconfidence** (predicted probabilities were too high) in the 0.6–0.8 range. This was corrected using **Platt scaling**, which fits a logistic regression on top of the raw model outputs to produce well-calibrated probabilities suitable for operational alert systems.

---

<a name="chapter-8"></a>
# Chapter 8: Discussion

### Q54. ⭐ What are the key findings of your project?
**Answer:** The key findings are:
1. **Carrier and airport historical rates are the strongest predictors** — more predictive than any individual flight attribute.
2. **Winter months have 2–3× the cancellation risk of autumn months.**
3. **Late-evening flights accumulate significantly more cancellation risk** due to delay cascades.
4. **Short-haul routes (< 300 miles) are more prone to cancellation** than long-haul routes.
5. **No single feature has correlation > 0.15** with the target, confirming the need for ensemble modelling.
6. **SMOTE was critical** — without it, models failed to identify the minority class reliably.

### Q55. How does your model compare with published literature?
**Answer:** The Random Forest F1-Score of **0.87** compares favourably with reported F1-Scores of 0.78–0.84 in similar cancellation prediction studies (e.g., Rajeev 2021 achieved 0.82). The improvement is attributable to (1) richer feature engineering including rolling carrier rates and cyclical time encodings, and (2) the use of more recent 2018–2019 data rather than the 2015 dataset used in prior studies.

### Q56. ⭐ What are the main limitations of your project?
**Answer:**
1. **No real-time weather data** — Weather causes 54% of cancellations, yet the model lacks weather forecast features (wind speed, precipitation, visibility). This is the single most impactful limitation.
2. **Temporal drift** — Trained on 2018–2019 data; aviation changed significantly after COVID-19. The model would need retraining on recent data.
3. **Pre-departure only** — Some additional signals available at T-2 hours (inbound aircraft position, gate availability) are not used.
4. **Binary prediction only** — The model predicts cancelled/not-cancelled but not the reason (A/B/C/D), which would be more actionable for airlines.

### Q57. What are the operational applications of your model?
**Answer:**
- **Passenger Alert Systems:** Notify passengers 24–48 hours before departure of high-risk flights, enabling proactive rebooking.
- **Crew and Aircraft Positioning:** Pre-position spare aircraft and crew at high-risk airports during risky periods.
- **Dynamic Pricing:** Adjust ticket prices on high-cancellation-risk routes to account for expected rebooking costs.
- **Airport Capacity Planning:** Adjust gate staffing and passenger services based on aggregate cancellation risk forecasts.

---

<a name="chapter-9"></a>
# Chapter 9: Future Enhancements

### Q58. What is the most impactful future enhancement?
**Answer:** The most impactful enhancement would be **integrating real-time and forecast weather data** from sources like the National Weather Service (NWS) API or the Aviation Weather Center. Features such as surface wind speed, precipitation type, ceiling height, visibility, and SIGMET alerts at the origin airport could be added. Since weather drives 54% of cancellations, this single addition would likely yield the largest improvement in model performance.

### Q59. How could deep learning improve this project?
**Answer:** **LSTM (Long Short-Term Memory)** networks and Transformer-based models could capture complex temporal dependencies across a sequence of flights operated by the same aircraft (tail-number tracking). A flight's cancellation risk often depends on the performance history of the inbound aircraft, a dependency that traditional ML models cannot capture since they treat each flight as an independent sample.

### Q60. What is SHAP and how would it improve the model?
**Answer:** **SHAP (SHapley Additive exPlanations)** provides per-prediction explanations by quantifying each feature's contribution to a specific prediction. For example: *"This flight has a 73% cancellation risk, primarily because it departs from ORD in February (weather risk +0.31) and the carrier had elevated cancellation rates this week (+0.22)."* This explainability is essential for airline operational teams who need to understand why the model flagged a specific flight.

### Q61. How would you deploy the model in production?
**Answer:** The trained Random Forest model would be **serialised using joblib** and deployed as a **REST API using Flask or FastAPI**. A **Docker container** would ensure reproducible deployment across different infrastructure environments. Airlines or travel apps could query this API with flight details (carrier, origin, departure time, season) and receive a cancellation probability score in real time.

### Q62. What is network-level modelling and why is it valuable?
**Answer:** Individual flight cancellations do not occur in isolation — a single cancellation at a hub airport can **cascade into dozens of downstream cancellations**. Network-level modelling treats the aviation network as a **graph** where airports are nodes and routes are edges. Incorporating network-level disruption indicators (e.g., number of incoming delayed flights to a hub) as additional features could significantly improve prediction accuracy during periods of widespread disruption.

---

<a name="chapter-10"></a>
# Chapter 10: Conclusion

### Q63. Summarise your project in 5 sentences.
**Answer:** This project delivered a complete end-to-end data science pipeline for flight cancellation prediction using 12 million BTS domestic flight records (2018–2019). After systematic data preprocessing and comprehensive EDA, 38 features were engineered including rolling carrier cancellation rates and cyclical time encodings. SMOTE was applied to address the severe 1.5% class imbalance. Four ML classifiers were trained and evaluated — the Random Forest achieved the best performance with F1 = 0.87 and AUC-ROC = 0.91. The results demonstrate that historical pre-departure attributes are strong predictors of cancellation risk and can power passenger alert systems, airline operational planning tools, and airport capacity management dashboards.

### Q64. What were your personal learning outcomes?
**Answer:** Key technical skills developed include:
- Large-scale data processing with **Pandas** on multi-gigabyte datasets (12M records, 9GB → 3GB with optimisation)
- **Scikit-learn** model development, pipeline construction, and cross-validation
- Imbalanced classification handling with **SMOTE**
- Data visualisation using **Matplotlib, Seaborn, and Plotly**
- Version control and reproducible research using **Git and GitHub**

---

# Appendix Questions

### Q65. Explain the code for cyclical feature encoding.
**Answer:**
```python
# Cyclical encoding for MONTH
df['MONTH_SIN'] = np.sin(2 * np.pi * df['MONTH'] / 12)
df['MONTH_COS'] = np.cos(2 * np.pi * df['MONTH'] / 12)

# This ensures December (12) and January (1) are close
# in the 2D sin/cos feature space
```

### Q66. What SMOTE parameters did you use?
**Answer:**
```python
smote = SMOTE(k_neighbors=5, random_state=42)
X_train_res, y_train_res = smote.fit_resample(X_train, y_train)
# k_neighbors=5 means 5 nearest neighbours used for interpolation
# Applied ONLY on training set; test set untouched
```

### Q67. What are the Random Forest hyperparameters and their meaning?
**Answer:**
```python
rf = RandomForestClassifier(
    n_estimators=200,    # 200 decision trees in the ensemble
    max_depth=20,        # Maximum depth of each tree
    min_samples_leaf=50, # Minimum samples required at a leaf node (prevents overfitting)
    max_features='sqrt', # Use sqrt(38) ≈ 6 features per split (decorrelates trees)
    random_state=42
)
```

### Q68. What is the GitHub repo structure of your project?
**Answer:**
```
Flight-Cancellation-Analysis-and-Prediction/
├── data/
│   ├── raw/         # Raw BTS CSV files
│   └── processed/   # Cleaned, feature-engineered dataset
├── notebooks/
│   ├── 01_data_loading.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_eda.ipynb
│   ├── 04_feature_engineering.ipynb
│   ├── 05_modelling.ipynb
│   └── 06_evaluation.ipynb
├── src/
│   ├── preprocess.py
│   ├── features.py
│   └── model.py
├── models/
│   └── random_forest_v1.joblib
└── requirements.txt
```

---

<a name="top-20"></a>
# ⭐ TOP 20 MOST IMPORTANT VIVA QUESTIONS

> These are the questions most likely to be asked. Practise these first.

1. **What is the problem your project solves, and why is it important?**
2. **What dataset did you use, where is it from, and what are its key statistics?**
3. **What is class imbalance? How severe is it in your dataset, and why is it a problem?**
4. **What is SMOTE? How does it work, and why was it applied only to training data?**
5. **Why did you not use Accuracy as your primary evaluation metric?**
6. **Explain Precision, Recall, and F1-Score with numbers from your project.**
7. **What is AUC-ROC? What does your score of 0.91 mean?**
8. **What are post-departure columns? Why did you drop them?**
9. **What is data leakage? How did you prevent it?**
10. **Explain Random Forest. Why did it perform better than other models?**
11. **What is XGBoost? How is it different from Random Forest?**
12. **What is cyclical encoding? Why did you use it for MONTH?**
13. **What is the rolling carrier cancellation rate? Why is it the most important feature?**
14. **What seasonal patterns did you find in the EDA?**
15. **Why do late-evening flights have higher cancellation rates than early morning flights?**
16. **How did you handle high-cardinality categorical features like ORIGIN (352 airports)?**
17. **What are the limitations of your model?**
18. **What is model calibration and why did you use Platt scaling?**
19. **What future enhancements would you recommend? Which is most impactful?**
20. **Summarise your project end-to-end in 5–6 sentences.**

---

<a name="quick-revision"></a>
# 📝 QUICK REVISION NOTES

## Dataset at a Glance
- **Source:** Bureau of Transportation Statistics (BTS), via Kaggle
- **Period:** Jan 2018 – Dec 2019
- **Size:** ~12 million records, 110 raw → 28 → 38 (after engineering) features
- **Target:** CANCELLED (0 = Not, 1 = Cancelled)
- **Class ratio:** 1.5% cancelled, 98.5% not cancelled

## Cancellation Reasons
| Code | Reason | % |
|---|---|---|
| B | Weather | 54% |
| A | Carrier | 28% |
| C | NAS | 17% |
| D | Security | 1% |

## Key EDA Findings
- Worst months: **January (2.8%) and February (3.1%)**
- Best months: **October and November (~0.7%)**
- Worst day: Monday (1.7%) | Best day: Saturday (1.1%)
- Worst time: Late Night | Best time: Early Morning
- Worst airports: ORD, DFW, LGA
- Short-haul routes (< 300 miles) = highest cancellation rates

## Models Summary
| Model | F1 | AUC-ROC |
|---|---|---|
| Logistic Regression | 0.70 | 0.82 |
| Decision Tree | 0.75 | 0.83 |
| **Random Forest ✅** | **0.87** | **0.91** |
| XGBoost | 0.85 | 0.90 |

## Top 5 Features
1. Carrier Cancellation Rate (30-day rolling) — 0.142
2. Origin Airport Cancellation Rate — 0.118
3. Month (cyclical) — 0.109
4. Departure Time Bin — 0.097
5. Distance Category — 0.088

## Key Formulas

```
Precision = TP / (TP + FP)
Recall    = TP / (TP + FN)
F1-Score  = 2 × (Precision × Recall) / (Precision + Recall)

Cyclical: MONTH_SIN = sin(2π × MONTH / 12)
          MONTH_COS = cos(2π × MONTH / 12)
```

## RF Confusion Matrix (Test Set — 2019)
```
                  Predicted: NOT  Predicted: CANCELLED
Actual: NOT       2,334,102 (TN)  22,450 (FP)
Actual: CANCELLED    4,920 (FN)   29,780 (TP)

Precision = 88.2%  |  Recall = 85.8%  |  F1 = 0.87
```

## Key Terminology Cheatsheet

| Term | Meaning |
|---|---|
| BTS | Bureau of Transportation Statistics |
| SMOTE | Synthetic Minority Over-sampling Technique |
| EDA | Exploratory Data Analysis |
| AUC-ROC | Area Under Receiver Operating Characteristic Curve |
| F1-Score | Harmonic mean of Precision and Recall |
| Bagging | Bootstrap Aggregating (Random Forest method) |
| Boosting | Sequential correction-based ensemble (XGBoost) |
| Platt Scaling | Probability calibration using logistic regression |
| SHAP | SHapley Additive exPlanations |
| NAS | National Air System |
| Data Leakage | Using future/unavailable data in training |
| Class Imbalance | Unequal representation of target classes |
| Cyclical Encoding | sin/cos transformation for circular features |
| Frequency Encoding | Replacing category with its historical rate |
| Feature Importance | How much each feature reduces impurity in RF |

---
