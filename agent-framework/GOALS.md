# Project Goals Backlog

> Add goals below as a numbered list. Each goal will be decomposed and executed by the agentic framework.
> Status: [ ] = pending | [~] = in progress | [x] = done | [!] = blocked

---

## Indian Stock Analyzer (`~/Projects/Indian-stock-analyzer`)

1. [x] Implement a trading bot with paper trading / simulation mode — mirrors SMA project's bot architecture, runs simulated trades on NSE stocks, tracks P&L, win rate, and portfolio performance — P1
2. [x] Explore all available free-tier data sources for Indian stocks (beyond Breeze API + yfinance) — scan current data fetching code, identify gaps in quality/coverage/latency, research alternatives (NSE official, Upstox free, Zerodha Kite free tier, Alpha Vantage, Twelve Data, Yahoo Finance, etc.), and create a prioritized plan to integrate the best options to improve data quality and live feed — P2

---

## Stock Market Analysis — SMA (`~/Projects/Stock-market analysis`)

1. [x] Diagnose why prediction accuracy is not improving — scan entire SMA project codebase, identify bottlenecks (model architecture, stale features, data quality, training loop, lack of feedback), then implement fixes including self-learning logic (online learning / retraining on recent predictions vs actuals) and adaptive feature weighting so the model improves over time automatically — P1

---

## Cross-Project / New

1. [x] Build AgentFlow — cross-platform (mobile + desktop) app to monitor agentic triggers, link Git repos, manage GOALS.md, and execute goals via UI — P1 | New project

---

## Completed
<!-- Move done goals here -->
