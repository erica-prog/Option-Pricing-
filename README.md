# Black-Scholes Options Pricing

## Process: 

Developed a Python-based options pricing engine implementing the Black-Scholes model to value European call and put options for Apple Inc. (AAPL). Retrieved real-time options data via **yfinance API**, extracting key parameters including stock price ($149.70), strike prices, time to expiration, risk-free rates (3.82%), and implied volatility. Implemented closed-form solutions for d1/d2 calculations and probability metrics (in-the-money likelihood) using scipy.stats normal distribution functions.

## Results:

- **Pricing accuracy:** Calculated theoretical call option price of $2.83 vs. market-traded price of $5.97 for Dec 2022 $160 strike, revealing potential mispricing or market sentiment factors
- **Market patterns:** Analyzed 67 option contracts across multiple expiration dates (2022-2025), identifying peak liquidity in near-term contracts (67 calls/puts in Jan 2023)
- **Volatility smile:** Discovered implied volatility ranging from 0.30 to 0.60+ across strike prices, exhibiting classic volatility skew pattern with higher IV for out-of-the-money options
- **Trading volume concentration:** Strike price distribution analysis revealed highest volume concentration at $300+ strike prices (250+ contracts), indicating bullish market sentiment

**Key Insight:** The 53% difference between Black-Scholes theoretical price and market price suggests either model assumptions (constant volatility, no dividends) don't capture real market dynamics, or the market is pricing in additional risk factors beyond the standard parameters.

![Option pricing presentation](https://github.com/erica-prog/Option-Pricing-/blob/main/distribtion%20of%20strike%20prices%20(Option%20Pricing).png)
