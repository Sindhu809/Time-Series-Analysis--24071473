German Electricity Demand Forecasting

Time-series case study comparing statistical, feature-based and neural forecasting methods on German electricity demand.

**Student:** Sindhu Gaddam  
**Student number:** 24071473  
**Assessment:** Assignment 1 - Time Series Modelling Case Study

## Research question

Which forecasting method remains reliable when the forecast origin is fixed and the following 104 weeks are genuinely unknown?

The project compares simple benchmarks with SARIMA, SARIMAX, Random Forest and a recursive LSTM. Every model is evaluated over the same blind two-year test period, with seasonal naive used as the principal benchmark.

## Main result

Seasonal naive produced the best test accuracy. Random Forest was the strongest learned model but did not improve on the benchmark.

| Model | RMSE  | MAE  | MAPE | RMSE skill vs seasonal naive |
|---|---:|---:|---:|---:|
| Seasonal naive | **2,988.25** | **2,288.49** | **4.36%** | **0.00%** |
| Random Forest | 3,153.71 | 2,405.64 | 4.62% | -5.54% |
| Mean | 4,402.06 | 3,789.85 | 6.98% | -47.31% |
| Drift | 4,466.49 | 3,850.78 | 7.14% | -49.47% |
| Naive | 4,475.77 | 3,858.15 | 7.15% | -49.78% |
| SARIMAX | 7,013.83 | 5,879.84 | 11.11% | -134.71% |
| SARIMA | 8,901.24 | 7,636.99 | 14.36% | -197.87% |
| Recursive LSTM | 10,236.18 | 9,179.25 | 16.15% | -242.55% |

Negative skill means the model was less accurate than seasonal naive. Bias in the notebook is defined as `actual - forecast`; therefore, negative bias indicates overprediction.

## Data

The demand series is downloaded automatically from the [Open Power System Data time-series package](https://data.open-power-system-data.org/time_series/2020-10-06/). The analysis uses:

- timestamp: `utc_timestamp`
- response: `DE_load_actual_entsoe_transparency`
- period: 1 January 2015 to 1 October 2020
- raw frequency: hourly
- modelling frequency: Sunday-ending weekly mean
- completeness rule: retain weeks containing at least 98% of 168 expected hours

The notebook also obtains historical Berlin temperature from the [Open-Meteo archive API](https://archive-api.open-meteo.com/v1/archive) and constructs German holiday variables with the `holidays` Python package.

Raw data are intentionally excluded from Git because they are large and reproducibly downloadable. See [`data/README.md`](data/README.md).

## Methods

The executed notebook contains the complete analysis:

1. Download, validate and clean the hourly load data.
2. Aggregate hourly demand to daily and weekly means.
3. Explore time, intraday, weekday and annual patterns.
4. Test stationarity using ADF and KPSS.
5. inspect original and differenced series with ACF and PACF.
6. Perform additive seasonal decomposition.
7. Construct temperature and holiday covariates.
8. Create chronological train, validation and test partitions.
9. Fit mean, naive, drift and seasonal-naive benchmarks.
10. Tune SARIMA over 147 ordinary-order combinations.
11. Select SARIMAX covariates using an inner validation period.
12. Tune six recursively evaluated Random Forest configurations.
13. Tune three LSTM architectures and generate a recursive hourly forecast.
14. Compare every model using RMSE, MAE, MAPE, MASE, bias and benchmark-relative skill.

## Reproducibility and leakage controls

- The final 104 weeks are held out as a blind test period.
- Model selection and hyperparameter tuning use training/validation data only.
- Scalers are fitted on training partitions only.
- Demand lags and rolling features contain only information available before the target.
- Recursive forecasts append predictions rather than realised test demand.
- Random seeds are fixed at 42 where supported.
- Realised future temperature makes the reported SARIMAX forecast conditional; a production system would require origin-available weather forecasts.

## Installation

Python 3.10 or 3.11 is recommended. TensorFlow installation can vary by operating system; Google Colab is the simplest execution environment.

```bash
git clone https://github.com/Sindhu809/Time-Series-Analysis--24071473.git
cd Time-Series-Analysis--24071473

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Running the analysis

Start Jupyter from the repository root so the notebook writes to the expected `data/` and `reports/` directories:

```bash
jupyter lab notebooks/Sindhu.ipynb
```

Then select **Run All Cells**. A network connection is required during the first run to download electricity and weather data. The SARIMA search and recursive LSTM forecast can take considerable time.

For Google Colab, upload `notebooks/Sindhu.ipynb`, choose a TensorFlow-compatible runtime and run the notebook from the first cell in order.

## Key analytical conclusions

- The weekly series shows strong annual dependence near lag 52.
- The level series passes both ADF and KPSS, so the selected highly differenced SARIMA specification is interpreted critically.
- Weather and holiday covariates reduce SARIMAX RMSE by 21.20% relative to SARIMA but do not beat seasonal naive.
- Random Forest is a credible challenger, although recursive feature replacement produces accumulating errors.
- The LSTM learns short-run hourly structure but becomes unstable over 17,472 recursive steps.
- Seasonal naive is recommended for operational two-year weekly point forecasting, supported by bias monitoring and empirically calibrated uncertainty.

## Report

The final eight-page report is available at [`reports/Sindhu.pdf`](reports/Sindhu.pdf). It contains the figures, model comparison, answers to the required analysis questions, limitations, operational recommendation and future work.
