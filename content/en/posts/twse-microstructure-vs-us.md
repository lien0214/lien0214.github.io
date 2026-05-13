---
title: "Why Western Quant Strategies Don't Transfer Cleanly to TWSE"
date: 2026-03-15
description: "Notes from building a backtesting engine for the Taiwan Stock Exchange. The market microstructure is genuinely different from US equities, and most quant literature doesn't acknowledge this."
tags:
  - quant
  - trading
  - TWSE
  - research
  - MIRLab
categories:
  - Research Notes
showToc: true
---

## Why This Matters

Most publicly available quant research — academic papers, blog posts, open-source backtesting libraries — is written for US equities. When our MIRLab group started working on TWSE strategies, I assumed the main adaptation would be getting the data. The actual main adaptation was understanding that several core assumptions about how markets work don't hold in Taiwan.

This post is notes on the structural differences I've encountered so far, and how they affect strategy design.

---

## The Biggest Structural Differences

### 1. Price Limits: ±10% Daily Bands

TWSE enforces a hard ±10% daily price limit on most stocks. Once a stock hits its limit, it can only trade at that price (or closer to prior close). This creates "limit up" and "limit down" events that have no US equivalent outside of circuit breakers.

**Implication for strategies:** Any strategy with a momentum signal needs to handle limit events carefully. If a stock is on limit-up, you can't necessarily buy it even if you want to — the order will sit in the queue. If your signal fires on limit-up price action, you might not get a fill.

More subtly: the existence of daily limits changes return distributions. The tails are capped. This means volatility-normalized momentum signals behave differently than on US data where extreme daily moves are possible.

### 2. T+2 Settlement With Pre-market Margin Requirements

TWSE settlement is T+2 (trade day + 2 business days). This is the same as the US. But the margin mechanics are different in ways that affect liquidity around corporate events.

**More importantly:** TWSE has a same-day trading (當沖, dang chong) mechanism that was expanded in 2020. Stocks eligible for same-day trading (a designation that changes over time) have significantly different intraday liquidity profiles than non-eligible stocks. If you're running a strategy that doesn't distinguish these, you're mixing two different liquidity regimes.

### 3. Retail-Dominated Order Flow

US equity markets (at least in large caps) have a significant fraction of order flow from institutional algorithms. TWSE is retail-heavy, especially in small and mid caps. Individual investors account for a large fraction of daily turnover.

**Implication:** Many patterns that "work" in US quant literature work because they exploit institutional behavior (e.g., end-of-month rebalancing flows, earnings announcement drift driven by analyst recommendation cascades). These flows don't exist in the same form on TWSE.

Conversely, TWSE shows stronger versions of behavioral finance patterns — herding, overreaction to momentum, mean reversion in small caps — because retail investors are more susceptible to these biases than institutional algorithms.

### 4. End-of-Day Call Auction

TWSE has a call auction in the final few minutes of trading (2:25–2:30 PM). Prices determined in this auction can differ significantly from the pre-auction continuous trading price.

**Implication for backtesting:** If your backtest uses "close" prices without knowing whether those closes were set in the continuous session or the auction, you may have unrealistic fills. The auction close can be 1–3% away from the pre-auction price on illiquid stocks.

### 5. Sector Composition

TWSE is semiconductor-heavy in a way that makes index-level signals different from US indices. TSMC alone is >20% of the index weight. A "Taiwan market" factor is heavily correlated with TSMC's performance, which is itself correlated with Apple's production cycle and US semiconductor export controls.

This means macro signals that work on broad indices in the US need to be rethought when applied to Taiwan.

---

## What We're Actually Building

Given all of this, our backtesting engine explicitly handles:

- **Limit events**: orders are rejected if they would execute at a limit price when the stock is locked. We log how often signals fire on locked stocks as a diagnostic.
- **Same-day trading eligibility**: we track the list of eligible stocks and separate the analysis for eligible vs. non-eligible.
- **Auction close flags**: we flag prices set in the call auction and exclude them from strategies that rely on intraday continuity.
- **Accurate transaction costs**: TWSE has a fixed 0.1425% broker commission plus a 0.3% securities transaction tax on sells. This is higher than US transaction costs for most retail strategies. Strategies with high turnover that work in the US often don't survive after TWSE transaction costs.

---

## Preliminary Signal Results

I won't share specific strategy performance yet (the work is ongoing and unpublished), but a few general observations from testing Western quant signals on TWSE daily data:

**Short-term (5-day) momentum**: weaker than expected, with more pronounced reversal. The retail herding effect creates initial momentum but then sharp reversals as the crowd exits simultaneously.

**Value factors (P/B, P/E)**: work in the expected direction but slowly. The holding periods needed to realize value factor returns on TWSE seem longer than in US literature (12–18 months vs. 6–12 months).

**Volatility scaling**: necessary and effective. Raw signal strength without volatility normalization produces unacceptably large drawdowns around major index moves (TSMC earnings, semiconductor news cycles).

**Earnings announcement drift**: present but smaller magnitude than US. The market seems to pre-price earnings more aggressively, possibly because institutional coverage of TWSE is thinner and earnings releases are less closely watched until release.

---

## What I'm Still Figuring Out

- How to handle the changing composition of the same-day trading eligible list without lookahead bias in the backtest
- Whether ML-based signal combination (gradient boosting on features) meaningfully outperforms equal-weight combination of the same features — early results are mixed
- How to think about capacity: strategies that work in backtest might not survive at meaningful size given TWSE daily volumes, especially in small caps

Will keep updating this as the project progresses.
