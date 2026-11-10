# What Drives Life Expectancy?

Exploratory analysis and predictive modelling of life expectancy across 193 countries from 2000 to 2015, using health, economic and social indicators.

Completed as part of my BComHons in Information Systems (Predictive Analysis module) at the University of Johannesburg, 2024.

![Life expectancy over time by continent](images/life_expectancy_by_continent.jpg)

## Question

Which health, economic and social factors explain the gap in life expectancy between countries, and how accurately can we predict it?

## Dataset

- **File:** `Life Expectancy Data.csv`
- **Size:** 2,938 rows × 22 columns (193 countries, 2000–2015)
- **Indicators:** adult mortality, infant and under-five deaths, alcohol, health expenditure, Hepatitis B, measles, polio and diphtheria immunisation, BMI, HIV/AIDS, GDP, population, thinness, income composition of resources, schooling
- **Source:** WHO and United Nations data, published on Kaggle as [Life Expectancy (WHO)](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who)

## Approach

1. **Cleaning:** renamed all 22 columns to consistent snake_case, checked for duplicates (none), visualised missing values with a heatmap (Hepatitis B had 553 gaps, population and GDP also had many), and imputed numeric gaps with column means.
2. **Feature engineering:** added a `continent` column with `pycountry` and `pycountry_convert` for regional comparisons.
3. **Exploratory analysis:** distributions and skewness, outlier boxplots, developed vs developing comparisons, top and bottom 10 countries, and trends over time by continent.
4. **Correlation analysis:** a correlation heatmap across all numeric indicators.
5. **Time series:** seasonal decomposition, an Augmented Dickey-Fuller stationarity test, Prophet and ARIMA forecasts.
6. **Machine learning:** label-encoded categoricals, scaled features, and compared four regressors on an 80/20 train/test split.

## Key findings

| Indicator | Correlation with life expectancy |
|---|---|
| Schooling | **+0.72** |
| Income composition of resources | **+0.69** |
| BMI | +0.56 |
| HIV/AIDS | **−0.56** |
| Adult mortality | **−0.70** |

- Developed countries cluster between 70 and 80 years; developing countries are far more spread out.
- Africa started the period below 55 years but improved the fastest. Europe stayed highest throughout.
- Japan had the highest average life expectancy (about 82.5 years) and Sierra Leone the lowest (about 46 years).

![Correlation heatmap](images/correlation_heatmap.jpg)

## Model results

| Model | Test RMSE (years) | Test R² |
|---|---|---|
| **Extra Trees Regressor** | **1.90** | **0.963** |
| XGBoost Regressor | 1.96 | 0.960 |
| Random Forest | 2.03 | 0.957 |
| Gradient Boosting Regressor | 2.30 | 0.945 |

![Model comparison](images/model_comparison_r2.jpg)

## Reflection and next steps

- XGBoost scored R² 0.999 on the training set against 0.960 on the test set, which points to some overfitting.
- The data has repeated yearly rows per country, so a random split lets the same country appear in both train and test. A **group split by country** would give a more honest score.
- Several columns are heavily skewed, so **median imputation** (or imputation within each country) would suit them better than the mean.
- The time-series steps treat the whole table as one series. Forecasting **per country** would make the ARIMA and Prophet results more meaningful.

## How to run

```bash
git clone https://github.com/NJNteso/life-expectancy-analysis.git
cd life-expectancy-analysis
pip install -r requirements.txt
jupyter notebook life_expectancy_analysis.ipynb
```

Note: the interactive Plotly charts don't render in GitHub's notebook preview. Run the notebook locally to see them.

## Tools

Python · pandas · NumPy · Matplotlib · seaborn · Plotly · statsmodels · Prophet · scikit-learn · XGBoost · pycountry

---

**Nyakallo Joy Nteso** · Data & Business Analyst · [GitHub](https://github.com/NJNteso)
