# Time-Series-Analysis--24071473
This assignment is about time Series forecasting on German electricity data

## About This Project

This project is for the ADS1 time series assignment. The main aim is to forecast German electricity demand using different time series and machine learning models.

The project uses German electricity load data from Open Power System Data. The original data is hourly, but most of the models in this project use weekly average electricity demand. Temperature data for Berlin is also used as an extra variable to check whether weather helps improve the forecast.

## Project Aim

The aim of this work is to understand the pattern of German electricity demand and compare different forecasting models.

The project looks at these main points:

- Preparing the electricity demand data
- Creating weekly and hourly time series
- Checking trend and seasonality
- Testing stationarity using ADF, KPSS, ACF and PACF
- Building simple benchmark forecasts
- Building SARIMA and SARIMAX models
- Adding temperature and holiday variables
- Building a Random Forest regression model
- Building an LSTM model using hourly data
- Comparing all models using RMSE, MAE and MAPE

## Files in This Repository

**SINDHU GADDAM**

This is the main notebook file. It contains the full Python code for data cleaning, EDA, stationarity tests, forecasting models, plots and evaluation metrics.

**SINDHU GADDAM.IPYNB** 

This has the whole coding part of the assignment.

**time_series_60min_singleindex_filtered-2(7).csv**

This is the electricity demand dataset used in the project.

**README.md**

This file explains the project, methods, files and results.

## Dataset

The dataset is from Open Power System Data. It contains hourly electricity data for different European countries.

For this project, only the German electricity demand column was used:

DE_load_actual_entsoe_transparency

The data was filtered from January 2015 to October 2020. The hourly data was converted into weekly values for the benchmark, SARIMA, SARIMAX and Random Forest models. The LSTM model used hourly data.

## Work Done in the Notebook

## 1. Data Preparation

The dataset was loaded using pandas. The timestamp column was converted into datetime format and set as the index. The German electricity demand column was selected and renamed to make the analysis easier.

The hourly data was also converted into weekly average demand for most of the forecasting models.

## 2. Exploratory Data Analysis

Initial plots were created to understand how German electricity demand changes over time. The plots show clear yearly seasonality in the electricity demand series.

## 3. Stationarity Testing

Stationarity was checked using:

- ADF test
- KPSS test
- ACF plot
- PACF plot
- Differencing checks

These tests helped in deciding the SARIMA and SARIMAX model settings.

## 4. Benchmark Forecasts

The following benchmark models were used:

- Mean Forecast
- Naive Forecast
- Seasonal Naive Forecast
- Drift Forecast

The Seasonal Naive model gave the best benchmark result. Because of this, it was used as the main baseline for comparing the other models.

## 5. SARIMA Model

SARIMA was used because the weekly electricity demand data has seasonal behaviour. A grid search was done for different values of p, d and q. The best AIC model was tested, but it did not give the best forecast result. So, forecast accuracy was also considered when selecting the final SARIMA model.

The final SARIMA model performed better than the Seasonal Naive benchmark.

## 6. SARIMAX Model

SARIMAX was used to add temperature as an external variable. Berlin temperature was used as a representative temperature for Germany.

The SARIMAX model showed that temperature has some explanatory value, but it did not improve much compared with the SARIMA model.

The temperature forecast is treated as a conditional forecast because actual future temperature is not normally known at the forecast origin.

## 7. SARIMAX with Holidays

Holiday variables were also tested. Holidays are useful because they are known in advance, so they can be safely used for forecasting.

Adding holidays improved the SARIMAX result.

## 8. Random Forest Model

A Random Forest regression model was used as the feature-based model. It used features such as:

- Weekly temperature
- Load lag 1
- Load lag 2
- Load lag 3
- Load lag 52
- Week of year
- Month

The lag features were created carefully so that future values were not used in the training data.

Random Forest performed better than the weekly statistical models.

## 9. LSTM Model

An LSTM model was built using hourly electricity demand data. This model gave the lowest error overall.

However, LSTM is more complex and harder to explain than SARIMA and Random Forest. It also needs more computation and tuning.

## Model Results

| Model | RMSE | MAE | MAPE |
| Seasonal Naive | 3006.76 | 2318.52 | 4.41 |
| SARIMA | 2788.72 | 2149.28 | 4.09 |
| SARIMAX | 2847.08 | 2155.22 | 4.12 |
| SARIMAX + Holidays | 2670.12 | 1955.78 | 3.75 |
| Random Forest | 2475.07 | 1789.35 | 3.44 |
| LSTM | 1166.40 | 893.31 | 1.66 |

## Main Findings

The Seasonal Naive model was a strong benchmark because the electricity demand data has clear yearly seasonality.

SARIMA improved the forecast compared with Seasonal Naive. SARIMAX with temperature did not improve much compared with SARIMA, but SARIMAX with holidays gave a better result.

Random Forest gave better results than the statistical weekly models because it used lag features and calendar features.

LSTM gave the best accuracy overall, but it used hourly data and is more complex to understand and maintain.

## Best Model for Use

For real use, Random Forest is a good balanced model because it gives better accuracy than SARIMA and SARIMAX and is easier to maintain than LSTM.

LSTM is best if only accuracy is important, but it is harder to explain. SARIMA is still useful because it is more interpretable and gives confidence intervals.

## How to Run

1. Download the repository.
2. Keep the dataset file in the same folder as the notebook.
3. Open the notebook file named **SINDHU GADDAM**.
4. Run the notebook cells from top to bottom.
5. The notebook will generate the plots, model results and evaluation metrics.

## Libraries Used

The main Python libraries used are:

- pandas
- numpy
- matplotlib
- statsmodels
- scikit-learn
- tensorflow / keras

## Important Notes

- The weekly models use the last 104 weeks as the test period.
- The LSTM model uses hourly data.
- Temperature values in the test period should be treated carefully because actual future temperature is not available in a real forecast.
- Holiday variables are safe to use because holidays are known in advance.
- The results in the report should match the values produced by the notebook.

## Conclusion

This project shows that German electricity demand has strong seasonal patterns. Simple models are useful as a baseline, but advanced models can improve the forecast. Random Forest gives a good balance between accuracy and simplicity, while LSTM gives the best accuracy but is more complex.
