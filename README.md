<div align="center">

# 🏠 Seattle Housing Price Analysis
### What Drives Property Sale Prices? A Statistical Analysis of Seattle Real Estate (2014–2015)

<p>
  <img src="https://img.shields.io/badge/R-Statistical%20Analysis-276DC3?style=for-the-badge&logo=r&logoColor=white"/>
  <img src="https://img.shields.io/badge/RStudio-IDE-75AADB?style=for-the-badge&logo=rstudio&logoColor=white"/>
  <img src="https://img.shields.io/badge/Microsoft%20Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white"/>
  <img src="https://img.shields.io/badge/Dataset-~20k%20Records-orange?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge"/>
</p>

<p><i>A statistical deep-dive into Seattle, WA housing sales data — using ANOVA, Tukey's HSD, and regression analysis to identify which property attributes most significantly influence sale price.</i></p>
</div>

---

## Project Overview

Real estate agencies face a critical challenge: understanding which property features actually drive sale prices. This project analyzes approximately **20,000 housing sales records** from the Seattle, Washington area (May 2014 – May 2015) to identify the factors that most significantly influence property sale prices.

We focused on three main systems of influence:

- **Property age and renovation status** — including time elapsed since last renovation
- **Condition and grade ratings** — individually and in combination
- **Living-lot ratio** — interior living space relative to total lot size

Statistical methods used include **single-factor ANOVA**, **Tukey's HSD post-hoc testing**, **simple linear regression**, and **multiple regression analysis** — all implemented in R/RStudio.

> **Key Result:** Combined condition and grade ratings explain **52.89% of price variability** (adjusted R² = 0.5289), far outperforming any single-factor model.

---

## Research Questions & Hypotheses

| # | Comparison | Research Question | Null Hypothesis | Method |
|---|-----------|-------------------|-----------------|--------|
| **H1** | Grade & Price | Does a property's grade rating influence its sale price? | H₀: μ₁ = μ₂ = μ₃ (all grade means equal) | ANOVA + Tukey's HSD |
| **H2** | Condition & Price | Does a property's condition influence its sale price? | H₀: μ₁ = μ₂ = μ₃ (all condition means equal) | ANOVA + Tukey's HSD |
| **H3** | Living-Lot Ratio & Price | How much influence does the living-lot ratio have on sale price? | H₀: β₁ = 0 | Simple Linear Regression |
| **H4** | Renovation Age & Price | Does years since last renovation influence sale price? | H₀: β₁ = 0 | Simple Linear Regression |
| **H5** | Condition + Grade & Price | Do condition and grade *combined* influence sale price? | H₀: β₁ = β₂ = 0 | Multiple Regression |

---

## Dataset

**File:** `Spring_2025_16W_Project_Dataset.xlsx`
**Source:** Seattle, WA housing sales — May 2014 to May 2015
**Records:** ~20,000 property sales
**Sheets:** `Housing Data`, `Data Dictionary`

### Data Dictionary

| Column | Type | Description |
|--------|------|-------------|
| `id` | String | Unique ID for each home sold |
| `date.sold` | Date | Date of the home sale |
| `price` | Float | Sale price of the property |
| `bedrooms` | Integer | Number of bedrooms |
| `bathrooms` | Float | Number of bathrooms (0.5 = toilet only, no shower) |
| `sqft_living` | Integer | Square footage of interior living space |
| `sqft_lot` | Integer | Square footage of total land area |
| `floors` | Float | Number of floors |
| `waterfront` | Binary | `1` = waterfront view, `0` = no waterfront |
| `view` | Integer | View quality index (0–4) |
| `condition` | Integer | Property condition index (1–5) |
| `grade` | Integer | Construction & design grade (1–13); 1–3 = below standard, 7 = average, 11–13 = high quality |
| `sqft_above` | Integer | Square footage above ground level |
| `sqft_basement` | Integer | Square footage of basement |
| `yr_built` | Integer | Year the property was originally built |
| `yr_renovated` | Integer | Year of last renovation (`0` = never renovated) |
| `zipcode` | Integer | ZIP code of the property |

---

## Engineered Features

**File:** `dataset_Project.xlsx`

Additional columns derived for analysis:

| Feature | Description |
|---------|-------------|
| `building_age` | Age of property at time of sale (`year.sold − yr_built`) |
| `age_buckets` | Categorized age groups: Under 20 / 20–50 / More than 50 years |
| `yrs_since_reno` | Years elapsed since last renovation |
| `renovated` | Binary flag: `1` = renovated, `0` = never renovated |
| `has_basement` | Binary flag: `1` = has basement, `0` = no basement |
| `living_lot_ratio` | `sqft_living / sqft_lot` — interior space relative to lot size |
| `age_w_reno` | Combined age accounting for renovation status |
| `year.sold` | Year extracted from `date.sold` |
| `ymo.sold` | Year-month extracted for time-series grouping |

**Analytical sheets included:**

| Sheet | Content |
|-------|---------|
| `summary stats` | Descriptive statistics of all numeric variables |
| `price_by_grade` | Average price broken down by grade rating |
| `price_by_condition` | Average price broken down by condition rating |
| `price_by_condition_and_grade` | Combined condition × grade price analysis |
| `price_by_ratio` | Price analysis by living-lot ratio |
| `price_by_age` | Price analysis by property age bucket |
| `price_by_renovated_age` | Price analysis by years since renovation |

---

## Methodology

### 1. Exploratory Data Analysis (EDA)
- Descriptive statistics for all numeric variables
- Price distribution analysis (variance: 135,982,911,732 — highly dispersed)
- Property age distribution across six age brackets (0–120 years)
- Condition and grade frequency analysis
- Living-lot ratio outlier identification (townhomes vs. condominiums)

### 2. Single-Factor ANOVA
Used to test whether mean sale prices differ significantly across groups:

```
H₀: All group means are equal
Hₐ: Not all means are the same
Significance level: α = 0.05
```

### 3. Tukey's Honestly Significant Difference (HSD)
Post-hoc pairwise comparison to identify *which specific groups* have statistically different mean prices. Implemented in **R/RStudio** with 95% confidence intervals.

### 4. Simple Linear Regression
Tested relationships between a single explanatory variable and price:

```
Model: Price ~ β₀ + β₁(X) + ε
Evaluation: Adjusted R², correlation coefficient, p-value, residual plots
```

### 5. Multiple Regression Analysis
Combined condition and grade as co-predictors:

```
Model: Price ~ β₀ + β₁(Grade) + β₂(Condition) + ε
Evaluation: Adjusted R², individual p-values per category
```

---

## Results & Findings

### H1 — Grade & Price (ANOVA)

<p align="center">
  <img src="Images/Picture1.png" alt="ANOVA Grade Results" width="700"/>
  <br/><em>Table 1 — ANOVA results: Grade vs. Sale Price<img width="778" height="41" alt="image" src="https://github.com/user-attachments/assets/9efde7dd-fdac-422d-8829-16ef111013ca" />
</em>
</p>

| Metric | Value |
|--------|-------|
| F Statistic | **1937.97** |
| F Critical | 1.789 |
| p-value | ≈ 0 (< 0.05) |
| Decision | ✅ **Reject H₀** |

Grade has a **statistically significant influence** on sale price. Higher grade evaluations (9+) are associated with distinctly higher average sale prices.

---

### H1 — Tukey's HSD: Grade Pairings

<p align="center">
  <img src="Images/Picture2.png" alt="Tukey HSD Grade" width="700"/>
  <br/><em>Figure 1 — 95% Confidence Intervals for pairwise grade differences (R/RStudio)<img width="893" height="32" alt="image" src="https://github.com/user-attachments/assets/ec3ab60e-08d1-4804-90a3-288fdf4a711b" />
</em>
</p>

- Low-to-average grade pairings (≤ 9) tend to include **zero** in their confidence intervals → not statistically significant
- Pairings involving **grades 9+** have the largest, most distinct average price differences
- The higher the grade, the more pronounced its effect on price

---

### H2 — Condition & Price (ANOVA)

<p align="center">
  <img src="Images/Picture3.png" alt="ANOVA Condition Results" width="700"/>
  <br/><em>Table 2 — ANOVA results: Condition vs. Sale Price<img width="809" height="41" alt="image" src="https://github.com/user-attachments/assets/d41cc374-5620-41c2-a4e3-79bf4bd4d376" />
</em>
</p>

| Metric | Value |
|--------|-------|
| F Statistic | **35.146** |
| F Critical | 2.372 |
| p-value | ≈ 0 (< 0.05) |
| Decision | ✅ **Reject H₀** |

Condition rating has a **statistically significant influence** on sale price.

---

### H2 — Tukey's HSD: Condition Pairings

<p align="center">
  <img src="Images/Picture4.png" alt="Tukey HSD Condition" width="700"/>
  <br/><em>Figure 2 — 95% Confidence Intervals for pairwise condition differences (R/RStudio)<img width="913" height="32" alt="image" src="https://github.com/user-attachments/assets/a5a5d202-a6cb-4e3f-bed1-959400e67dc7" />
</em>
</p>

- Confidence intervals for pairings **2-1** and **4-1** include zero → differences are **not statistically significant**
- Lower condition ratings have **wider** confidence intervals (fewer observations at conditions 1 & 2)
- Better condition → higher price, most pronounced at condition ratings 4 and 5

---

### H3 — Living-Lot Ratio & Price (Regression)

<p align="center">
  <img src="Images/Picture5.png" alt="Living-Lot Regression" width="700"/>
  <br/><em>Table 3 — Regression results: Living-Lot Ratio vs. Sale Price<img width="835" height="41" alt="image" src="https://github.com/user-attachments/assets/c342e12a-8059-4427-b3c9-89da4adcef07" />
</em>
</p>
<p align="center">
  <img src="Images/Picture5.png" alt="Living-Lot Regression" width="700"/>
  <br/><em>Table 4 — Regression results: Living-Lot Ratio vs. Sale Price<img width="770" height="41" alt="image" src="https://github.com/user-attachments/assets/c1f33e4e-f1b8-4e07-b238-c272fa66ecab" />
</em>
</p>


| Metric | Value |
|--------|-------|
| Adjusted R² | **0.0132** (1.32% of variation explained) |
| Correlation | 0.1153 (positive, weak) |
| Decision | ❌ **Accept H₀** |

Living-lot ratio alone is **not a significant predictor** of sale price. However, residual patterns suggest it may contribute meaningfully in a multi-variable model.

---

### H4 — Renovation Age & Price (Regression)

<p align="center">
  <img src="Images/Picture7.png" alt="Renovation Regression" width="700"/>
  <br/><em>Table 5— Regression results: Renovation Age vs. Sale Price <img width="1071" height="41" alt="image" src="https://github.com/user-attachments/assets/04cb0f50-8804-4a06-ba5e-1750f8c3645c" />
</em>
</p>

| Metric | Value |
|--------|-------|
| Adjusted R² | **0.0074** (0.74% of variation explained) |
| Correlation | −0.0858 (negative, extremely weak) |
| p-value | 1.09E-34 |
| Decision | ❌ **Accept H₀** |

Years since renovation is **not a meaningful predictor** of sale price alone.

---

### H5 — Condition + Grade & Price (Multiple Regression)

<!-- Add screenshot: images/regression_multiple.png -->
<p align="center">
  <img src="Images/Picture8.png" alt="Multiple Regression" width="700"/>
  <br/><em>Table 6 — Multiple regression results: Condition + Grade vs. Sale Price <img width="846" height="41" alt="image" src="https://github.com/user-attachments/assets/5ef2a655-5503-4bc9-ba34-c24261a69b5c" />
</em>
</p>

| Metric | Value |
|--------|-------|
| Adjusted R² | **0.5289** (52.89% of variation explained) |
| Decision | ✅ **Reject H₀** |

The combined condition + grade model is **dramatically more powerful** than any single-factor model. Grade evaluations of **9 and above** and condition rating **5** are the most statistically significant predictors.

---

## Key Findings Summary

| Factor | Impact on Price | Statistical Significance |
|--------|----------------|--------------------------|
| **Grade** (single) | Strong positive effect — higher grade = higher price | ✅ Significant (F = 1937.97) |
| **Condition** (single) | Positive effect — better condition = higher price | ✅ Significant (F = 35.146) |
| **Living-Lot Ratio** | Very weak positive relationship | ❌ Not significant alone (R² = 0.013) |
| **Renovation Age** | Extremely weak negative relationship | ❌ Not significant alone (R² = 0.007) |
| **Grade + Condition** (combined) | Explains **52.89%** of price variation | ✅ Strongest model |

**Top actionable insights for real estate agents:**
- Prioritize **grade (9+) and condition (5)** when advising clients on pricing strategy
- Properties with **above-average grade ratings** command the largest price premiums
- Living-lot ratio and renovation age alone are poor price predictors — use them only as part of a broader model
- Future models should incorporate additional variables (location, waterfront, sqft) to push explanatory power beyond 52.89%

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| [R](https://www.r-project.org/) | Statistical analysis (ANOVA, Tukey's HSD, regression) |
| [RStudio](https://posit.co/products/open-source/rstudio/) | R development environment |
| [Microsoft Excel](https://www.microsoft.com/excel) | Data cleaning, feature engineering, and summary statistics |
| [ggplot2 (R)](https://ggplot2.tidyverse.org/) | Visualization of confidence intervals and regression plots |

---


<div align="center">
  <sub>ADTA 5130 — Data Analytics I | University of North Texas | Spring 2025</sub>
</div>
