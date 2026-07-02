# Theoretical-predictive-model-of-bitcoin-price
Disclaimer: This project is strictly a theoretical and educational exercise. It was created to explore machine learning methods in time-series analysis and to understand market microstructure dynamics. It is not a production-ready trading system, nor does it constitute financial advice.


The primary objective was to build a robust data pipeline and a deep learning model for forecasting binary directional changes in the price of Bitcoin (BTC-USD) over a daily horizon. A critical component of this research was evaluating how real-world transaction costs (fees and slippage) impact the effectiveness of a theoretical trading strategy.

RobustScaler was implemented for scaling the input features.
Cryptocurrency time series are characterized by extreme volatility and fat-tailed distributions. 
RobustScaler, which relies on the median and the Interquartile Range (IQR), prevents the model from being dominated by extreme outliers. In contrast, standard Z-score normalization would severely flatten and distort the underlying market signal during flash crashes or spikes.

Predictive Model (Attention-LSTM)
A hybrid architecture combining Long Short-Term Memory (LSTM) layers with a Multi-Head Attention mechanism was deployed.

The problem was framed as binary classification (probability of price increase/decrease) rather than point price regression.

 Performing regression on highly noisy financial time series almost inevitably leads to a "lagging" effect (the model merely repeats yesterday's price). The Attention mechanism allows the model to dynamically assign weights to significant events within a 30-day window, theoretically enabling better identification of trend reversal patterns than classic recurrent neural networks.

3. Backtesting Methodology
The implemented backtesting engine rigorously accounts for Fees and Slippage.

Cost Analysis: Introducing a total cost of 0.15% per transaction serves as a strict reality check. Most academic models that exhibit high Win Rates in frictionless environments quickly lose their profitability once market microstructure and execution costs are factored in.

Edge Erosion: The model achieves a theoretical accuracy of 52-54%. In a frictionless vacuum, this generates a positive return. However, once real-world execution costs are applied, this statistical edge is almost entirely eroded, resulting in a break-even or slightly negative equity curve.

Prediction Lag: Analysis of the prediction outputs reveals an autocorrelation of errors—the model correctly identifies the trend, but often with a one-period lag (t+1). In daily trading, this delay renders the signal operationally useless.

Systematic Risk (Drawdown): The model lacks mechanisms to react to exogenous events (e.g., macroeconomic news releases). Consequently, it suffers from severe capital drawdowns (Maximum Drawdown) during sudden market regime shifts.

Data Limitations: The model operates exclusively on endogenous data (price, volume). It completely ignores on-chain metrics and Level 2 Order Book data, which severely limits its ability to forecast short-term liquidity voids.

Interpretability: Despite the integration of Attention layers, the model remains fundamentally a "black-box" structure. This makes it highly difficult to isolate specific decision-making factors during periods of extreme market volatility.



This project empirically demonstrates that building a model with high theoretical accuracy is strictly secondary to the complexities of transaction cost management and risk mitigation.
