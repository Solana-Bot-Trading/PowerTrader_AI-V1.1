# PowerTrader AI V1.4 — Fork

Fully automated crypto trading powered by a custom price prediction AI and a structured/tiered DCA system, enhanced with small account optimization, risk management, and IRS tax export.

**Forked from:** [garagesteve1155/PowerTrader_AI](https://github.com/garagesteve1155/PowerTrader_AI)

---

## What This Fork Adds

This fork builds on garagesteve's excellent foundation with features specifically designed for small account profitability, capital preservation, and tax compliance. All changes are backward compatible — set `account_mode: "force_existing"` to revert to 100% original behavior.

### Small Account Optimization (accounts under $2,500)
- **Auto-detection** of account size with appropriate settings applied automatically
- **10% position sizing** instead of 0.005% (turns $0.50 trades into $25 trades)
- **Linear DCA** (equal-size buys) instead of exponential (2x multiplier)
- **3 wider DCA levels** (-7.5%, -15%, -25%) instead of 7 aggressive levels
- **Max 3 concurrent positions** to prevent correlation overload
- **20% cash reserve** enforced to maintain dry powder

### Risk Management
- **Hard stop loss at -35%** with one-time neural L7 override before forced exit
- **Tiered profit taking** — sell 33% at +7%, 50% of remainder at +15%, trail the rest
- **Neural DCA confirmation** required before executing DCA entries on small accounts

### Enhanced GUI
- **Win rate %**, average win/loss, best/worst coin, and hard stop count in the Account box
- **Manual Buy panel** — place market buys from the Hub with auto coin-add and optional auto-train
- **Manual Sell panel** — select a held coin and sell 100% from the Hub
- **Tax Export panel** — one-click IRS Form 8949 CSV export for any tax year

### Data Accuracy
- **Paginated order fetching** — follows Robinhood's pagination up to 25 pages per coin to prevent cost basis corruption
- **Decimal precision P&L** — uses Python's `Decimal` type for cent-accurate profit/loss tracking
- **Form 8949 CSV export** — generates a tax-ready CSV with all required IRS columns, compatible with TurboTax, H&R Block, FreeTaxUSA, and other tax software

---

## Setup

Follow garagesteve's original setup instructions, but use this fork's files instead. All setup steps remain identical.

### Requirements

```
Python 3.10+
```

Install dependencies:
```bash
python -m pip install -r requirements.txt
```

If using Python 3.12+:
```bash
python -m pip install "setuptools==81.0.0"
```

### Run

```bash
python pt_hub.py
```

---

## Files

| File | Role |
|---|---|
| `pt_hub.py` | GUI dashboard — tkinter interface, charts, account display, manual buy/sell, tax export |
| `pt_trader.py` | Core trading engine — order execution, DCA logic, position management, tax export |
| `pt_thinker.py` | Neural network runner — generates buy/sell signal levels |
| `pt_trainer.py` | Neural network trainer — trains models on market data |
| `requirements.txt` | Python package dependencies |

---

## Configuration

All settings are in `gui_settings.json`. Key settings for this fork:

```json
{
  "account_mode": "auto",
  "small_account_threshold": 2500,
  "small_account_settings": {
    "start_allocation_pct": 10.0,
    "dca_multiplier": 1.0,
    "dca_levels": [-7.5, -15.0, -25.0],
    "max_dca_buys_per_24h": 1,
    "pm_start_pct_no_dca": 8.0,
    "pm_start_pct_with_dca": 5.0,
    "trailing_gap_pct": 1.5,
    "trade_start_level": 4,
    "hard_stop_enabled": true,
    "hard_stop_pct": -35.0,
    "max_positions": 3,
    "reserve_minimum_pct": 20.0,
    "require_neural_dca_confirmation": true,
    "profit_mode": "tiered",
    "tiered_profit_t1_pct": 7.0,
    "tiered_profit_t1_sell_pct": 33,
    "tiered_profit_t2_pct": 15.0,
    "tiered_profit_t2_sell_pct": 50
  }
}
```

To revert to 100% original behavior:
```json
"account_mode": "force_existing"
```

---

## Tax Export (Form 8949)

At the end of the tax year, use the Tax Export panel in the Hub GUI:

1. Enter the tax year (e.g., 2026)
2. Click **Export 8949 CSV**
3. The CSV is saved to `hub_data/form_8949_trades_YYYY.csv`
4. The containing folder opens automatically
5. Import the CSV into your tax software

The CSV includes all IRS Form 8949 columns: description of property, date acquired, date sold, proceeds, cost basis, gain or loss, and hold period (short-term vs long-term). A totals row is included at the bottom.

---

## Documentation

- `IMPROVEMENTS_SUMMARY.md` — detailed changelog of every feature, bug fix, and design decision
- `Project_summary_v_1_3_4_5_26.md` — complete project history with session-by-session development notes

---

## Credits

**Original Author:** garagesteve1155 — [PowerTrader_AI](https://github.com/garagesteve1155/PowerTrader_AI)  
**Fork Maintainer:** Jim  
**License:** Apache 2.0

---

## Disclaimer

IMPORTANT: This software places real trades automatically. You are responsible for everything it does to your money and your account. Keep your API keys private. This is not financial advice. The maintainers are not responsible for any losses incurred or any security breaches. You are fully responsible for doing your own due diligence to understand this trading system and use it properly. You are fully responsible for all of your money and any gains or losses.

The Form 8949 CSV export is a convenience tool for record-keeping, not tax advice. Consult a qualified tax professional for your specific situation. Verify all exported data against your Robinhood 1099 and account statements.

**Use at your own risk.**
