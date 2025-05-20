## Question 1: Proportion Test for Sound Spikes

**Goal:** 95% CI for $p=\Pr[S1\_Sound>1]$, test $H_0:p=0.025$.

1. **Count spikes**:

   ```python
   spikes = (df["S1_Sound"] > 1).sum()
   n = len(df)
   p̂ = spikes / n
   ```
2. **Check normal approx**: ensure $n p_0\ge10$ and $n(1-p_0)\ge10$.
3. **95% CI** (Wald):

   $$
     \hat p \pm z_{0.975} \sqrt{\frac{\hat p(1-\hat p)}{n}}
   $$
4. **Z-test**:

   $$
     z = \frac{\hat p - 0.025}{\sqrt{0.025\,(1-0.025)/n}},\quad
     p\text{-value}=2(1-\Phi(|z|))
   $$

### Interpretation

* If $p$-value < 0.05, conclude $p\neq0.025$.
* Report CI, e.g. “We estimate $p=…$ with 95% CI $[…,…]$.”
* Confirm normal-approx holds (both $np_0$ and $n(1-p_0)\ge10$).

---

## Question 2: One-Sample Mean Test for Temperature

**Goal:** Is $\mu_{\rm temp}=25$°C?

1. **Compute sample mean & std**:

   ```python
   x̄ = df["S2_Temp"].mean()
   s = df["S2_Temp"].std(ddof=1)
   n = len(df)
   ```
2. **t-statistic**:

   $$
     t = \frac{x̄ - 25}{s/\sqrt{n}},\quad \text{df}=n-1
   $$
3. **p-value**: two-sided from Student’s t.
4. **95% CI**:

   $$
     x̄ \pm t_{0.975,\,n-1}\,\frac{s}{\sqrt{n}}
   $$

### Interpretation

* If $p<0.05$, mean differs from 25 °C.
* State CI, e.g. “95% CI for μ is \[…,…]°C.”

---

## Question 3: Correlation Occupancy ↔ CO₂

**Goal:** Pearson $r$ and significance.

1. **Pearson**:

   ```python
   from scipy.stats import pearsonr
   r, p = pearsonr(df["Room_Occupancy_Count"], df["S5_CO2"])
   ```
2. **Scatterplot**:

   ```python
   plt.scatter(df["Room_Occupancy_Count"], df["S5_CO2"])
   plt.xlabel("Occupancy"); plt.ylabel("CO2")
   ```

### Interpretation

* Report “$r=…$, $p$-value=…; indicates \[weak/moderate/strong] \[positive/negative] correlation.”
* Show scatter to visualize linear trend.

---

## Question 4: Correlation Ambient Temp ↔ CO₂ & CO₂ Slope

**Goal:** AvgTemp vs. S5\_CO2 and vs. S5\_CO2\_Slope.

1. **Compute avg\_temp**:

   ```python
   temps = df[["S1_Temp","S2_Temp","S3_Temp","S4_Temp"]]
   df["avg_temp"] = temps.mean(axis=1)
   ```
2. **Two Pearson tests**:

   ```python
   r1, p1 = pearsonr(df["avg_temp"], df["S5_CO2"])
   r2, p2 = pearsonr(df["avg_temp"], df["S5_CO2_Slope"])
   ```
3. (Optional) two small scatterplots.

### Interpretation

* “Avg temp correlates with CO₂ at $r_1=…$ ($p_1=…$); with CO₂ slope at $r_2=…$ ($p_2=…$).”

---

## Question 5: OLS Regression for CO₂ Prediction

**Goal:** Predict S5\_CO2 from temps, lights, and occupancy.

1. **Select predictors**:

   ```python
   X = df[["S1_Temp","S2_Temp","S3_Temp","S4_Temp",
           "S1_Light","S2_Light","S3_Light","S4_Light",
           "Room_Occupancy_Count"]]
   y = df["S5_CO2"]
   ```

2. **Fit OLS** (statsmodels):

   ```python
   import statsmodels.api as sm
   X2 = sm.add_constant(X)
   model = sm.OLS(y, X2).fit()
   print(model.summary())
   ```

3. **Key outputs**:

   * **R²**: overall fit
   * **Coefficients** with p-values

4. **Pred vs. Actual plot**:

   ```python
   ŷ = model.predict(X2)
   plt.scatter(y, ŷ); plt.xlabel("Actual CO2"); plt.ylabel("Predicted CO2")
   ```

### Interpretation

* Highlight the largest significant predictors (e.g. “each +1 °C in S2\_Temp ↦ +0.8 ppm CO₂, $p<0.01$”).
* Comment on R²: “Model explains \~X% of CO₂ variability.”

---

## Question 6: OLS Regression for Occupancy Prediction

**Goal:** Predict Room\_Occupancy\_Count from aggregates & sensors.

### 20% Steps

1. **Compute aggregates**:

   ```python
   df["avg_light"] = df[["S1_Light","S2_Light","S3_Light","S4_Light"]].mean(1)
   df["avg_sound"] = df[["S1_Sound","S2_Sound","S3_Sound","S4_Sound"]].mean(1)
   ```
2. **Fit OLS**:

   ```python
   X = df[["avg_temp","avg_light","avg_sound","S5_CO2","S6_PIR","S7_PIR"]]
   X2 = sm.add_constant(X)
   occ_model = sm.OLS(df["Room_Occupancy_Count"], X2).fit()
   print(occ_model.summary())
   ```
3. **Inspect**:

   * Significant predictors
   * R²
   * Warning if occupancy has low variance

### Interpretation

* Note which environmental factors best predict occupancy.
* If R² low or only one strong predictor, discuss “limited variation in occupancy counts may weaken model.”

---

## Putting It All Together

When you write up your report, emphasize:

* **Q1**: “Observed spike rate = … (95% CI: \[…,…]); normal approx OK; $p$-value=… → \[not] different from 2.5%.”
* **Q2**: “Mean = …°C (95% CI: \[…,…]); $t$-statistic=…, $p$=… → \[no] evidence mean ≠ 25°C.”
* **Q3–4**: Correlation coefficients with brief interpretation.
* **Q5**: R² and top 2–3 predictors—plot actual vs. predicted.
* **Q6**: R², significant predictors, note any occupancy data limitations.

