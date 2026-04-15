# Multi-Source Stock Return Prediction and Portfolio Optimization

## Overview
This project develops a machine learning pipeline to predict daily stock returns for a selected Indian equity universe and construct an optimal portfolio strategy based on these predictions.

The system integrates multiple data sources, including market data, macroeconomic indicators, firm fundamentals, and financial news sentiment, to generate predictive signals. The framework is designed to operate under realistic financial constraints such as temporal consistency, absence of look-ahead bias, and robustness across assets. :contentReference[oaicite:0]{index=0}

---

## Problem Statement
The objective is to:
- Forecast daily returns (or direction) for a basket of Indian stocks:
  - RELIANCE
  - HDFCBANK
  - INFY
  - MM
  - BHARTIARTL
  - HUL
- Develop a portfolio allocation strategy using these forecasts
- Evaluate performance on a strictly out-of-sample test period

---

## Data Sources
The model incorporates diverse data sources:

- Market Data: OHLC prices, volume (Yahoo Finance)
- Technical Features: returns, volume changes, momentum indicators
- Fundamentals: EPS, revenue, profit margin (Perplexity Finance)
- Macroeconomic Variables: repo rate, CPI, yields (RBI)
- Market Indicators: NIFTY50 returns, sector returns, India VIX
- Capital Flows: FII/DII flows
- Currency Rates: USD/INR, EUR/INR, GBP/INR, JPY/INR
- Commodities: crude oil, gold returns
- Sentiment: news sentiment (GDELT + FinBERT)

---

## Methodology

### Data Preprocessing
- Removed features with more than 15% missing values
- Forward-filled remaining missing data where appropriate
- Computed log returns from adjusted close prices
- Applied winsorization (1% tails) to control outliers
- Ensured strict temporal alignment to avoid look-ahead bias
- All transformations were fit only on training data

### Feature Engineering
- Excluded raw price levels due to non-stationarity
- Constructed technical indicators such as:
  - Moving averages (SMA, EMA)
  - Momentum indicators (MACD, RSI)
  - Volatility measures (Bollinger Bands, ATR)
- Created lagged features and interaction terms
- Applied rolling standardization using past data only

### Feature Selection
A multi-method pipeline was used:
- Mutual Information (domain-based)
- Random Forest feature importance
- Recursive Feature Elimination (Ridge)
- SHAP values (LightGBM)

Final features were selected using majority voting across methods.

Two approaches were explored:
1. Broad feature pool with fundamentals included
2. Structured approach with always-keep variables and technical feature selection

---

## Models
The following models were trained for both regression and classification:

- LightGBM
- XGBoost
- Random Forest
- CatBoost

Training setup:
- 70/15/15 train-validation-test split
- Grid search hyperparameter tuning
- Early stopping for boosting models

Tasks:
- Regression: Predict residualized returns
- Classification: Predict direction of returns

---

## Evaluation Metrics
- Regression:
  - R²
  - MAE
  - RMSE
- Classification:
  - Accuracy
  - Directional accuracy
  - High-confidence accuracy

---

## Results

### Key Observations
- Predicting exact returns is difficult; R² values are generally low
- Boosting models (XGBoost, CatBoost) perform more consistently
- Classification (direction prediction) performs better than regression
- Performance varies across stocks, indicating varying signal strength

---

## Portfolio Construction

### Approach
- Monte Carlo simulation
- Mean-variance optimization
- Efficient frontier analysis

The tangency portfolio was selected based on maximum Information Ratio.

### Performance (Oct–Dec 2025)
- Annualized Return: 56.86%
- Annualized Volatility: 11.57%
- Information Ratio: 4.91
- Sharpe Ratio: 2.72
- Maximum Drawdown: -4.34%
- Directional Hit Ratio: 59.68%

### Insights
- Portfolio performance is significantly stronger than individual model metrics
- Allocation remains diversified with concentration in higher-signal stocks
- Strategy shows stable growth with controlled drawdowns

---

## Key Takeaways
- Multi-source data improves predictive capability but signals remain weak at daily frequency
- Directional prediction is more reliable than magnitude prediction
- Ensemble boosting models are best suited for this task
- Portfolio optimization effectively converts weak signals into strong financial outcomes

---

## Future Work
- Incorporate alternative data sources (e.g., options data, high-frequency signals)
- Improve feature engineering using deep learning or representation learning
- Explore advanced portfolio construction techniques (e.g., reinforcement learning)
- Enhance robustness with regime detection and adaptive models

---
