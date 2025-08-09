# Option Volatility & Pricing Notes

## Chapter 7: Risk Measurement I

### Outstanding Questions
1. **P98**: Is reduced forward price a result of group of traders with short positions, or one trader's short position causing his own reduced forward price?
   - **IMO**: Forward price reduced because of a huge short selling group in the market

2. **P100, Figure 7-2**: If domestic interest rate falls, shouldn't futures options calls value go up?

3. **P117**: What does the sentence mean?

4. **P125**: How can theoretical edge be a result of dynamic hedging? What if the underlying is monotonously increasing, the total delta is always positive at each interval, requiring selling underlying each time, resulting in negative adjustment PnL?

5. **What is implied Roll?**

### Key Concepts

#### Risk Types
- **Basis Risk**: Spot price of asset to be hedged - futures price of contract used
- **Borrowing Costs**: Long rate - short rate

#### Interest Rate Effect (Rho)

##### Stock-Type Settlement on Stock Options
**Call options have positive rho**:

**Interpretations**:
1. Increasing interest rate makes buying calls more desirable than buying stocks
2. The movement in forward price going up (affecting payoff distribution) has greater impact than the discount factor to the final payoff
3. Seller who sells the call needs to hedge by buying stocks; if interest rate is rising, the cost of hedging increases (borrowing at a higher rate)

##### Settlement Types Comparison
- **Futures-type settlement of futures options**: Option values insensitive to change in interest rates
- **Stock-type settlement of futures options**: Option values sensitive to change in interest rate
  - Increasing interest rates, while leaving forward price unchanged, will lower present value of options
  - However, increase in forward price will dominate the impact of reduced present value, therefore option price will increase

#### The Greeks

##### Gamma
- **Definition**: Option's curvature - rate of change in the delta as the underlying price changes
- **Delta change per point**: What is the meaning of one full point?
- **Long positions**: Long both put and call = long gamma (positive gamma)
- **Short positions**: Short both put and call = short gamma (negative gamma)
- **Visual**: Call above, put below, straight line = 0

**Average Delta Formula** (assuming gamma unchanged):
```
Average delta = (delta + delta + (s1 - s2) * gamma) / 2
```

**New Option Value**:
```
C_new = C + (s1 - s2) * Average Delta = C + (s1-s2) * delta + (s1-s2)² * gamma/2
```

##### Theta
- **Definition**: Time decay - a function of t, not T-t
- **Expression**: Negative dollar value expressed in unit of a day
- **Example**: -0.05 means the option will lose 0.05 in value if the underlying has no movement
- **Note**: Theta differentiates against t, not T-t

**Time to Expiry Effects**:
- 3 Months: -0.03
- 3 Weeks: -0.06
- 3 Days: -0.16

**Positive Theta**: Can occur if option is determined to be ITM
- Example: Call K = 60, S = 100, intrinsic value = 40
- Option might be worth PV of 40 = 39
- Tomorrow will be worth more than today's 39

**Premium Decay Logic**: 
Premium above intrinsic value exists because protected one side (call for downside) together with normal upside creates convexity. Longer time = higher possibility of price movement = greater convexity effect = higher premium.

##### Vega
- **Definition**: Change in theoretical value for each one percentage point change in volatility

##### Theoretical Edge
- **Positive edge**: Buy options at prices less than theoretical value OR sell at prices greater than theoretical value
- **Negative edge**: If inputs into model are correct, strategy will lose money in long run

### Risk Analysis

#### Delta Analysis
- **Positive/Negative delta**: Indication that trader expects underlying to move upward/downward

#### Gamma Analysis
- **Negative gamma**: 
  - Upward movement creates more negative delta → want price to go down
  - Both directions of underlying movement are undesirable
  - Good indication trader wants underlying to sit still or move very slowly

- **Positive gamma**: 
  - Upward movement creates positive delta → want price to go up

#### Speed Relationships
- **Delta**: Measure of directional risk
- **Gamma**: Measure of magnitude risk or speed of movement

| Gamma | Magnitude Preference | Speed Preference |
|-------|---------------------|------------------|
| Negative | Smaller magnitude | Slower movement |
| Positive | Larger magnitude | Faster movement |

**Example**: Position with delta = -10 and gamma = -28 wants slow downward move in underlying price.

#### Gamma-Theta Relationship
- **Negative gamma** often concurrent with **positive theta**
- Profit from time value when underlying movement direction/speed is desired
- **Important**: Gamma and theta almost always have opposite signs with high absolute value correlation

#### Vega-Gamma Relationship
- **Negative gamma** sometimes concurrent with **negative Vega**
- **Gamma**: Measure of preference for higher/lower realized volatility
- **Vega**: Measure of preference for higher/lower implied volatility

---

## Chapter 8: Dynamic Hedging

### Core Concepts

#### Adjustments
**Definition**: Trades made primarily to ensure position remains delta neutral

#### Dynamic Hedging Strategy
- Required to capture option's mispricing using theoretical pricing model
- Adjustments made at regular intervals (e.g., weekly)
- At end of each interval, delta recalculated using:
  - Current price of underlying
  - Time remaining to expiry
  - Constant interest rate and volatility (model assumption)

#### Hedging Mechanics

##### Call Hedging Process
- **Price rises**: Delta position becomes positive → forced to sell underlying
- **Price falls**: Delta position becomes negative → forced to buy underlying
- **Result**: Buy low, sell high → profit if price reverts during time to maturity

### Theoretical Framework

#### Expected Value Relationship
```
E(X) + E(Y) = E(X)
```
Where:
- X = discounted option payoff
- Y = PnL from hedge

#### Variance Reduction
```
Var(X+Y) = Var(X) + Var(Y) + 2Cov(X,Y)
```
Adjustments make Var(Y) smaller, thus reducing overall variance.

#### Continuous Hedge Portfolio
**Portfolio composition**: -delta × underlying stock + borrow/lend at risk-free rate + Call

**Portfolio value change**:
```
+theta × dt + 0.5 × gamma × (sigma × S)² × dt - q × delta × S × dt - (C - delta × S) × r × dt = 0
```

**Simplified (ignoring dividend and interest)**:
```
+theta × dt + 0.5 × gamma × (sigma × S)² × dt = 0
```

**Key insight**: Time value decay should equal gamma gain.

### Unhedged Amount Analysis

#### Definition
**Unhedged amount** = 0.5 × gamma × (dS)²

#### Random Process
As dS is random process: sigma × sqrt(dt) × z

#### Expected Value
```
E(0.5 × gamma × (dS)²) = 0.5 × gamma × (sigma × S)²
```

#### Volatility Impact
- **Higher realized volatility** than model input → greater profit than theoretical edge
- **Reason**: Higher volatility → increased unhedged amount → larger accumulated gain
- **Alternative interpretation**: Higher volatility → higher price reversion → more adjustment cash flow profit

### Replication Theory
- Dynamic hedging = short replication of the option
- Cost of replication = sum of all cash flows from dynamic hedging process
- **Present value of cash flows = option's theoretical value**
- **E(Cash Flows) = Theoretical Value**

---

## Chapter 9: Risk Measurement II

### Outstanding Questions
1. Intuitive explanation why call delta can be interpreted as probability to be ITM?
2. Why ITM options have less time value than ATM options? 
   - **IMO**: ITM call's time value results from protection from loss below exercise price. Such protection = OTM put value, which < ATM put value
3. Why isn't vega decay highest at delta ±50?
4. **P152**: 'Reducing time or volatility' or just reducing time only?

### Delta Analysis

#### ATM Forward
**Definition**: Forward price equals option exercise price

#### Volatility Effects on Delta
- **Increasing volatility**: 
  - Call ITM delta decreases
  - Call OTM delta increases
  - **IOW**: Delta moves closer to ±0.5
- **Decreasing volatility**: Delta moves toward 0,1 or -1,0
- **ATM forward delta**: Remains around 0.5 regardless of volatility

#### Time Effects on Delta
Similar to volatility effects on delta.

#### Second-Order Greeks
- **Vanna**: Sensitivity of delta to volatility change
- **Charm (Delta Decay)**: Sensitivity of delta to time passage
- **Implied Delta**: Delta resulting from using implied volatility

#### Vanna/Charm Characteristics
**Maximum absolute values** at:
- **Calls**: Delta 20, 80
- **Puts**: Delta -20, -80

**Near zero values** at:
- **Calls**: Delta 0, 50, 100  
- **Puts**: Delta 0, -50, -100

**Signs**:
- **Call delta 20**: Vanna/Charm positive
- **Call delta 80**: Vanna/Charm negative
- **Put delta -20**: Vanna/Charm negative
- **Put delta -80**: Vanna/Charm positive

#### Volatility/Time Relationships
- **Lower volatility** → increases absolute value of Vanna
- **Shorter time to expiration** → increases absolute value of Charm

### Theta Analysis

#### Basic Characteristics
- **Highest theta**: ATM options vs ITM/OTM options
- **Higher underlying price**: ATM option has greater theta than lower-priced ATM option

#### Volatility Relationship
- **Increasing volatility** → **increasing theta**
- **Reasoning**: Tomorrow's time value will decay more if volatility increases
- **ATM relationship**: Linear between theta and volatility

#### OTM Call vs OTM Put
Under lognormal assumption:
- **OTM call** (high exercise price) has **higher time premium** than **OTM put** (equidistant)
- **Higher time value** → **faster decay** → **higher theta**

#### ATM Valuation Formula (Arithmetic Motion)
```
ATM call/put option value = S × sqrt(T-t) × sigma × sqrt(1/(2×π))
```

### Vega Analysis

#### Basic Characteristics
- **Highest vega**: ATM options vs ITM/OTM options
- **Reasoning**: ATM options have most uncertainty (50/50 vs one-sided dominant)

#### Exercise Price Relationship
- **ATM option**: Higher exercise price → higher vega
- **Logic**: If volatility = 0, time value = 0; therefore higher time value → higher vega

#### Vanna Alternative Definition
**Mathematically identical**: Sensitivity of vega to underlying price change

#### Volatility Effects on Vega
- **ATM options**: Vega relatively constant with volatility changes
- **ITM/OTM options**: Increasing volatility pushes delta toward 50
  - Since ATM has highest vega, moving toward delta 50 increases vega
  - **Green line**: ATM, **Blue line**: OTM/ITM

#### Volga (Vomma)
**Definition**: Sensitivity of vega to volatility change
- **ATM**: Volga ≈ 0
- **Maximum**: Calls with delta 10, 90; Puts with delta -10, -90

#### Time Relationship
**Long-term options** > **Short-term options** for volatility sensitivity
- **Reasoning**: Per unit volatility increase, Gaussian distribution one-sigma change is greater for longer expiry

### Gamma Analysis

#### Fundamental Principle
**Gamma, theta, and vega are greatest when option is at the money**
- **Result**: ATM options most actively traded (desired characteristics)

#### Exercise Price Effects
- **ATM options**: Gamma declines at higher exercise prices
- **Reasoning**: $1 change for $50 stock > $1 change for $100 stock (percentage terms)
- **Absolute measurement**: Lower exercise prices have higher gamma

#### Speed
**Definition**: Sensitivity of gamma to underlying price change
- **Greatest around**: Delta 15/85 for calls, -15/-85 for puts

#### Time/Volatility Effects on Gamma
##### ATM Options
- **Time passes** → **gamma rises**
- **Volatility reduces** → **gamma rises**
- **Reasoning**: Gaussian distribution window shrinks → delta changes more swiftly

##### ITM/OTM Options
- **Volatility reduces** → **gamma falls**
- **Volatility increases** → **gamma rises**

#### Higher-Order Greeks
- **Color**: Sensitivity of gamma to time passage
  - **ATM**: Shorter time → higher gamma → **positive color**
  - **ITM/OTM**: Shorter time → lower gamma → **negative color**
- **Zomma**: Sensitivity of gamma to volatility change (similar to Color)

#### Gamma Options Risk
**ATM options close to expiration in low volatility environment** = one of riskiest trades

### Lambda (Elasticity)

#### Definition
**Percentage change in option value per percentage change in underlying price**
```
Lambda = (delta × S) / TV
```
Where TV = option's theoretical value

#### Alternative Names
- **Leverage value**
- **Elasticity**

#### Volatility/Time Effects
**Lambda increases** (absolute terms) when:
- **Volatility increases**
- **Time increases**
- **Reasoning**: Larger time value (denominator) reduces lambda

---

## Chapter 10: Introduction to Spreading

### Outstanding Questions
1. Under assumption of traditional theoretical pricing model, why does delta-neutral ratio spread with more options purchased than sold always result in credit?
2. Why does ratio spread with more buys than sells always have gamma > 0?

### Market Structure

#### Scalping Limitations
- **Options markets** rarely sufficiently liquid to support scalping
- **Scalpers**: Try to determine equilibrium price balancing buyers and sellers

#### Spreading Benefits
- Take advantage of theoretically mispriced options
- Reduce effects of short-term bad luck

### Spread Types

#### Intramarket Spreads
All contracts based on same underlying

#### Intermarket Spreads  
Require identifying price relationship between different financial instruments

**Example**: Buy 2-month futures, sell 4-month futures
- If theoretical 4-month futures price < market price → short the spread
- Risk: If actual spread widens → potential loss

#### Specialized Spreads
- **Crack Spread**: Crude oil futures vs refined product futures
- **Leg**: One side of spreading strategy
- **Naked Position**: Leg positions with remaining legs not executed

---

## Chapter 11: Volatility Spreads

### Characteristics of Volatility Spreads
1. **Approximately delta neutral**
2. **Sensitive to underlying price changes**
3. **Sensitive to implied volatility changes**
4. **Sensitive to time passage**

### Common Volatility Spreads

#### Straddle
**Components**: Call + Put (same exercise price and expiry)

**Types**:
- **Bull Straddle**: Net positive delta
- **Ratio Straddle**: Unequal calls/puts with net zero delta

**Long Straddle Characteristics**:
- **+Gamma**: Benefits from large moves
- **-Theta**: Loses value over time
- **+Vega**: Benefits from volatility increase

#### Strangle
**Components**: Call + Put (same expiry, different exercise prices)
- **Typical**: OTM options
- **ITM version**: Called "guts" (less actively traded)

#### Butterfly
**Components**: Three-sided spread with equally spaced exercise prices
- **Ratio**: Always 1×2×1
- **Long butterfly**: Buy outside exercises, sell inside exercise
- **Call butterfly**: Left wing ITM
- **Put butterfly**: Right wing ITM

**Delta Neutral Butterfly** (inside exercise ≈ ATM):
- **Long butterfly**: Acts like short straddle (-Gamma/+Theta/-Vega)
- **Short butterfly**: Acts like long straddle (+Gamma/-Theta/+Vega)

**Risk Characteristics**:
- **Reduced unlimited loss risk** vs short straddle
- **Larger position sizes** typically executed

**Put-Call Equivalence**:
If European options: March 90/100/110 call butterfly = March 90/100/110 put butterfly
- **Same payoff** at maturity
- **Arbitrage opportunity** if prices differ

#### Condor
**Components**: Four options (2 inside + 2 outside exercise prices)
- **Ratio**: Always 1×1×1×1
- **Same type and expiry**
- **Delta neutral**: When underlying midway between inside exercises
- **View**: Strangle with limited risk/reward

### Ratio Spreads

#### Definition
**Unequal numbers** of same-type options with same expiry
- **Typically delta neutral**
- **Directional bias**: Profit maximized/loss minimized in preferred direction

#### Types
- **Call Ratio Spread**: More calls purchased than sold (wants upward movement)
- **Put Ratio Spread**: More puts purchased than sold (wants downward movement)
- **Back Spread**: More options purchased than sold
- **Front Spread**: More options sold than purchased

#### Theoretical Pricing
**Delta-neutral ratio spread** (more purchased than sold) should result in **credit**
- **Reasoning**: Convexity effect (-25 delta put < half of -50 delta put)

#### Strategy View
**Ratio spread** = **Straddle** with limited upside/downside risk

### Christmas Trees/Ladders

#### Structure
- **Call Christmas Tree**: Buy/sell call at lower exercise + sell/buy one call each at two higher exercises
- **Put Christmas Tree**: Buy/sell put at higher exercise + sell/buy one put each at two lower exercises

#### Characteristics
**More options bought than sold**:
- Like **long strangle** (+Gamma/-Theta/+Vega)

**More options sold than bought**:
- Like **short strangle** (-Gamma/+Theta/-Vega)
- Wants underlying to sit still and/or implied volatility to decline

### Calendar Spreads (Time/Horizontal Spreads)

#### Basic Structure
**Opposing positions** in two same-type options with:
- **Same exercise price**
- **Different expiration dates**

#### Position Types
- **Long calendar**: Buy long-term, sell short-term (debit)
- **Short calendar**: Sell long-term, buy short-term (credit)

#### Long Calendar Characteristics
**Conflicting market conditions desired**:

1. **+Theta**: Short-term has larger theta than long-term
   - If underlying sits still → collect time value

2. **-Gamma**: Short-term has larger gamma than long-term  
   - If underlying moves far ITM/OTM → spread value decreases

3. **+Vega**: Long-term has larger vega than short-term
   - If implied volatility rises → long-term rises faster

**Ideal scenario**: Underlying price stable but implied volatility rising
- Example: Night before CEO announcement with no expected fortune-relevant information

### Time Butterflies

#### Structure
**Same exercise price, three different expiration dates**
- **Equal time intervals** between expirations
- **Same option type**

#### Long Time Butterfly
**Construction**: Buy short-term calendar spread + sell long-term calendar spread

**Reasoning**: Convexity of square root of time
- **ATM May + July** < **2 × June options**
- **Short-term ATM spread** worth more than **long-term ATM spread**
- **Long time butterfly** results in **debit**

#### Characteristics
- **-Gamma**: Due to concavity of decreasing gamma over time
- **+Theta**: Convexity of increasing theta  
- **+Vega**: Convexity of time for volatility sensitivity

### Interest Rate and Dividend Effects

#### Calendar Spread Sensitivities
**Call Calendar Spread** (March/June):
- **Interest rates rise**: June forward ↑ more than March → spread widens → **+Rho**
- **Interest rates fall**: Spread narrows

**Put Calendar Spread** (March/June):
- **Interest rates rise**: Spread narrows → **-Rho**
- **Interest rates fall**: Spread widens

#### Dividend Effects
**Opposite to interest rate effects**:
- **Increasing dividend**: Call calendar narrows, put calendar expands
- **Decreasing dividend**: Call calendar widens, put calendar narrows

### Diagonal Spreads
**Similar to calendar spreads** but with **different exercise prices**

### Trading Guidelines

#### Delta Considerations
Spread should **not** be considered volatility spread when:
- **Large positive/negative delta** makes directional considerations more important than volatility

#### Implied Volatility Strategy
- **Low implied volatility** (options underpriced): Look for **+Vega** spreads
- **High implied volatility** (options overpriced): Look for **-Vega** spreads

#### Calendar Spread Timing
- **Long calendars**: Profitable when implied volatility low but expected to rise
- **Short calendars**: Profitable when implied volatility high but expected to fall

---

## Chapter 12: Bull and Bear Spreads

### Outstanding Questions
1. **Figure 12-12**: When time passes, does vega increase for bull spread?

### Bull and Bear Ratio Spreads

#### Example Structures

##### Basic Bull Ratio Spread
```
Buy 1 June 100 Call (56)
Sell 2 June 110 Calls (28)
```
- **-Gamma**: When underlying rises, position tends toward negative delta

##### Extended Bull Ratio Spread  
```
Buy 2 June 100 Calls (56)
Sell 3 June 110 Calls (28)
```
- Initially bullish, but turns bearish as underlying rises
- **Volatility characteristics** eventually outweigh directional considerations

##### Delta Neutral Configuration
```
Buy 2 June 110 Calls (28)
Sell 1 June 100 Call (56)
```

##### Bullish Bias Reflection
```
Buy 3 June 110 Calls (28)
Sell 1 June 100 Call (56)
```

#### Time Decay Effects
**Without underlying movement**:
- **June 100 call delta** → rises
- **June 110 call delta** → declines  
- **Net delta shift**: Positive → negative (**+Charm**)

**Result**: As volatility declines, bullish bias reflection becomes bearish

### Bull and Bear Butterflies and Calendar Spreads

#### Strategy Selection Based on Market View

##### High Implied Volatility Environment
**Delta-neutral position** with underlying at 100:
- **Buy June 95/100/105 butterfly**

##### Bullish Sentiment with High Implied Volatility
**Choose butterfly** with inside exercise price above current price:
- **June 105/110/115 call butterfly** 
- **Net positive delta** due to concavity of OTM deltas
- **Risk**: If underlying moves to 120, butterfly inverts to negative delta

#### Calendar Spreads with Directional Bias

##### Bullish Calendar Spread
- **Exercise price above current underlying price**
- Example: Underlying at 100, **June/April 110 calendar spread**

##### Bearish Calendar Spread  
- **Exercise price below current underlying price**
- Example: Underlying at 100, **June/April 90 calendar spread**

#### Gamma Considerations
**Both long butterflies and calendar spreads have -Gamma**:
- If underlying moves through exercise price → **delta inverts**
- **Bullish sentiment**: +Delta with +Gamma > +Delta with -Gamma
- **Bearish sentiment**: -Delta with +Gamma > -Delta with -Gamma

### Credit/Debit/Vertical Spreads

#### Definition
- **One option purchased + one option sold**
- **Same type and expiration**
- **Different exercise prices only**

#### Usage
**Focus primarily on underlying direction**:
- **Delta maintains sign** (doesn't invert)

#### Directional Rules
- **Buy lower exercise + sell higher exercise** = **Bullish**
- **Buy higher exercise + sell lower exercise** = **Bearish**

**Reasoning**: Long calls and short puts both have positive delta

#### Examples
**Bull Spread Variations**:
- **Bull debit call spread**: Buy low strike call + sell high strike call
- **Bull credit put spread**: Sell high strike put + buy low strike put
- **Terminal payoff identical**

### Strategy Calibration

#### Delta Magnitude Selection
1. **Greater exercise price spread** = **greater delta**
   - 95/110 bull spread more bullish than 100/105 bull spread

2. **Strong directional view** = **far OTM/ITM options**
   - Lower delta but highly leveraged position possible
   - Execute many spreads for amplified exposure

#### Implied Volatility Considerations

##### Low Implied Volatility Environment
- **Focus on purchasing ATM options**
- **Prefer**: Call bull 100/105 over call bull 95/100
- **Reasoning**: 100/105 has +Gamma/-Theta, benefits from volatility increase

##### High Implied Volatility Environment  
- **Focus on selling ATM options**
- **Prefer**: Call bull 95/100 over call bull 100/105
- **Reasoning**: 95/100 has -Gamma/+Theta, benefits from volatility decrease

#### Cost-Benefit Analysis
**Call bull 100/105 spread**:
- **Lower cost** + **higher profit potential** than 95/100
- **Reason**: 95/100 profits in more scenarios
- **Risk**: Requires underlying movement for 100/105 profit

### Greeks Behavior in Bull Spreads

#### Greek Changes with Time/Volatility
**General pattern** (most Greeks increase in absolute value as T or volatility decrease):

**Exception**: **Theta decreases** when volatility decreases
- **All other Greeks increase** in absolute value

#### Intuitive Analysis
**Value vs Final Payoff**:
- **High volatility**: Maximum value fluctuation near exercise prices, no fluctuation at midpoint
- **Time passage**: Higher value decay near exercise prices, no decay at midpoint (still 2.5)

**Note**: Delta ≈ 0.5 interpretation doesn't apply for vega analysis

### Strategic Flexibility

#### Bull/Bear Spread Advantage
**Profit scenarios beyond expected direction**:
- **Bull spread**: Can profit if market fails to rise (sometimes even if falls)
- **Bear spread**: Can profit if market fails to fall (sometimes even if rises)

**Key insight**: Flexibility is major factor in dramatic growth of options markets

---

## Chapter 13: Risk Considerations

### Outstanding Questions
1. Does butterfly have positive gamma if underlying goes far up? **Yes**
2. **Figure 13-5**: Shouldn't theoretical P&L for spread 2 go negative?
3. Will gamma go to infinity when ATM?

### Risk Categories

#### Delta (Directional) Risk
**Definition**: Risk that underlying moves in one direction rather than another
- **Measure**: Delta position sensitivity to directional moves

#### Gamma (Curvature) Risk  
**Definition**: Risk of large move in underlying, regardless of direction
- **Measure**: Gamma position sensitivity to such moves
- **Note**: **Positive gamma** doesn't have gamma risk (increases value with movement)

#### Vega (Volatility) Risk
**Two forms**:
1. **Risk of incorrect realized volatility estimate** over strategy life
2. **Risk of implied volatility changes** in options market

### Risk Analysis Example

#### Scenario Setup
**Conclusion**: All options overpriced (based on 18% volatility estimate)
**Underlying**: 48.4

#### Strategy Comparison
```
Spread 1: Short Straddle    -15 May 48 Calls; -20 May 48 Puts
Spread 2: Short Ratio       +10 May 50 Calls; -20 May 52 Calls  
Spread 3: Long Butterfly    +10 May 46 Puts; -20 May 48 Puts; +10 May 50 Puts
```

#### Gamma Risk Analysis
**After matching theoretical edge** (quantities adjusted):

| Spread | Gamma | Risk Profile |
|---------|--------|--------------|
| Spread 1 | -406 | Highest gamma risk |
| Spread 2 | -165.5 | Lowest current gamma risk |
| Spread 3 | -370 | Moderate gamma risk |

**Important caveat**: Risk measures change with market conditions

#### Directional Risk Preferences
- **Large downward move concern**: Choose **Spread 2**
- **Unlimited loss unacceptable**: Choose **Spread 3**  
- **Large upward move concern**: Choose **Spread 3**

#### Vega Risk Analysis
**After matching theoretical edge**:

| Spread | Vega | Breakeven Volatility |
|---------|-------|---------------------|
| Spread 1 | -2.625 | 21% |
| Spread 2 | -0.875 | 24% |
| Spread 3 | -2.4 | 21.5% |

**Conclusion**: **Spread 2** has lowest volatility risk

#### High Volatility Scenarios (30-40%)
**Volga effects**: Positive volga helps position
- **Reduces loss rate** as volatility rises
- **Increases profit rate** as volatility falls

**Implied volatility risk**: **Spread 3** represents best value due to volga benefits

#### Extreme Volatility (approaching 0%)
- **Spread 2**: No profit/loss (both options worthless)
- **Issue**: Should show loss (transaction costs, etc.)

#### Risk Ranking Summary
**For short volatility strategies**:
1. **Spread 2** (ratio spread): Clear winner for risk management
2. **Spread 3** (butterfly): Better than straddle, good for extreme volatility
3. **Spread 1** (straddle): Highest risk across most scenarios

### Alternative Spreads

#### Calendar and Diagonal Spreads
```
Spread 4: Short Calendar     +85 May 48 Puts; -85 Jul 48 Puts
Spread 5: Diagonal Call      +100 May 52 Calls; -100 Jul 54 Calls  
Spread 6: Diagonal Ratio     +30 May 48 Puts; -80 Jul 44 Puts
```

**Greeks Summary**:

| Spread | Name | Gamma | Theta | Vega |
|---------|------|--------|--------|------|
| Spread 4 | Short calendar | +289 | -0.306 | -2.635 |
| Spread 5 | Diagonal call | +240 | -0.25 | -1.5 |
| Spread 6 | Diagonal ratio | -52 | +0.05 | -2.8 |

#### Volatility Effects
- **Spread 6**: Time value increases then drops with volatility reduction
- **Spread 5**: Increases to constant (net credit received) with volatility drop

#### Time Decay Patterns
- **Spread 4**: Continues losing value (long side more valuable near maturity)
- **Spread 5**: Initially diagonal, becomes negative gamma/positive theta over time
- **Spread 6**: Transitions to positive gamma, therefore negative theta

### Risk Management Guidelines

#### Simple Rules
**When time is limited for detailed analysis**:
**Straddles and strangles are riskiest of all spreads**

#### Margin of Error Principle
- **Small margin of error** → **Small trade size**
- **Large margin of error** → **Large trade size**

**Example**:
- Estimated vol: 25%, Implied vol: 23% → 2% margin → **small size**
- Estimated vol: 25%, Implied vol: 18% → 7% margin → **large size**

#### Efficiency Ratio
**For negative theta/positive gamma positions**:
```
Efficiency = Gamma / |Theta|
```
- **Gamma**: Reward
- **Theta**: Risk

### Adjustment Strategies

#### Delta Adjustment Options
**When position has negative delta**:

1. **Buy underlying**: No change to gamma/theta/vega risk
2. **Sell puts**: 
   - **Increases** theoretical edge
   - **Increases** gamma/theta/vega risk
   - **Higher loss** on significant market moves
3. **Buy calls**:
   - **Decreases** theoretical edge  
   - **Decreases** other risks besides delta

### Trading Styles

#### Positive Gamma Style
- **Adjusts against trend**
- **Market up**: Delta increases → sell underlying
- **Market down**: Delta decreases → buy underlying
- **Frequent adjustment** = **contrarian trading**

#### Negative Gamma Style  
- **Adjusts with trend**
- **Market up**: Delta turns negative → buy underlying
- **Market down**: Delta turns positive → sell underlying
- **Frequent adjustment** = **momentum trading**
- **Alternative**: **Positive gamma with minimal adjustment**

### Liquidity Considerations

#### Illiquid Options Warning
**Key question**: Are you willing to hold position until expiration?

**Reason**: Difficult to exit mistaken positions at fair prices in illiquid markets

**Conclusion**: **Illiquid options are dangerous to trade**

---

## Chapter 14: Synthetics

### Synthetic Underlying Positions

#### Long Synthetic Underlying
```
Long June 100 Call + Short June 100 Put
```
**At expiration**: Always results in buying underlying at 100
- **Above 100**: Exercise call (by choice)
- **Below 100**: Assigned on put (by force)

#### Short Synthetic Underlying
```
Short June 100 Call + Long June 100 Put  
```
**At expiration**: Always results in selling underlying at 100
- **Above 100**: Assigned on call (by force)
- **Below 100**: Exercise put (by choice)

#### Key Relationships
- **Synthetic long** ≡ **Long call + Short put**
- **Synthetic short** ≡ **Short call + Long put**

**Important**: These have underlying characteristics but don't become actual underlying until expiration.

#### Delta Relationship
**Same exercise price and expiration**:
```
|Call Delta| + |Put Delta| ≈ 100
```

### Synthetic Options

#### Basic Synthetic Options
- **Synthetic long call** ≡ **Long underlying + Long put**
- **Synthetic long put** ≡ **Short underlying + Long call**
- **Synthetic short call** ≡ **Short underlying + Short put**
- **Synthetic short put** ≡ **Long underlying + Short call**

#### Hedged Position Equivalence
**Single option + underlying hedge** = **Synthetic companion option**
- **Companion option**: Opposite type (call/put) at same exercise price

**Example**: Sell call + hedge = Sell synthetic put

#### Complete Synthetic Framework
**Six basic synthetic contracts**:
1. Long/short underlying
2. Long/short calls  
3. Long/short puts

### Greeks in Synthetic Relationships

#### Gamma and Vega
**Underlying contract**: Gamma = 0, Vega = 0
**Therefore**: **Companion calls and puts have identical gamma and vega**

**Volatility trading insight**: No distinction between calls and puts with same exercise/expiration
- Own call but prefer put? → Just sell underlying

#### Theta Considerations
**Original position ≠ synthetic** for theta (due to carry costs)

**Exception**: **Futures options with futures-type settlement**
- No carry costs → **companion calls and puts have identical theta**

### Spreading with Synthetics

#### Bull Spread Equivalence
**Bull call spread**: +1 June 100 Call / -1 June 105 Call (debit)
**Equivalent synthetic**: +1 June 100 Put / -1 June 105 Put (credit)

**Validation**: Long bull call + short bull put = guaranteed $5 at expiration
- **Current combined value** = **PV of $5**

### Advanced Synthetic Spreads

#### Iron Butterfly
**Definition**: Combines strangle and straddle (straddle centered in strangle)

**Traditional butterfly**:
```
+1 June 95 Call / -2 June 100 Calls / +1 June 105 Call (Debit: 1.75)
```

**Iron butterfly**:  
```
+1 June 95 Put / +1 June 105 Call / -1 June 100 Call / -1 June 100 Put (Credit: 3.25)
```

**Relationship**: Butterfly + Iron butterfly = exercise price spread
- **1.75 debit + 3.25 credit = 5.00** (with zero interest)

#### Iron Condor
**Definition**: Combines long strangle with short strangle (one centered in other)

**Traditional condor**:
```  
+1 June 90 Call / -1 June 95 Call / -1 June 105 Call / +1 June 110 Call
```

**Iron condor**:
```
+1 June 90 Put / +1 June 110 Call / -1 June 95 Put / -1 June 105 Call
```

**Relationship**: Condor + Iron condor = inside/outside exercise spread (e.g., 5)
- If condor trades at 3.75 → iron condor should trade at 1.25

---

## Chapter 15: Option Arbitrage

### Basic Arbitrage Strategies

#### Conversion and Reversal
- **Conversion**: Sell synthetic underlying + buy actual underlying
- **Reversal**: Buy synthetic underlying + sell actual underlying

#### Put-Call Parity Relationships

##### Futures Underlying (Futures-type Settlement)
**Effective interest rate = 0**:
```
C - P = F - X
```

##### Futures Underlying (Stock-type Settlement)
```
C - P = (F - X)/(1 + r×t)
```

##### Stock Underlying (Simplified Mental Math)
```
C - P = S - X + X×r×t - D
```
Where:
- **RHS**: P&L from buy/sell underlying
- **LHS**: Upfront payment now

**Usage**: Derive implied interest rate and dividend expectations

### Pin Risk

#### Definition
**Pinned**: Exercise price exactly equals underlying market price at expiration
**Pin Risk**: Uncertainty about assignment on one leg while other leg exercise timing is unclear

#### Characteristics
- **ATM call exercise**: Cheaper than buying underlying (lower transaction costs)
- **Risk reduction**: Use smaller contract sizes
- **Cash settlement**: Pin risk not applicable (e.g., stock indices)

### Settlement Risk

#### Stock-Type Options on Futures-Type Underlying
**Issue**: Different interest rate sensitivities create delta imbalance

**Example**: Long put + short call + long futures
- **Futures up**: Immediate credit with interest → **realized gain**
- **Put-call**: Unrealized loss → **net positive delta**

### Interest and Dividend Risk

#### Risk Characteristics
- **Conversion**: Net debit → **negative rho**, long underlying → **positive dividend risk**
- **Reversal**: Net credit → **positive rho**, short underlying → **negative dividend risk**

#### Risk Elimination: Three-Way Strategy
**Replace underlying** with deep ITM options:
- **Long underlying** → **Sell deep ITM put** or **buy deep ITM call**

### Boxes

#### Structure
**Long box example**:
```
+1 June 90 Call / -1 June 100 Call
-1 June 90 Put / +1 June 100 Put
```

**Interpretation**: Synthetically long at 90, synthetically short at 100

#### Valuation
**Expiration value**: Always 10 (exercise difference)
**Present value** (stock-type settlement): 10/(1 + r×t)

**Example**: If worth 9.8 by market rates
- **Sell below 9.8**: Borrowing at higher rate
- **Buy above 9.8**: Lending at lower rate

#### Alternative View (Horizontal)
**Bull call spread + bear put spread**
- If bull call = 6 → bear put should = 3.8
- Use for market making bear put spread

### Rolls

#### Long Roll Structure
**Short short-term synthetic + long long-term synthetic** (same exercise)
- **Conversion short-term + reversal long-term**
- **Economic**: Sell underlying at X (short expiry), buy underlying at X (long expiry)

#### Valuation
```
Roll value = X/(1+rs×ts) - X/(1+rl×tl) - D ≈ X×r×t - D
```
Where:
- **D**: Dividends between expirations
- **t**: Time between expirations

#### Characteristics
- **Fluctuates** with interest rates and dividends
- **Alternative view**: Long call calendar + short put calendar

### Time Boxes/Diagonal Rolls

#### Structure  
**Combine box with roll**
- Buy long-term synthetic at Xl + sell short-term synthetic at Xs

#### Valuation
```
Value = Xs/(1+rs×ts) - Xl/(1+rl×tl) - D
```

**Examples**:
- Buy August 90/100 box + sell June/August 90 roll
- Buy June 90/100 box + sell June/August 100 roll

### Synthetics in Volatility Trading

#### Straddle Alternative
**Instead of**: Buy C + P
**Alternative**: Buy P + P + S

**Reasoning**:
```
C = P + (S - X + S×r×t - D)/(1+r×t)
```

#### Butterfly Execution Choice
**Buy 45/50/55 butterfly**:
- **Put butterfly**: 1.3
- **Equivalent iron butterfly**: Credit of 4.95 - 1.3 = 3.65

**If market price** for iron butterfly = 3.7:
**Best execution**: Sell iron butterfly (better than direct butterfly purchase)

---

## Chapter 16: Early Exercise of American Options

### Outstanding Questions
1. Better explanation for: "Interest value must be approximate cost of carrying exercise price to expiration"
2. **P502**: What does "losing volatility value and interest value" mean?

### Basic Principle
**Early call exercise generally not desirable** when no dividends received from early holding

### Arbitrage Boundaries

#### European Call Lower Boundary
```
European Call ≥ (F - X)/(1 + r×t) ≈ S - X/(1 + r×t) - D
```

**Arbitrage capture**: Buy call + sell stock at S → equivalent cost F at expiry → repurchase at X via exercise

**Note**: Boundary can be positive even when S - X < 0

#### American Call Boundaries
```
American Call ≥ max(0, S - X, (F - X)/(1 + r×t))
```

**Additional boundary**: Early exercise for intrinsic value

#### European Put Lower Boundary  
```
European Put ≥ (X - F)/(1+r×t) ≈ X/(1 + r×t) + D - S
```

#### Time Effects on Boundaries
**Interest rate impact**:
- **r >> d**: High interest from selling stock → **call boundary rises**
- **r << d**: High dividend cost from short stock → **call boundary falls**

#### Maximum Boundaries
- **American put** ≤ X
- **European put** ≤ X/(1+r×t)  
- **American call** ≤ S
- **European call** ≤ F/(1 + r×t) ≈ S - D

### Early Exercise Analysis

#### Call Option Value Components
1. **Intrinsic value**: (S-X)⁺
2. **Volatility value**: Protection when stock falls below exercise price
3. **Interest-rate value**: Rises with interest rates
4. **Dividend value**: Falls with dividends (negative for calls)

#### Early Exercise Condition for Calls
**Exercise when**:
```
Dividend value ≥ Volatility value + Interest rate value
```

**Timing**: Only exercise day before dividend payment
- **Reasoning**: Lose one day of volatility/interest value to gain dividend

#### Put Option Value Components  
```
Put value = Intrinsic value + Volatility value - Interest value + Dividend value
```

#### Early Exercise Condition for Puts
**Exercise when**:
```
Interest value ≥ Dividend value + Volatility value
```

**Timing**: Exercise when at least **blackout period** away from ex-dividend
```
Blackout period = Dividend Paid / Interest Earned Per Day
```

### Options on Futures Early Exercise

#### Reasoning
**P307**: Earn intrinsic value through exercise, but why give up volatility value?

### American vs European Option Pricing

#### Volatility Effects
**Higher volatility** → **smaller American premium** over European
- **Reason**: Higher volatility value makes early exercise less attractive
- **Logic**: Hard to give up valuable protection by converting to underlying

#### ITM Effects
**Deep ITM scenarios**:
- **Calls**: Volatility value → 0, early exercise benefits dominate  
- **Puts**: Similar pattern for deep ITM puts

**Premium differences approach**:
- **Call**: 1 - (90×0.06×22/365) = 0.67
- **Put**: 110×0.06×21/365 = 0.38

---

## Chapter 17: Hedging with Options

### Position Definitions
- **Long position**: Benefits when price rises
- **Short position**: Benefits when price falls
  - Example: Short EUR for EUR-denominated payment due in 3 months

### Basic Hedge Types

#### Natural Positions
- **Natural long**: Inherent long exposure
- **Natural short**: Inherent short exposure

#### Protective Strategies
- **Protective call**: Purchase call to protect short position
- **Protective put**: Purchase put to protect long position

### Covered Strategies

#### Covered Write Strategies
- **Covered call**: Long underlying + Short call
- **Covered put**: Short underlying + Short put

#### Characteristics
**Covered call**:
- **Limits upside potential**
- **Provides premium cushion** when underlying falls
- **Credit provides limited protection** against adverse moves

**Covered put**:
- **Limits downside potential**  
- **Provides premium cushion** when underlying rises

#### Buy/Write Strategy
**Structure**: Buy stock + sell call simultaneously

**Outcomes**:
- **Stock rises**: Forced to sell at exercise price (target price achieved)
- **Stock falls**: Keep premium + stock (cushioned loss)

**Target setting**: Exercise price = target selling price
- **Either**: Sell at target price
- **Or**: Keep premium from call sale



