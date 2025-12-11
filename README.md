# MacroFlow-n8n
MacroFlow-n8n is a self-hosted, fully containerized autonomous trading system designed to capitalize on high-impact economic news events. Unlike technical analysis bots, this engine focuses on fundamental data ingestion, utilizing n8n for orchestration, Node.js for scraping, and MetaTrader 5 (MT5) for execution.
This is a sophisticated project that combines DevOps, backend scripting, and financial technology. Below is a curated list of repository names and a professional, "GitHub-ready" README template.

# 🤖 MacroFlow-n8n: Autonomous Event-Driven Trading Bot

**MacroFlow-n8n** is a self-hosted, fully containerized autonomous trading system designed to capitalize on high-impact economic news events. Unlike technical analysis bots, this engine focuses on fundamental data ingestion, utilizing **n8n** for orchestration, **Node.js** for scraping, and **MetaTrader 5 (MT5)** for execution.

## 📖 Project Overview

Trading economic news (like Non-Farm Payrolls, CPI, or Rate Decisions) requires precision and speed. This project automates the entire pipeline:

1.  **Scrapes** economic calendars for high-impact events.
2.  **Normalizes** disparate timezones (EST/NY) into a unified UTC format.
3.  **Stores** structured data in a SQL database.
4.  **Executes** trades via n8n workflows connected to an MT5 hedging account.

The entire stack runs on a secure, cloud-hosted VPS managed by Docker Compose.

## ⚙️ Architecture & Workflow

The system is built on a microservices architecture:

### 1\. Infrastructure (DevOps)

  * **Host:** Ubuntu VPS (Cloud).
  * **Security:** **Caddy** acts as a reverse proxy, handling automatic SSL (Let's Encrypt) for the n8n dashboard (`task.up2cloud.online`) and firewall management (`ufw`).
  * **Containerization:** All services (n8n, MySQL, Caddy) are defined in `docker-compose.yml` and run on isolated bridge networks.

### 2\. The Data Pipeline (Node.js)

  * **Ingestion:** A custom Node.js script crawls economic news sources.
  * **Normalization:**
      * Parses relative dates (e.g., "Friday, December 12").
      * Converts source timezones (New York) to **UTC** to ensure perfect synchronization with the broker server.
      * Filters events based on "Impact" (Low, Moderate, High).

### 3\. Persistence (MySQL)

  * **Database:** A persistent MySQL container (`market_data`) stores the event schedule.
  * **Management:** **phpMyAdmin** is deployed alongside for visual inspection of the `economic_events` table and query debugging.

### 4\. Logic & Execution (n8n & MT5)

  * **The Brain:** n8n workflows poll the database for events matching `CURRENT_TIME + X minutes`.
  * **The Hands:** Upon trigger validation, n8n sends execution commands to the MetaTrader 5 terminal (Forex Hedged USD Account).

## 🛠️ Tech Stack

| Component | Technology | Role |
| :--- | :--- | :--- |
| **Orchestration** | **n8n** (Self-Hosted) | Workflow automation & Logic |
| **Scripting** | **Node.js** | Web Scraping & Date Parsing |
| **Database** | **MySQL** | Data Storage |
| **Proxy/SSL** | **Caddy** | Reverse Proxy & HTTPS Security |
| **Execution** | **MetaTrader 5** | Trade Terminal (Forex Hedged) |
| **OS** | **Ubuntu Linux** | Server Environment |

## 🚀 Deployment

### Prerequisites

  * A VPS (DigitalOcean, Hetzner, AWS, etc.)
  * Docker & Docker Compose installed
  * Domain name pointed to VPS IP

### Installation

1.  **Clone the Repo:**

    ```bash
    git clone https://github.com/yourusername/MacroFlow-n8n.git
    cd MacroFlow-n8n
    ```

2.  **Configure Environment:**
    Create a `.env` file with your credentials:

    ```env
    MYSQL_ROOT_PASSWORD=your_secure_password
    N8N_HOST=task.yourdomain.com
    N8N_BASIC_AUTH_USER=admin
    N8N_BASIC_AUTH_PASSWORD=password
    ```

3.  **Spin up the Stack:**

    ```bash
    docker-compose up -d
    ```

4.  **Run the Initial Scrape:**
    Access the Node container to seed the database:

    ```bash
    docker exec -it n8n_scraper_1 node scraper.js
    ```

## 📸 Screenshots

  * **n8n Dashboard:** Secure, self-hosted workflow editor.
  * **Scraper Logs:** Real-time console output showing accurate UTC conversion.
  * **Database:** Structured SQL tables ready for querying.

## ⚠️ Disclaimer

*Trading Forex and CFDs carries a high level of risk and is not suitable for all investors. The high degree of leverage can work against you as well as for you. This software is for educational purposes only.*

-----

### 💡 Next Step

Your screenshots show the database setup (`economic_events` table) and the scraper logs, but I noticed you are using `phpMyAdmin`.

**Would you like me to draft the specific SQL Schema (`CREATE TABLE` code) for your `economic_events` table so you can include it in the repository for other users to replicate your database structure?**
