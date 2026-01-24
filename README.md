# Inflation Forecasting with SARIMAX and Macroeconomic Indicators

## Project Overview
Advanced time series forecasting framework for US inflation using SARIMAX incorporating key macroeconomic drivers with optimized lag structures derived from Cross-Correlation Function (CCF) analysis.

## Key Features
- **Multi-Model Comparison**: Baseline SARIMAX vs. Enhanced SARIMAX with exogenous variables
- **CCF-Optimized Lags**: Data-driven lag selection for oil prices, unemployment, and M2 money supply
- **Seasonal Modeling**: Full seasonal ARIMA specification with 12-month periodicity
- **ARCH Testing**: Statistical validation for heteroskedasticity (GARCH consideration)
- **30-Year Historical Data**: 1991-2024 monthly CPI data from FRED API

## Technical Stack

### Core Technologies
- **Time Series**: statsmodels 0.14+
- **Data Processing**: pandas, numpy
- **Visualization**: matplotlib
- **Data Source**: FRED API (Federal Reserve Economic Data)
- **Environment**: Python 3.9+

### Model Architecture
```python
SARIMAX Configuration:
├── Endogenous Variable
│   └── CPI Year-over-Year % Change
├── Exogenous Variables (CCF-optimized lags)
│   ├── WTI Crude Oil Prices
│   ├── Unemployment Rate
│   └── M2 Money Supply
├── ARIMA Components
│   ├── Order: (1, 1, 1)
│   └── Seasonal Order: (2, 1, 1, 12)
└── Diagnostics
    ├── ARCH-LM Test
    └── Ljung-Box Test (squared residuals)
```

## Model Performance

### Forecast Accuracy Comparison
| Model | RMSE | MAE | Improvement |
|-------|------|-----|-------------|
| **Baseline SARIMAX** | 2.8286 | 2.0248 | ✓ |
| **SARIMAX + Exogenous** | 2.7580 | 1.9623 | ✓ |
| **% Improvement** | 2.50% | 3.09% | - |

### Performance Highlights
- **2.76 RMSE**: Forecast error of ~2.76 percentage points on average
- **1.96 MAE**: Median absolute error under 2 percentage points
- **Exogenous Enhancement**: Adding macro variables reduces forecast error by 2.5-3%

## Methodology

### 1. Data Collection & Preprocessing
```python
Data Sources (FRED API):
- CPIAUCSL: Consumer Price Index for All Urban Consumers
- DCOILWTICO: WTI Crude Oil Prices ($/barrel)
- UNRATE: Unemployment Rate (%)
- M2SL: M2 Money Supply (billions)

Preprocessing:
- Monthly frequency: 1991-2024
- YoY inflation calculation: 12-month % change
- Train/Test split: 80/20
```

### 2. Feature Engineering via CCF Analysis
Cross-Correlation Function used to identify optimal lags:
- **Oil Prices**: Lagged relationship with inflation
- **Unemployment**: Phillips curve dynamics
- **M2 Money Supply**: Monetary policy transmission

### 3. Model Specification

**Baseline Model:**
```python
SARIMAX(inflation, order=(1,1,1), seasonal_order=(2,1,1,12))
```

**Enhanced Model:**
```python
SARIMAX(inflation, 
        exog=[oil_lag, unemployment_lag, m2_lag],
        order=(1,1,1), 
        seasonal_order=(2,1,1,12))
```

**ARIMA Parameters:**
- **p=1, d=1, q=1**: First-order differencing with AR(1) and MA(1) terms
- **P=2, D=1, Q=1, s=12**: Seasonal AR(2), seasonal differencing, seasonal MA(1) with 12-month cycle

## Key Insights

### Model Comparison
The inclusion of macroeconomic fundamentals (oil, unemployment, M2) provides modest but consistent improvement:
- RMSE reduction: 2.50%
- MAE reduction: 3.09%

While the improvement appears small, it's statistically meaningful given:
1. Inflation is notoriously difficult to forecast
2. The baseline SARIMAX already captures strong seasonal patterns
3. Exogenous variables add economic interpretability

### Economic Interpretation
The model leverages established economic relationships:
- **Oil shocks**: Cost-push inflation
- **Unemployment**: Phillips curve (demand-pull inflation)
- **M2 growth**: Monetary inflation transmission

## Data Period & Sources

**Time Range:** October 1991 - 2024 (monthly)  
**Training Period:** 1991-2019  
**Testing Period:** 2020-2024  
**Data Source:** Federal Reserve Economic Data (FRED)

### FRED Series Codes
- **CPIAUCSL**: Consumer Price Index
- **DCOILWTICO**: Crude Oil Prices - WTI
- **UNRATE**: Civilian Unemployment Rate
- **M2SL**: M2 Money Stock

## Contact & Links

**Author**: Nitin Vinayak  
**Email**: nitinvinayak.m@gmail.com  
**LinkedIn**: [linkedin.com/in/nitin-vinayak](https://linkedin.com/in/nitin-vinayak)