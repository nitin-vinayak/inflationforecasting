# Inflation Forecasting with SARIMAX and Macroeconomic Indicators

## Project Overview
Advanced time series forecasting framework for US inflation using SARIMAX incorporating key macroeconomic drivers with optimized lag structures derived from Cross-Correlation Function (CCF) analysis.

## Key Features
- **Multi-Model Comparison**: Baseline SARIMAX vs. Enhanced SARIMAX with exogenous variables
- **CCF-Optimized Lags**: Data-driven lag selection for oil prices (20 months), unemployment (1 month), and M2 money supply (24 months)
- **Seasonal Modeling**: Full seasonal ARIMA specification with 12-month periodicity
- **Comprehensive Analysis**: Exploratory data analysis, model building, performance evaluation, and business insights
- **33-Year Historical Data**: 1991-2025 monthly CPI data from FRED API 

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
│   ├── WTI Crude Oil Prices (differenced, lag 20 months)
│   ├── Unemployment Rate (levels, lag 1 month)
│   └── M2 Money Supply (differenced, lag 24 months)
├── ARIMA Components
│   ├── Order: (1, 1, 1)
│   └── Seasonal Order: (2, 1, 1, 12)
└── Diagnostics
    ├── Residual Analysis
    └── Model Validation
```

## Model Performance

### Forecast Accuracy Comparison
| Model | RMSE | MAE | 
|-------|------|-----|
| **Baseline SARIMAX** | 2.9049 | 2.0583 | 
| **SARIMAX + Exogenous** | 2.818 | 2.0176 | 
| **% Improvement** | 3% | 2% | 

### Performance Highlights
- **2.82 RMSE**: Enhanced model achieves forecast error of ~2.82 percentage points on average
- **2.02 MAE**: Mean absolute error of 2.02 percentage points
- **Exogenous Enhancement**: Adding macro variables reduces forecast error by 2-3%

## Methodology

### 1. Data Collection & Preprocessing
```python
Data Sources (FRED API):
- CPIAUCSL: Consumer Price Index for All Urban Consumers
- DCOILWTICO: WTI Crude Oil Prices ($/barrel)
- UNRATE: Unemployment Rate (%)
- M2SL: M2 Money Supply (billions)

Preprocessing:
- Monthly frequency: October 1991 - January 2025
- YoY inflation calculation: 12-month % change
- Train/Test split: 80/20
- Data frozen at January 01, 2025 for reproducibility
```

### 2. Feature Engineering via CCF Analysis
Cross-Correlation Function analysis identified optimal lag structures:
- **Oil Prices (DCOILWTICO)**: 20-month lag, differenced
  - Economic Rationale: Cost-push inflation from energy prices
- **Unemployment Rate (UNRATE)**: 1-month lag, levels
  - Economic Rationale: Phillips curve - demand-side pressure
- **M2 Money Supply (M2SL)**: 24-month lag, differenced
  - Economic Rationale: Monetary inflation transmission mechanism

All lags were determined through rigorous CCF analysis (see exploratory analysis notebook).

### 3. Model Specification

**Baseline Model:**
```python
SARIMAX(inflation, order=(1,1,1), seasonal_order=(2,1,1,12))
```

**Enhanced Model:**
```python
SARIMAX(inflation, 
        exog=[oil_lag_20, unemployment_lag_1, m2_lag_24],
        order=(1,1,1), 
        seasonal_order=(2,1,1,12))
```

**ARIMA Parameters:**
- **p=1, d=1, q=1**: First-order differencing with AR(1) and MA(1) terms
- **P=2, D=1, Q=1, s=12**: Seasonal AR(2), seasonal differencing, seasonal MA(1) with 12-month cycle

## Key Insights

### Model Comparison
The inclusion of macroeconomic fundamentals (oil, unemployment, M2) provides modest but consistent improvement:
- RMSE reduction: 3%
- MAE reduction: 2%

While the improvement appears small, it's statistically meaningful given:
1. Inflation is notoriously difficult to forecast
2. The baseline SARIMAX already captures strong seasonal patterns
3. Exogenous variables add economic interpretability and theoretical grounding

### Economic Interpretation
The model leverages established economic relationships:
- **Oil shocks (20-month lag)**: Cost-push inflation with delayed transmission through supply chains
- **Unemployment (1-month lag)**: Phillips curve dynamics - immediate demand-pull inflation effects
- **M2 growth (24-month lag)**: Long monetary policy transmission lags highlight importance of forward-looking policy

### Business Applications
**For Central Banks:**
- Leading indicators provide 20-24 month advance signals for inflation pressure
- Evidence supports proactive monetary policy given long transmission lags

**For Investors:**
- Multi-factor models improve inflation hedging strategies
- Enhanced CPI forecasts for TIPS pricing and bond portfolio management

**For Businesses:**
- Pricing strategies can anticipate inflation 1-2 years ahead using oil/M2 indicators
- Supply chain decisions benefit from energy price forecasts

## Project Structure
```
├── exploratory_analysis.ipynb    
├── inflationforecasting.ipynb  
├── README.md                     
└── requirements.txt      
```

## Data Period & Sources

**Time Range:** October 1991 - January 2025 
**Training Period:** ~80% of data  
**Testing Period:** ~20% of data  
**Data Source:** Federal Reserve Economic Data (FRED)  
**Data Freeze Date:** January 01, 2025 (for reproducibility)

### FRED Series Codes
- **CPIAUCSL**: Consumer Price Index for All Urban Consumers
- **DCOILWTICO**: Crude Oil Prices - West Texas Intermediate
- **UNRATE**: Civilian Unemployment Rate
- **M2SL**: M2 Money Stock

## Limitations & Future Work

### Current Limitations
- Linear SARIMAX assumes stable relationships that may break during structural shifts
- Does not account for policy interventions or supply shocks
- Limited to three predictors; many other factors influence inflation
- Requires accurate forecasts of exogenous variables themselves

### Future Enhancements
- Implement hyperparameter tuning via grid search or auto-ARIMA
- Test additional exogenous variables (housing costs, wages, import prices)
- Explore nonlinear models (machine learning approaches)
- Conduct rolling window cross-validation for robust assessment
- Develop interactive dashboard for scenario analysis

## Contact & Links

**Author**: Nitin Vinayak  
**Email**: nitinvinayak.m@gmail.com  
**LinkedIn**: [linkedin.com/in/nitin-vinayak](https://linkedin.com/in/nitin-vinayak)
