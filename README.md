# Theoretical-predictive-model-of-bitcoin-price
Disclaimer: This project is strictly a theoretical and educational exercise. It was created to explore machine learning methods in time-series analysis and to understand market microstructure dynamics. It is not a production-ready trading system, nor does it constitute financial advice.

1. Project Overview
This project is a case study in Quantitative Finance. The primary objective was to build a robust data pipeline and a deep learning model for forecasting binary directional changes in the price of Bitcoin (BTC-USD) over a daily horizon. A critical component of this research was evaluating how real-world transaction costs (fees and slippage) impact the effectiveness of a theoretical trading strategy.

2. System Architecture & Tool Selection Justification
2.1. Data Processing (Polars)
Polars was chosen over the industry-standard Pandas library.

Justification: Utilizing a Rust-backed engine and lazy evaluation allows for significant memory and time optimization when calculating rolling features. This efficiency is critical in High-Frequency Trading (HFT) environments or when handling massive historical datasets.

2.2. Normalization (RobustScaler)
RobustScaler was implemented for scaling the input features.

Justification: Cryptocurrency time series are characterized by extreme volatility and fat-tailed distributions. RobustScaler, which relies on the median and the Interquartile Range (IQR), prevents the model from being dominated by extreme outliers. In contrast, standard Z-score normalization would severely flatten and distort the underlying market signal during flash crashes or spikes.

2.3. Predictive Model (Attention-LSTM)
A hybrid architecture combining Long Short-Term Memory (LSTM) layers with a Multi-Head Attention mechanism was deployed.

Decision: The problem was framed as binary classification (probability of price increase/decrease) rather than point price regression.

Logic: Performing regression on highly noisy financial time series almost inevitably leads to a "lagging" effect (the model merely repeats yesterday's price). The Attention mechanism allows the model to dynamically assign weights to significant events within a 30-day window, theoretically enabling better identification of trend reversal patterns than classic recurrent neural networks.

3. Backtesting Methodology
The implemented backtesting engine rigorously accounts for Fees and Slippage.

Cost Analysis: Introducing a total cost of 0.15% per transaction serves as a strict reality check. Most academic models that exhibit high Win Rates in frictionless environments quickly lose their profitability once market microstructure and execution costs are factored in.

4. Results & Error Analysis
Based on out-of-sample testing, the following conclusions were drawn:

Edge Erosion: The model achieves a theoretical accuracy of 52-54%. In a frictionless vacuum, this generates a positive return. However, once real-world execution costs are applied, this statistical edge is almost entirely eroded, resulting in a break-even or slightly negative equity curve.

Prediction Lag: Analysis of the prediction outputs reveals an autocorrelation of errors—the model correctly identifies the trend, but often with a one-period lag (t+1). In daily trading, this delay renders the signal operationally useless.

Systematic Risk (Drawdown): The model lacks mechanisms to react to exogenous events (e.g., macroeconomic news releases). Consequently, it suffers from severe capital drawdowns (Maximum Drawdown) during sudden market regime shifts.

5. Limitations & Project Scope
This project is an experimental sandbox for testing machine learning hypotheses in finance.

Data Limitations: The model operates exclusively on endogenous data (price, volume). It completely ignores on-chain metrics and Level 2 Order Book data, which severely limits its ability to forecast short-term liquidity voids.

Interpretability: Despite the integration of Attention layers, the model remains fundamentally a "black-box" structure. This makes it highly difficult to isolate specific decision-making factors during periods of extreme market volatility.

6. Engineering Conclusion
This project empirically demonstrates that building a model with high theoretical accuracy is strictly secondary to the complexities of transaction cost management and risk mitigation. The true value of this system lies in its high-performance Polars data pipeline and its analytical framework, which ruthlessly exposes how operational costs destroy theoretical Sharpe ratioskiej trafności teoretycznej jest wtórna wobec problemu zarządzania kosztami transakcyjnymi i ryzykiem.
