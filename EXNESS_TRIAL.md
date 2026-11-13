# Aurum EMA trial

Magic number: **26111305**. The only approved live entry point is `exness_trial.py`, which uses the shared package in the sibling `Monn` checkout.

## Strategy profile

Original `aurum.strategies.trend.EMACrossover`: default 32/128 spans and volatility normalization, validated native artifact per symbol, M15. The rule is non-trainable; the native fit marks it ready without estimating a model. Enter only when absolute forecast is at least 0.25, with 2 ATR stop and 4 ATR target. MT5 bid bars are converted to mid for the Aurum signal. This is an explicit trial adapter, not the full portfolio, walk-forward selection, native OMS or Claude desk. No statistically demonstrated edge is claimed.

## Shared execution policy

Every live proposal goes through the same shared `Monn/exness_arena/guard.py`. Current fixed risk cap is 3.50 USC per position, 7.00 USC combined open stop-risk, 5.2444 USC cumulative booked loss budget per repo, and a 1% account-equity drawdown entry halt. A 10% execution buffer is reserved; volume never rounds up and remains capped at 0.01 cent lots. Missing state, stale tracker/quotes and ambiguous results fail closed. A first buy and first sell may coexist. Further same-direction entries require every existing position in that direction to be positive after swap and known opening fees. There is no numeric position-count or lifetime-entry cap. New opposite signals do not liquidate existing positions; server-side SL/TP remain active. Existing manual positions are excluded from management and per-repo results. Gaps can exceed estimated stop risk. Adding a repo never raises the shared budget or resets the baseline, history or loss counters.

## Validation and operations

`exness_trial.py --preflight` loads the original strategy and evaluates current broker data through a no-order gateway. Aurum artifact hashes are verified. The shared tests exercise ownership, sizing, hedging, profitable additions, failure handling and each new adapter. Preflight proves plumbing, not profitability. All rule-based trial profiles incur zero AI API costs.

Account pins, runtime settings, artifacts and the SQLite deal journal stay local in `Monn/.exness-arena/`, excluded from Git. The authoritative enabled repo list is `settings.json.enabled_bots`. `Monn/restart_exness_trial.ps1` checks and restarts this list while preserving open positions and history. Dashboard: http://127.0.0.1:8765 . Stop new entries with `Monn/stop_exness_trial.ps1`.
