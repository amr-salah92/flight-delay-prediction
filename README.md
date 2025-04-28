# Flight Delay Prediction Analysis Report  
**SkyAnalytics Inc. - Enhancing Airline Operational Efficiency through Data-Driven Insights**

---

## Table of Contents

1. [Project Background](#project-background)
2. [Project Goals](#project-goals)
3. [Data Structure & Initial Checks](#data-structure--initial-checks)
4. [Data Preprocessing & Modeling](#data-preprocessing--modeling)
5. [Executive Summary](#executive-summary)
6. [Insights Deep Dive](#insights-deep-dive)
7. [Recommendations](#recommendations)
8. [Technical Details](#technical-details)
9. [Assumptions and Caveats](#assumptions-and-caveats)

---

## Project Background

**Company Overview**  
SkyAnalytics Inc. specializes in aviation analytics, providing actionable insights to airlines to optimize operations. Since 2018, the company has analyzed over 500,000 flight records annually, focusing on metrics like on-time performance, delay root causes, and operational efficiency.

**Business Model**  
SkyAnalytics partners with mid-sized airlines to reduce delay-related costs (e.g., \$3.2 billion in annual industry losses from delays) and improve passenger satisfaction. Key metrics include delay frequency, weather impact, and peak delay periods.

**Current Analysis**  
This project uses synthetic data mimicking 50,000 flight records (2020–2023) to predict delays and identify contributing factors.

---

## Project Goals

1. **Identify Delay Patterns**: Determine how temporal factors (departure hour, day of week) correlate with delays.  
2. **Evaluate Weather Impact**: Quantify the effect of weather severity on delays.  
3. **Optimize Scheduling**: Provide data-backed recommendations for flight scheduling adjustments.  
4. **Improve Predictive Accuracy**: Enhance real-time delay prediction models to reduce airline costs by 15–20%.

---

## Data Structure & Initial Checks

**Dataset Overview**  
- **Rows**: 50,000 synthetic flight records  
- **Columns**:  
  1. **flight_id**: Unique flight identifier  
  2. **airline**: Categorical (7 airlines: Delta, United, JetBlue, etc.)  
  3. **origin/destination**: Categorical (8 airports: JFK, LAX, DEN, DFW, SFO, etc.)  
  4. **departure_hour**: Integer (0–23)  
  5. **day_of_week**: Integer (1–7)  
  6. **scheduled_duration_min**: Flight duration (60–360 minutes)  
  7. **distance_km**: Flight distance (300–4000 km)  
  8. **weather_severity**: Categorical (Clear, Rain, Storm, Snow)  
  9. **holiday**: Binary (0 = non-holiday, 1 = holiday)  
  10. **delayed**: Binary target (0 = on-time, 1 = delayed)

**Initial Checks**  
- No duplicates or missing values detected.  
- **Outlier Analysis**: Boxplots reveal no extreme outliers; the difference between the 25th percentile and minimum closely matches the difference between the 75th percentile and maximum, indicating symmetric distributions without skewed extremes.  
- **Correlation Checks**: Delay is positively correlated with `holiday` and `departure_hour`, highlighting calendar and temporal influences.

---

## Data Preprocessing & Modeling

1. **Categorical Encoding**  
   - Applied `pd.get_dummies` to `['airline', 'origin', 'destination', 'weather_severity']`
2. **Feature Scaling**  
   - Standardized continuous features: `['departure_hour', 'scheduled_duration_min', 'distance_km']`
3. **Class Balancing**  
   - Used both oversampling (e.g., SMOTE) and undersampling techniques to mitigate class imbalance.
4. **Model Selection & Tuning**  
   - Evaluated Random Forest, XGBoost, and LightGBM.  
   - Optimized hyperparameters using `GridSearchCV` with `StratifiedKFold` cross-validation.
5. **Ensemble Model**  
   - Constructed a `StackingClassifier` combining the tuned base learners.

**Performance Metrics**  
```
=== Stacking Classifier ===
              precision    recall  f1-score   support

           0       0.85      0.92      0.88      7,712
           1       0.91      0.84      0.87      7,712

    accuracy                           0.88     15,424
   macro avg       0.88      0.88      0.88     15,424
weighted avg     0.88      0.88      0.88     15,424

ROC AUC: 0.9397
Confusion Matrix:
[[7076, 636],
 [1272, 6440]]
```

---

## Executive Summary

- **Holidays & Peak Hours**: Delay probability increases on holidays (2,313 delays) and during rush hours (16:00–20:00).  
- **Weather Impact**: Clear days account for the highest number of delays (4,131 flights), though storms exhibit the greatest rate increase.  
- **Airport Flows**:  
  - **LAX**: Highest delays as origin; second-highest as destination.  
  - **SFO**: Fewest delays as origin; second-fewest as destination.  
  - **DEN**: Top destination; lowest origin.  
  - **DFW**: Top origin; lowest destination.  
- **Airline Insights**: JetBlue leads in flight count, average distance, and speed; ranks second in average duration.

---

## Insights Deep Dive

### Temporal Patterns
- **Rush Hours**: 16:00–20:00 show a 28% delay rate versus 10% during 00:00–06:00.  
- **Weekend Effects**: Higher delays on Saturdays and Sundays compared to mid-week.

### Holiday Correlation
- **Delays**: 2,313 on holidays vs. 9,127 on non-holidays.

### Weather Analysis
- **Clear vs. Severe**: Clear weather accounts for 4,131 delayed flights.  
- **Severity Ranking**: Storm > Snow > Rain > Clear in terms of rate increase.

### Airline Performance
- **JetBlue**: Top in flight volume, distance, speed; 2nd in duration.  
- **Spirit Airlines**: Highest delay rate (32%); **Delta**: Lowest (12%).

### Airport Dynamics
- **LAX**: Top delayed origin, 2nd as destination.  
- **SFO**:  Least delayed origin, 2nd least as destination.  
- **DEN/DFW**: DEN as top destination, DFW as top origin; each opposite for bottom ranking.

### Route Efficiency
- **Long-Haul (>3,000 km)**: 18% lower delay rate, suggesting schedule buffers.  

### Top Delayed Route Counts (≈5% of all delays)
- **LAX → DEN**: 206 delayed flights  
- **ORD → JFK**: 203 delayed flights  
- **DEN → LAX**: 201 delayed flights

---

## Recommendations

1. **Reschedule Peak Flights**: Move ~15% of 16:00–20:00 departures to off-peak slots.  
2. **Holiday Buffers**: Add extra turnaround time during holidays.  
3. **Weather Contingencies**: Increase schedule buffers by 20% for storm/snow forecasts.  
4. **Best-Practice Sharing**: Adopt Delta’s efficient operations strategies.  
5. **Airport-Specific Plans**: Tailor improvements for LAX, SFO, DEN, and DFW based on delay trends.

---

## Technical Details

- **Programming**: Python (Pandas, Scikit-learn, Matplotlib)  
- **Workflow**: Jupyter Notebooks for data prep, modeling, and visualization  
- **BI Tools**: SQL for data extraction; Tableau for dashboards

---

## Assumptions and Caveats

1. **Synthetic Data**: Simplified patterns may not reflect all real-world nuances.  
2. **Holiday Definition**: Binary indicator; regional differences not modeled.  
3. **Weather Categories**: Broad classifications may hide sub-type effects.



