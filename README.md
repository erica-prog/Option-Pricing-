# Black-Scholes Options Pricing (LSE Data Science Society MT Project Showcase) 

## Process

Developed a Python-based options pricing engine implementing the Black-Scholes model to value European call and put options for Apple Inc. (AAPL). Retrieved real-time options data via **yfinance API**, extracting key parameters including stock price ($149.70), strike prices, time to expiration, risk-free rates (3.82%), and implied volatility (IV). Implemented closed-form solutions for d1/d2 calculations and probability metrics (in-the-money likelihood) using scipy.stats normal distribution functions.

## Black-Scholes Formula

$$
C = S_0 \cdot N(d_1) - K \cdot e^{-rT} \cdot N(d_2)
$$

### Put Option Price
$$
P = K \cdot e^{-rT} \cdot N(-d_2) - S_0 \cdot N(-d_1)
$$

$$
d_1 = \frac{\ln\left(\frac{S_0}{K}\right) + \left(r + \frac{\sigma^2}{2}\right)T}{\sigma \sqrt{T}}
$$

$$
d_2 = d_1 - \sigma \sqrt{T}
$$

Parameters:

- $$S_0$$ = Current stock price
- K = Strike price
- T = Time to expiration (in years)
- r = Risk-free interest rate
- $$\sigma$$ = Volatility (standard deviation of returns)
- N(x) = Cumulative distribution function of standard normal distribution

Probability Metrics:

$$
P(\text{Call ITM}) = N(d_2)
$$

$$
P(\text{Put ITM}) = 1 - N(d_2)
$$

## Data Collection

```
import yfinance as yf

# Fetch AAPL options data
aapl = yf.Ticker('AAPL')
expiration_dates = aapl.options  # Get all available expiration dates
opt = aapl.option_chain(date='2022-12-16')  # Retrieve specific contract
Output: Retrieved 67 call option contracts with strikes ranging from $50 to $290.
```
## Black-Scholes Implementation

```
class BlackScholes:
    def call_price(self, S, K, T, r, sigma):
        d1 = self._d1(S, K, T, r, sigma)
        d2 = self._d2(S, K, T, r, sigma)
        return norm.cdf(d1) * S - norm.cdf(d2) * K * np.exp(-r*T)
```

Key Methods:

`call_price()` - Calculate theoretical call option value
`put_price()` - Calculate theoretical put option value
`call_in_the_money()` - Probability of call finishing ITM
`put_in_the_money()` - Probability of put finishing ITM

## Parameter Extraction

```
S = 149.70                              # Current AAPL stock price
K = 160                                 # Strike price
T = 34/365                              # Time to expiration (34 days)
r = 0.03819                             # Risk-free rate (3.819%)
sigma = opt_calls['impliedVolatility'][26]  # Market implied volatility (35.03%)
```

## Theoretical Pricing

```
bs_model = BlackScholes()
theoretical_price = bs_model.call_price(S, K, T, r, sigma) # Output: $2.83
```

**Market Comparison:**

```
pythonmarket_price = opt_calls['lastPrice'][26] # Output: $5.97
```

Pricing Gap: 53% difference (Market price $3.14 higher than theoretical)

## Market Analysis
A. Options Volume Over Time
Analyzed call and put contract availability across all expiration dates (2022-2025):
- Peak liquidity: January 2023 (67 contracts)
- Long-term contracts: Steady at ~50 contracts through 2025
- Insight: Near-term expiries have higher liquidity

B. Strike Price Distribution
```
pythonplt.plot(opt_call1['strike'], opt_call1['volume'])
```

**Findings:**

- Highest volume at $300+ strikes (250+ contracts)
- Low activity at ATM strikes ($150 range)
- Suggests bullish market sentiment

C. Implied Volatility Smile

```
pythonplt.plot(opt_call1['strike'], opt_call1['impliedVolatility'])
```

**Observations:**
- IV ranges from 0.30 (ATM) to 0.60+ (deep OTM)
- Classic volatility smile pattern
- Higher uncertainty for extreme strikes

## Results

- **Pricing accuracy:** Calculated theoretical call option price of $2.83 vs. market-traded price of $5.97 for Dec 2022 $160 strike, revealing potential mispricing or market sentiment factors
- **Market patterns:** Analyzed 67 option contracts across multiple expiration dates (2022-2025), identifying peak liquidity in near-term contracts (67 calls/puts in Jan 2023)
- **Volatility smile:** Discovered implied volatility ranging from 0.30 to 0.60+ across strike prices, exhibiting classic volatility skew pattern with higher IV for out-of-the-money options
- **Trading volume concentration:** Strike price distribution analysis revealed highest volume concentration at $300+ strike prices (250+ contracts), indicating bullish market sentiment

**Key Insight:** The 53% difference between Black-Scholes theoretical price and market price suggests either model assumptions (constant volatility, no dividends) don't capture real market dynamics, or the market is pricing in additional risk factors beyond the standard parameters.

![Option pricing presentation](https://github.com/erica-prog/Option-Pricing-/blob/main/distribtion%20of%20strike%20prices%20(Option%20Pricing).png)
