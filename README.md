

# Job Salary Prediction: EDA & Hypothesis Testing

An exploratory data analysis and statistical testing project on a dataset of 250,000 job records. The goal is to understand what drives salary and to test, with formal hypothesis tests, which features have a statistically significant relationship with it.

## Project Overview

The project has two parts:

- **Part A: Exploratory Data Analysis.** Visualizations of the target (`salary`), each feature on its own, and each feature against salary.
- **Part B: Hypothesis Testing.** Formal tests of the relationship between salary and every feature, a combined regression model, and a final ranking of the features.

Every test follows the same layout: state **H0 and H1**, check the assumptions, run the test, read the **p-value** (significance level 0.05), report an effect size, and write a plain-English conclusion.

## Dataset

**Size:** 250,000 rows, 10 columns (9 features + 1 target). No missing values and no duplicate rows.

| Column | Type | Description |
|---|---|---|
| `job_title` | Categorical | Job role (12 categories) |
| `experience_years` | Numeric (discrete) | Years of professional experience (0 to 20) |
| `education_level` | Categorical (ordinal) | High School, Diploma, Bachelor, Master, PhD |
| `skills_count` | Numeric (discrete) | Number of professional skills listed |
| `industry` | Categorical | Industry sector (10 categories) |
| `company_size` | Categorical (ordinal) | Startup, Small, Medium, Large, Enterprise |
| `location` | Categorical | Country or region of employment (10 categories, including Remote) |
| `remote_work` | Categorical | No, Hybrid, Yes |
| `certifications` | Numeric (discrete) | Number of certifications held (0 to 5) |
| `salary` | Numeric (continuous), **target** | Annual salary |

## Methods

### Part A: Visualization
- Univariate: histograms for salary, experience, skills and certifications; count plots for each categorical column.
- Bivariate: salary against each numeric and categorical feature.
- Multivariate and outlier checks.

### Part B: Hypothesis Testing

| Question | Comparison | Test used | Why |
|---|---|---|---|
| B1 | Numeric feature vs salary | Spearman correlation | Data is not normally distributed (Shapiro-Wilk p < 0.05) and features are discrete counts |
| B2 | Categorical feature vs salary | Kruskal-Wallis, then Dunn's test (Bonferroni) | Normality and equal-variance checks failed, so a rank-based test was used |
| B3.10 | All nine features together | Multiple linear regression (OLS) with F-test and Type II ANOVA | Handles a mix of numeric and categorical predictors |
| B3.11 | Ranking of features | Partial eta-squared from the OLS model | Puts numeric and categorical features on one comparable scale |

**Effect sizes:** Spearman's rho (B1), epsilon-squared (B2) and partial eta-squared (B3). They are reported alongside p-values because, with 250,000 rows, almost any difference is "significant".

**Model assumption checks (B3):** residual normality, equal variance (Breusch-Pagan), multicollinearity (VIF, highest 1.84) and linearity. Unequal variance was handled by re-running the model with robust (HC3) standard errors, and the conclusions did not change.

## Key Findings

**Numeric features (B1).** All three are significant, but only experience is a meaningful predictor.

| Feature | Spearman rho | p-value | Strength |
|---|---|---|---|
| experience_years | 0.438 | < 0.001 | Moderate |
| skills_count | 0.125 | < 0.001 | Weak |
| certifications | 0.072 | < 0.001 | Very weak |

**Categorical features (B2).**

| Feature | p-value | Effect size (eps2) | Result |
|---|---|---|---|
| location | < 0.001 | 0.310 | Largest differences (India lowest, USA highest) |
| job_title | < 0.001 | 0.159 | Data Analyst lowest, AI Engineer highest |
| company_size | < 0.001 | 0.155 | Pay rises from Startup to Enterprise |
| education_level | < 0.001 | 0.092 | Pay rises at every education step |
| remote_work | < 0.001 | 0.004 | Tiny effect; only "Yes" is higher |
| industry | **0.739** | 0.000 | **No significant difference** |

**Combined model (B3).** The nine features together explain about **96% of the variation in salary** (R-squared = 0.963, F-test p < 0.001). Every feature is significant except `industry` (p = 0.82).

**Feature ranking (strongest to weakest, by partial eta-squared):**

1. location (0.898)
2. experience_years (0.839)
3. company_size (0.819)
4. job_title (0.816)
5. education_level (0.724)
6. skills_count (0.301)
7. certifications (0.129)
8. remote_work (0.110)
9. industry (0.000, not significant)




## Tools and Libraries

Python, pandas, NumPy, Matplotlib, Seaborn, SciPy, statsmodels, scikit-posthocs, Jupyter Notebook.

## Author

**Juliet**
