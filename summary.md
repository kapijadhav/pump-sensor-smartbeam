# Pump Sensor Data – Technical Challenge Summary (Kapil Jadhav)

## Objective
- Analyze Pump Sensor dataset and understand sensor behavior around failures.
- Identify top 3 sensors predicting failure (BROKEN).
- Build a classification model to predict `machine_status` (NORMAL / RECOVERING / BROKEN).

## Dataset Overview
- Rows: 220,320 | Columns: timestamp + sensor_00…sensor_51 + machine_status
- Class distribution: NORMAL = 205,836 | RECOVERING = 14,477 | BROKEN = 7
- Noted missing data: sensor_15 is fully missing and was dropped. Other sensors had partial missing values.

## Key Analysis & Insights
### 1) sensor_00 vs machine_status
- sensor_00 stays stable around ~2.4–2.5 during NORMAL for long periods.
- Around each BROKEN timestamp, sensor_00 shows a sharp drop / step-change to very low values.
- In some failure windows, sensor_00 slowly recovers back towards normal levels.

### 2) Correlation Heatmap
- Correlation heatmap shows clear clusters of highly correlated sensors.
- This indicates redundancy (some sensors are measuring related patterns).

## Top 3 Predictors of Failure
- Failure was defined as `machine_status == BROKEN`.
- Using XGBoost with imbalance weighting and permutation importance on failure windows,
  the top 3 sensors predicting failure are:
  **sensor_40, sensor_51, sensor_00**

## Machine Status Classification Model
- Model used: XGBoost multi-class classifier.
- Evaluation: confusion matrix + classification report on time windows around failure events.
- Result: model predicts NORMAL very well; RECOVERING has low recall; BROKEN is difficult to predict due to only 7 samples.
- Limitation: BROKEN events are extremely rare, so the model has very few examples to learn reliable failure patterns.

## Additional Notes / Hypothesis
- Failures appear strongly associated with sudden sensor_00 drops, suggesting abnormal operating mode.
- More BROKEN examples (or longer labeled pre-failure periods) would improve the ability to predict failures earlier and more accurately.
