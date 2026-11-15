[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Idan-A/backtest-that-disproves-itself/blob/main/notebook/project.ipynb)
# A backtest that tries to disprove itself
**Question:** can a Ridge regression that picks 3 of 6 ETFs (SPY, EFA, AGG, LQD, GLD, SHY) each month beat simple portfolios after trading costs?
**Method:** each month a pooled Ridge model (alpha = 1) is trained on the previous 60 months to predict next-month returns from 3-month momentum, 12-month momentum and 21-day volatility, and the top 3 ETFs get 1/3 each. Everything is walk-forward with 10 bp costs per unit traded, compared with 60/40 and equal weight, and tested against 200 random-pick placebos. All settings were fixed before seeing results; see [notebook/project.ipynb](notebook/project.ipynb).

| Dec 2010 – Oct 2026, after costs | CAGR | Volatility | Sharpe | Max drawdown | Annual turnover |
|---|---|---|---|---|---|
| Ridge top-3 | 8.4% | 9.9% | 0.86 | -21.5% | 3.83 |
| 60/40 (SPY/AGG) | 9.5% | 9.1% | 1.05 | -20.0% | 0.06 |
| Equal weight | 6.1% | 7.2% | 0.87 | -16.9% | 0.06 |

**Result:** the strategy does **not** beat 60/40; it ties equal weight on Sharpe and beats random picks (placebo p = 0.015).
**Limitations:** about 15 years and 6 ETFs, simplified costs (flat 10 bp, no drift between rebalances), no taxes, risk-free rate = 0, one design chosen by me (no multiple-testing correction).
I designed the method and used Claude Code to write the code, then checked it with the two checks in the notebook (future-poisoning and a hand-computed metrics test).
Run: `pip install -r requirements.txt`, then execute the notebook top to bottom (about 40 seconds).
