# 🤖 MacroFlow-AI: LLM-Powered Economic Trading Bot

**MacroFlow-AI** is a self-hosted, autonomous trading system that combines fundamental news scraping with AI-driven market analysis.

Unlike simple algorithmic bots, this system uses **n8n** to orchestrate a sophisticated decision engine: it scrapes economic events using **Puppeteer**, fetches multi-timeframe market data (H4, M15, M5), and feeds this context into **Anthropic's Claude (LLM)** to generate intelligent trading signals for **MetaTrader 5**.

## 📖 Project Overview

This project solves the "context" problem in automated trading. Instead of blindly executing on a single indicator, this system:

1.  **Scrapes** high-impact economic events (e.g., Non-Farm Payrolls, CPI) in real-time.
2.  **Aggregates** technical market structure (Candle data) across multiple timeframes.
3.  **Synthesizes** this data using an **AI Agent (Claude)** to determine market sentiment.
4.  **Executes** validated trades on a self-hosted platform.

## ⚙️ Architecture & Workflow

The system operates on a 4-stage automated pipeline:

### 1\. The Infrastructure (DevOps)

  * **Host:** Cloud VPS running Ubuntu Linux.
  * **Orchestration:** **Docker Compose** manages the lifecycle of `n8n`, `MySQL`, and `Caddy`.
  * **Security:** **Caddy Web Server** provides reverse proxying and automatic SSL for secure remote access.

### 2\. Data Acquisition (Puppeteer & Node.js)

  * **Event Scraper:** A custom **Puppeteer** script navigates economic calendars (headless browser), extracting event data.
  * **Normalization:** The script parses relative dates (e.g., "Friday, December 12") and converts local NY/EST times into **UTC timestamps** to ensure synchronization with broker servers.
  * **Impact Filtering:** Logic automatically categorizes events into `High`, `Moderate`, or `Low` impact.

### 3\. The "Brain": AI Analysis (n8n & Anthropic)

The core logic is handled by an advanced n8n workflow that performs **Multi-Modal Analysis**:

  * **Technical Input:** Fetches OHLC (Open-High-Low-Close) candle data for **H4 (4-Hour)**, **M15 (15-Minute)**, and **M5 (5-Minute)** timeframes.
  * **Fundamental Input:** Queries the local MySQL database for upcoming events (`Base Future Events` & `Quote Future Events`).
  * **Sentiment Input:** Scrapes live news headlines via HTTP requests.
  * **AI Synthesis:** All data is aggregated and sent to an **Anthropic Chat Model** node. The LLM acts as a financial analyst, reviewing the confluence of technical structure and fundamental news before authorizing a trade.

### 4\. Persistence & Execution

  * **Database:** A **MySQL** container persists the schedule of economic events.
  * **Execution:** Validated signals are transmitted to **MetaTrader 5** (Forex Hedged USD Account).

## 🛠️ Tech Stack

| Component | Technology | Role |
| :--- | :--- | :--- |
| **Orchestration** | **n8n** (Self-Hosted) | Workflow Automation & AI Agents |
| **AI / LLM** | **Anthropic Claude** | Market Sentiment Analysis |
| **Scraping** | **Node.js & Puppeteer** | Headless Browser Automation |
| **Database** | **MySQL** | Event Storage |
| **Server** | **Docker & Caddy** | Containerization & SSL Security |
| **Trading** | **MetaTrader 5** | Execution Engine |

## 📸 System Snapshots

### 1\. The AI Decision Engine (n8n)

*The workflow merges multi-timeframe candle data (H4, M15, M5) with economic calendar events before passing them to an LLM Chain for final analysis.*

### 2\. Intelligent Scraper Logs

*Custom Node.js logic converting messy timezone data (New York EST) into clean UTC timestamps for precise scheduling.*

### 3\. Puppeteer Extraction Logic

*JavaScript code utilizing Puppeteer to navigate DOM elements and extract historical event data.*

### 4\. Infrastructure Config

*Docker Compose configuration defining the isolated networks (`n8n_net`) and volume persistence.*

-----

## 🚀 Deployment

### Prerequisites

  * A VPS (Ubuntu 20.04+)
  * Docker & Docker Compose
  * An Anthropic API Key (for the LLM node)
  * MetaTrader 5 Account


## ⚠️ Disclaimer

*This project involves financial trading and experimental AI analysis. It is for educational purposes only. Past performance of the AI model does not guarantee future results.*

-----




Since this is the "secret sauce" of your bot, **would you like me to help you draft the `System Prompt` for that LLM node?**

I can help you write a prompt that effectively teaches Claude how to weigh the *technical* data (H4 candles) against the *fundamental* data (News events) to avoid hallucinations.
