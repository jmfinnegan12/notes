# Position Management Routine (PMR)
## Definition
A set of rules for systematically managing an open position.

## Usage
We can "attach" a PMR to a set of entry rules to create a full strategy
$$
\text{Entry rules} + \text{PMR} = \text{Strategy}
$$
**This modularizes strategy development.**

A few trivial PMR examples:
- stop loss
- trailing stop loss
- take profit
- timed exit

We can create and backtest more complex PMRs that incorporate combinations of these examples as well as directional signals we may derive from the entry rules themselves or elsewhere. 

## Rationale
Most systematic trading strategies deal with entry rules -- some data generates a signal to enter a trade. Entry rules generally do not make the best exit rules. 

Two simple entry rule only cases that are easily backtested:
- a strategy that can be long or short (always in the market)
- a strategy that can be long, short, or flat

What if we are still not satisfied with that? What if we want to use a stop loss? A trailing stop loss? What about adding to the position if it reaches a profit threshold? There are infinitely many sets of rules for managing a position 

Much of the quant literature deals with position management at the *portfolio* level. There is an abundance of research and well known quantitative frameworks (Kelly formula, Modern Portfolio Theory, various ML frameworks) that aim to answer the question:  
>1. If we have a portfolio of several strategies, how much of our total capital do we allocate to each of them, given what we know about their historical performance? 

Comparatively few quantitative frameworks deal with position management at the *strategy* level. I am not aware of any frameworks that aim to answer the question:  
>2. If we have an open position, how can we formulate a set of rules to maximize its expected value?

Question 2 comes after the entry signal. Entry signals do not always make the best exit signals, especially for strategies with skewed upside/downside risk profiles. We might want to cut losses, take profits, double down, etc. 

In the discretionary/speculative realm, "trading around a position" is a crucial component of returns. "Pyramiding" into a winning position is an excellent way to manage risk and maximize reward for winners. Conversely, stop losses are key for avoiding catastrophic losing positions. However, these concepts are difficult to translate into quantitative strategies. An algorithmic trader attempting to subjectively manage positions taken by automated strategies by watching charts and ticks on a screen all day and would be wildly impractical and suboptimal. Defining the position management routine entirely within the strategy is also impractical because we will end up reinventing the wheel for every new strategy. A modular design is the natural solution. The search for an answer to question 2 and a scalable way to avoid these impracticalities led me to the concept of Position Management Routines. 