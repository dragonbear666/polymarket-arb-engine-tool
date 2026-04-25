# Kalshi Earnings Bot

> Automated earnings event trader for Kalshi. Monitors upcoming corporate earnings announcements, matches them to related Kalshi markets, and enters positions based on historical beat/miss rates and implied probability divergence.

*Documentation updated: April 2026*

![preview_kalshi earnings event trader](https://github.com/user-attachments/assets/445aaee5-854a-4e84-8a3a-9d8fb560c07c)

---

## What is Kalshi Earnings Bot?

Kalshi Earnings Bot tracks upcoming corporate earnings announcements and matches them to related Kalshi markets - stock price above/below thresholds, earnings beat/miss markets, and sector-related prediction markets. It scores each opportunity using historical beat rates and current Kalshi pricing, entering positions where model diverges from the market.

Earnings are the highest-signal events for related prediction markets. This bot is built to trade them systematically.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/dragonbear666/polymarket-arb-engine-tool/releases) |

---

## What It Monitors

| Data Source | Content | Update Rate |
|-------------|---------|-------------|
| **Earnings calendar** | Upcoming announcement dates and estimates | Daily |
| **Historical beat rates** | Beat/miss history per company | Static + weekly update |
| **Kalshi earnings markets** | All active earnings-related prediction markets | Real-time |
| **Pre-earnings price drift** | Kalshi market price movement in week before report | Real-time |

---

## Engine Features

* **Earnings calendar integration** - auto-loads upcoming earnings from public calendar feeds
* **Beat rate scoring** - calculates historical beat frequency per company and sector
* **Kalshi market matching** - maps earnings events to relevant active Kalshi markets
* **Pre-earnings drift detection** - monitors Kalshi price movement in the 5 days before report
* **Position sizing** - Kelly-based sizing proportional to edge confidence
* **Post-earnings exit** - automatically closes positions after announcement resolution
* **Telegram alerts** - notifications on upcoming events and position entries

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Calendar** | Auto-loaded | Configurable source |
| **Entry** | Auto on threshold | Full control |
| **Config** | `config.toml` | Direct code access |
| **Alerts** | Dashboard + Telegram | JSON + Telegram |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set min edge threshold and Kalshi API key
# 3. Run Kalshi Earnings Bot - calendar loads immediately, monitoring starts
```

### Python

```bash
cd kalshi-earnings-bot/python
pip install -r requirements.txt
python kalshi-earnings-bot-v.1.0.7.py
```

---

## How It Works

![kalshi earnings event pipeline](https://github.com/user-attachments/assets/625da244-8d1e-4195-9da8-769d452d853e)

Four stages per daily cycle:

1. **Load calendar** - fetches upcoming earnings announcements for the next 14 days
2. **Match** - finds active Kalshi markets related to each upcoming announcement
3. **Score** - calculates edge from historical beat rate vs. current Kalshi implied probability
4. **Enter** - places Kelly-sized position when edge clears minimum threshold

### Config Reference

```toml
[earnings]
lookahead_days = 14
min_edge_score = 0.07
kelly_fraction = 0.20
max_position_usd = 40

[filters]
min_historical_reports = 8
min_kalshi_liquidity_usd = 200

[exit]
auto_exit_after_announcement = true
exit_hold_hours = 2

[kalshi]
api_key = ""
api_secret = ""

[telegram]
bot_token = ""
chat_id = ""

[export]
earnings_log_csv = "data/earnings/earnings_log.csv"
```

---

## Earnings Log Format

```json
{
  "trade_id": "ern_20260406_005",
  "company": "NVDA",
  "announcement_date": "2026-04-22",
  "kalshi_market": "nvda-above-900-post-earnings-q1-2026",
  "historical_beat_rate": 0.78,
  "kalshi_yes_price": 0.54,
  "edge_score": 0.12,
  "kelly_size_usd": 36.00,
  "side_entered": "YES",
  "entry_price": 0.54,
  "timestamp": "2026-04-06T09:00:00Z"
}
```

---

## Verified Live

![kalshi earnings trade result](https://github.com/user-attachments/assets/f334ab6e-6e7a-4a20-881c-0a9498b6e830)

**Configuration used:**
* Lookahead 14d, min edge 0.07, Kelly 0.20, auto-exit after announcement

**Earnings entry:**

| | Details |
|---|---|
| Company | NVDA |
| Announcement | April 22, 2026 |
| Market | nvda-above-900-post-earnings-q1-2026 |
| Historical beat rate | 78% |
| Kalshi YES price | 0.54 |
| Edge score | 0.12 |
| Position | YES $36.00 |
| Tx hash | 0x9e3d6b1f4c7a20e5b8d1f4c7a0e3b6d9f2c5a8e1b4d7f0a3c6e9b2d5f8a1c4e7 |

---

## Frequently Asked Questions

**What is Kalshi Earnings Bot?**
Kalshi Earnings Bot monitors upcoming corporate earnings announcements, matches them to related Kalshi prediction markets, and enters positions where historical beat rate diverges from current Kalshi implied probability.

**What data source does it use for earnings calendar?**
The bot loads from public earnings calendar RSS and JSON feeds. No paid financial data subscription is required.

**What is historical beat rate?**
Beat rate is the percentage of recent earnings reports where the company exceeded consensus EPS or revenue estimates. Companies with 75%+ beat rates historically are scored as more likely to beat again.

**Does it exit after the announcement?**
Yes. With `auto_exit_after_announcement = true`, the bot places an exit order 2 hours after the announcement date, regardless of outcome. This captures pre-announcement positioning and avoids resolution risk.

**What Kalshi markets does it target?**
It matches stock price above/below threshold markets, earnings beat/miss markets (where available), and sector ETF markets related to the reporting company.

**Can it run alongside other Kalshi tools?**
Yes. Earnings Bot runs on its own calendar and does not conflict with other tools operating on different market categories.

---

## Use Cases

- **Kalshi earnings trader** - systematic earnings event entries on related Kalshi markets
- **Kalshi beat rate analysis** - historical beat rate scoring for upcoming announcements
- **Kalshi corporate events** - track and trade earnings-related prediction markets
- **Kalshi pre-earnings entries** - enter positions before announcement based on historical data
- **Prediction market earnings** - connect earnings calendar to active Kalshi market opportunities

---

## Repository Structure

```
kalshi-earnings-bot/
+-- kalshi-earnings-bot-v.1.0.7.exe
+-- config.toml
+-- data/
|   +-- earnings/
|   +-- calendar/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- calendar_loader.py
|   |   +-- matcher.py
|   |   +-- executor.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, typer[all], httpx, kalshi-python, pandas, feedparser
```

* Kalshi account with API access
* Telegram bot token (for alerts)

---

*Set it. Monitor it. Act on it.*
