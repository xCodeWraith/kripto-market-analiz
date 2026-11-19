# 🔍 Crypto Market Technical Analysis Bot

[![n8n](https://img.shields.io/badge/Workflow-n8n-orange?style=for-the-badge&logo=n8n)](https://n8n.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)
[![Developer](https://img.shields.io/badge/Developer-xCodeWraith%20DEV.-purple?style=for-the-badge)](https://github.com/xCodeWraith)
![Status](https://img.shields.io/badge/Status-Production%20Ready-success?style=for-the-badge)

> **Autonomous AI-powered crypto technical analysis, automated TradingView chart rendering, and instant Telegram intelligence broadcasting.**

---

## 📋 Table of Contents

- [Executive Overview](#-executive-overview)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [Prerequisites](#-prerequisites)
- [Step-by-Step Deployment](#-step-by-step-deployment)
- [Credential Configuration](#️-credential-configuration)
- [Customization & Scheduling](#-customization--scheduling)
- [Cost & API Consumption](#-cost--api-consumption)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

---

## 🎯 Executive Overview

This autonomous n8n workflow operates scheduled market surveillance twice daily (08:00 & 20:00 UTC), performing end-to-end technical intelligence:

1. **Renders multi-asset TradingView charts** for top crypto assets (**BTC, ETH, SOL, XRP**).
2. **Scrapes breaking crypto market news** across trusted crypto news outlets.
3. **Synthesizes technical indicators & news** with an LLM (OpenRouter / GPT-4o) into concise, high-conviction market briefings.
4. **Dispatches rich formatted alerts** directly to your private or team Telegram channel.

### Who Is This For?

* 📈 **Crypto Traders:** Automated daily technical confluence without staring at charts all day.
* 📊 **Portfolio Managers:** Systematic, non-emotional morning/evening market briefings.
* 🤖 **Automation Enthusiasts:** Production reference architecture for multimodal LLM & charting pipelines.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 📊 **Automated Charting** | Generates real-time TradingView chart snapshots via the Chart-img API |
| 🌐 **News Aggregation** | Live web scraping and headline extraction powered by BrowserAI |
| 🤖 **AI Confluence Engine** | OpenRouter / GPT-4o synthesizes chart patterns and sentiment into actionable takeaways |
| 📱 **Telegram Broadcaster** | Rich markdown messages with embedded chart images and entry/invalidation zones |
| ⏰ **Cron Scheduler** | Configurable bi-daily schedule (defaults to 08:00 & 20:00 UTC) |
| 🔄 **Fault-Tolerant Retries** | Automatic retry loops on upstream API transient failures |

---

## ⚙️ System Architecture

```
┌─────────────────────────────────┐
│   Cron Trigger (08:00 & 20:00)  │
└─────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐       ┌─────────────────────────────────┐
│   Chart-img API                 │       │   BrowserAI Scraper             │
│   (TradingView Chart Snapshots) │       │   (Real-Time Crypto Headlines)  │
└─────────────────────────────────┘       └─────────────────────────────────┘
                 │                                         │
                 └───────────────────┬─────────────────────┘
                                     │
                                     ▼
                      ┌─────────────────────────────┐
                      │   OpenRouter / GPT-4o       │
                      │   Synthesizes Chart + News  │
                      └─────────────────────────────┘
                                     │
                                     ▼
                      ┌─────────────────────────────┐
                      │   Telegram Bot Dispatch     │
                      │   (Formatted Message + Img) │
                      └─────────────────────────────┘
```

---

## 📋 Prerequisites

### Core Software
* **n8n** v1.0.0 or higher (Self-Hosted Docker, Desktop, or n8n Cloud)
* **Node.js 18+** (if running n8n via npm)

### API Credentials Required

| Service | Purpose | Registration Link |
|---|---|---|
| **Chart-img** | Chart snapshot generation | [chart-img.com](https://chart-img.com/account/api) |
| **BrowserAI** | Automated web scraping | [browser.ai](https://browser.ai) |
| **OpenRouter** | LLM API routing | [openrouter.ai](https://openrouter.ai/keys) |
| **Telegram Bot** | Signal broadcasting | [@BotFather](https://t.me/BotFather) |

---

## 🚀 Step-by-Step Deployment

### Step 1: Clone the Repository
```bash
git clone https://github.com/xCodeWraith/kripto-market-analiz.git
cd kripto-market-analiz
```

### Step 2: Import into n8n
1. Open your n8n web dashboard.
2. Navigate to **Workflows** → Click **"Import from File"**.
3. Select `kripto-piyasa-analiz-botu.json` and click **Import**.

### Step 3: Configure Node Credentials
Set up the following credentials in your n8n dashboard:

#### 1. Chart-img API
* **Credential Type:** `HTTP Bearer Auth`
* **Token:** `YOUR_CHARTIMG_API_KEY`

#### 2. BrowserAI API
* **Credential Type:** `HTTP Bearer Auth`
* **Token:** `YOUR_BROWSERAI_API_KEY`

#### 3. OpenRouter API
* **Credential Type:** `OpenRouter API` (or standard OpenAI Header Auth)
* **API Key:** `YOUR_OPENROUTER_API_KEY`

#### 4. Telegram Bot API
* **Credential Type:** `Telegram API`
* **Bot Token:** `YOUR_BOT_TOKEN`

---

## ⚙️ Configuration & Customization

### Target Assets & Symbols
To add or modify monitored assets, edit the parameters in the chart generator node:
```json
{
  "symbols": ["BINANCE:BTCUSDT", "BINANCE:ETHUSDT", "BINANCE:SOLUSDT", "BINANCE:XRPUSDT"],
  "interval": "4h",
  "theme": "dark"
}
```

### Telegram Target Channel
In the `Telegram: Send Message` node:
* **Chat ID:** Set your personal Telegram User ID or `@your_channel_name`.
* Ensure your bot is added as an Administrator in your target channel.

---

## 💰 Estimated Cost Breakdown

| Component | Daily Requests | Monthly Cost |
|---|---|---|
| **Chart-img API** | ~8 requests/day | Free tier / ~$5 mo |
| **BrowserAI** | ~2 requests/day | Free credits |
| **OpenRouter (GPT-4o)** | ~2 runs/day | ~$0.30 - $0.90 / mo |
| **n8n Self-Hosted** | Unlimited | Free (Open-Source) |

---

## 🛠️ Troubleshooting

* **Chart image not rendering in Telegram:** Ensure Chart-img returned a valid binary image and Telegram media upload permissions are granted to your bot.
* **OpenRouter timeout:** If responses take longer than 30s, increase the timeout limit in the HTTP Request node to 60000ms.
* **Rate limit errors:** Verify your Chart-img plan accommodates 4 requests in sequence or insert a 2-second delay node between asset queries.

---

**Maintained by:** [xCodeWraith DEV.](https://github.com/xCodeWraith)  
**License:** [MIT](LICENSE)
