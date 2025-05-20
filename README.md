# 📊 Assignment on Statistical Analysis and Linear Modeling

**Dataset**: Room Occupancy Estimation
**Course**: Probability and Statistics
**Department**: Computer Engineering, UNDIP
**Date**: May 7, 2025

---

## 🧩 Introduction

This assignment involves analyzing the [Room Occupancy Estimation Dataset](https://www.kaggle.com/datasets/ananthr1/room-occupancy-estimation-data-set) using various statistical techniques:

* Hypothesis testing
* Interval estimation
* Correlation analysis
* Regression modeling

You will examine if observed sensor measures differ from expected values and explore predictors for environmental factors like **CO₂ levels** and **occupant counts**.

---

## 🔍 Research Questions and Instructions

### **Question 1: Proportion Test for Sound Spikes**

**Research Question:**
What is the 95% confidence interval for the proportion of intervals with a sound spike (`S1_Sound > 1`)?
Is the observed proportion significantly different from the hypothesized value of 2.5%?

**Instructions:**

* Define a sound spike as any interval where `S1_Sound > 1`.
* Calculate the sample proportion.
* Check if both $np_0$ and $n(1−p_0)$ are at least 10.
* Compute the 95% confidence interval.
* Test the hypothesis:

  $$
  H_0: p = 0.025 \quad vs. \quad H_1: p \ne 0.025
  $$

---

### **Question 2: One-Sample Mean Test for Room Temperature**

**Research Question:**
Is the mean room temperature (`S2_Temp`) significantly different from the target value of 25°C?

**Instructions:**

* Perform a one-sample t-test comparing the mean of `S2_Temp` to 25.
* Compute and interpret the 95% confidence interval.

---

### **Question 3: Correlation Between Occupancy and CO₂ Levels**

**Research Question:**
Is there a significant correlation between `Room_Occupancy_Count` and `S5_CO2`?

**Instructions:**

* Calculate the **Pearson correlation coefficient** and its p-value.
* Plot a **scatterplot** between occupancy and CO₂.

---

### **Question 4: Correlation with Ambient Temperature**

**Research Question:**
Do higher **ambient temperatures** (average of `S1_Temp` to `S4_Temp`) correlate with:

* Higher **CO₂ concentration** (`S5_CO2`)?
* Steeper **CO₂ slope** (`S5_CO2_Slope`)?

**Instructions:**

* Calculate `avg_temp` from the four sensors.
* Compute Pearson correlations between:

  * `avg_temp` and `S5_CO2`
  * `avg_temp` and `S5_CO2_Slope`

---

### **Question 5: Regression Model for CO₂ Prediction**

**Research Question:**
Can CO₂ concentration (`S5_CO2`) be predicted using:

* Temperature (`S1_Temp`–`S4_Temp`)
* Light (`S1_Light`–`S4_Light`)
* Room occupancy count?

**Instructions:**

* Build an OLS regression model using the listed predictors.
* Evaluate:

  * **R-squared**
  * **F-statistic**
  * **Coefficient p-values**
* Plot **actual vs. predicted** CO₂.

---

### **Question 6: Regression Model for Occupancy Prediction**

**Research Question:**
Can `Room_Occupancy_Count` be predicted using other sensor readings?

**Instructions:**

* Create aggregated features:

  * `avg_temp` (S1–S4)
  * `avg_light` (S1–S4)
  * `avg_sound` (S1–S4)
* Use predictors:
  `avg_temp`, `avg_light`, `avg_sound`, `S5_CO2`, `S6_PIR`, `S7_PIR`
* Fit an OLS regression to predict `Room_Occupancy_Count`.
* Interpret results and note any **limitations** (e.g., low occupancy variance).

---

## ✅ Conclusion Guidelines

For each question, conclude with:

* Significance of test or model
* Confidence intervals and interpretations
* Important variables
* Visual support (plots)
* Limitations if any

---

Let me know if you'd like this as a `.md` file or embedded into your Jupyter notebook too!
