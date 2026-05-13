---
title: "Algorithm Trading — TWSE"
description: "Systematic daily trading strategies for the Taiwan Stock Exchange: signal generation, backtesting, live paper-trading, and a multi-broker execution dashboard."
date: 2026-01-01
period: "Jan 2026 – Jun 2026"
team: "MIRLab · 3-person team"
category: "Research"
github: "https://github.com/ntumirlab/stock-analysis"
stack: ["Python", "TWSE", "Quant", "FinLab", "Docker", "Dash", "Flask"]
---

## The System

A production-grade trading system built on FinLab's real-time financial data. Covers the full pipeline from strategy development to automated execution.

**Strategy layer**: Momentum-based and mean-reversion signals on daily OHLCV data. The TWSE is a retail-dominated market with different microstructure than US equities — many patterns from Western quant literature don't transfer cleanly, so we research Taiwan-specific signals.

**Execution**: Automated via crontab. Three cronjobs handle account data fetching, independent backtesting reports, and order execution via Yuanta Fugle API and Sinopac Shioaji API. Multi-user and multi-broker support configured through `config.yaml`.

**Dashboard**: Web UI (Dash + Bootstrap + Gunicorn) showing trade info, account balance charts, and monthly return visualizations. Deployed with Docker Compose.

**Currently**: Evaluating ML-based signal combination. Live paper-trading is running.

## Stack

FinLab · shioaji · ta-lib · pandas · Dash · Flask · Docker

*9 stars on GitHub · v3.1.0 released Jan 2026*
