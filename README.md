# Swap Option Volatility

An implied volatility is the volatility implied by the market price of an option based on an option pricing model. You can find volatility data Interest Rate Implied Volatility.

o construct a reliable volatility surface, it is necessarily to apply robust interpolation methods to a set of discrete volatility data. Arbitrage free conditions may be implicitly or explicitly embedded in the procedure. Typical approaches are

Local Volatility Model: a generalization of the Black-Scholes model.
Stochastic Volatility Models: such as SABR, Heston, Levy.
Parametric or Semi-Parametric Models: such as SVI model, Omega model, etc.
Market Volatility Model: directly modeling the implied volatility dynamics
Interpolation/Extrapolation Model: interpolating or extrapolating volatility data using specific function forms.

At FinPricing, we use the SABR model to construct swaption volatility surfaces following the best market practice.

Any volatility models must meet arbitrage free conditions. Typical arbitrage free conditions are

Static arbitrage free condition: Static arbitrage free condition makes it impossible to invest nothing today and receive positive return tomorrow.
Calendar arbitrage free condition: The cost of a calendar spread should be positive.
Vertical (spread) arbitrage free condition: The cost of a vertical spread should be positive.
Horizontal (butterfly) arbitrage free condition: The cost of a butterfly spread should be positive.
Vertical arbitrage free and horizontal arbitrage free conditions for swaption volatility surfaces depend on different strikes. There is no calendar arbitrage in swaption volatility surfaces as swaptions with different expiries and tenors have different underlying swaps and are associated with different indices. In other words, they can be treated independently.

The absence of triangular arbitrage condition is sufficient to exclude static arbitrages in swaption surfaces. Let Sw(t,T_s,T_e,K) be the present value of a swaption at time t, where T_s is the start date of the underlying interest rate swap; T_e is the end date of the underlying interest rate swap; K is the fixed rate of the underlying interest rate swap. The triangular arbitrage free conditions are

SABR stands for “stochastic alpha, beta, rho” referring to the parameters of the model. The SABR model is a stochastic volatility model for the evolution of the forward price of an asset, which attempts to capture the volatility smile/skew in derivative markets.

There is a closed-form approximation of the implied volatility of the SABR model. In the swaption volatility case, the underlying asset is the forward swap rate. The dynamics of the SABR model

The four model parameters (α, β, ρ, υ) in above have the following intuitive meaning:

α is the volatility that is tied closely to the at-the-money volatility value.
β is the exponent that is related to the backbone that the ATM volatility traces when the ATM forward rate varies.
ρ is the correlation that describes the “skew” or average slope of the volatility curve across strike.
υ is the volatility of volatility that describes the “smile” or curvature/convexity of the volatility curve across the strike for a given term and tenor.

For each term (expiry) and tenor of the swaption, conduct the following calibration procedure.

The β parameter is estimated first and typically chosen a priori according to how the market prices are to be observed.
Alternatively β can be estimated by a linear regression on a time series of ATM volatilities and of forward rates.
After β is set, we can obtain α by using σ_ATM to solve the following equation
The Viete method is used to solve this equation.
Given the α,β solved above, we can find the optimized value of (ρ,v) by minimizing the distance between the SABR model output volatilities and market volatilities across all strikes for each term and tenor.
The Levenberg-Marquardt least-squares optimization routine is used for optimization.
After (α, β, ρ, υ) calibrated, one can generate SABR volatility (swaption volatility) for any moneyness.
Repeat the above process for each term and tenor.

You can find interest rate data at
https://finpricing.com/lib/IrCurveIntroduction.html
