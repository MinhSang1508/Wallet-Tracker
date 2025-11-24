# Program language

## 💻 Program Language – The Logic Behind Everything

### 1. Introduction

After learning about **APIs**, **on-chain data**, **hosting**, and **databases**, we finally reach the **core of any system** — the **programming language** that connects them all.

Your program language defines:

* How your bot interacts with blockchain APIs
* How data is processed or stored
* How outputs (messages, reports, dashboards) are generated

In short, it’s the **“brain”** of your automation.

***

### 2. What Is a Programming Language?

A **programming language** is a structured way to tell the computer _what to do_ and _how to do it_.\
Each language has its own style, ecosystem, and strengths.

For blockchain automation, we usually consider:

* **Python** – fast prototyping, rich API ecosystem
* **JavaScript / Node.js** – ideal for web and bot integration
* **Google Apps Script** – cloud-hosted and serverless for small tools

All three can connect to blockchain data and APIs — but each fits different use cases.

***

### 3. Python – The Power of Simplicity

Python is widely used in data analysis, blockchain scripting, and backend automation.

#### ✅ Pros

* Easy to learn and read
* Massive ecosystem: `requests`, `pandas`, `web3`, `aiohttp`, etc.
* Perfect for **CLI tools**, **API bots**, or **data pipelines**
* Works smoothly with **Google Sheets API**, **Supabase**, or **PostgreSQL**
* Strong community support for **Cardano ecosystem tools** (Koios, Blockfrost SDKs)

#### ⚙️ Example: Fetch ADA price from CoinGecko

```python
import requests

url = "https://api.coingecko.com/api/v3/simple/price?ids=cardano&vs_currencies=usd"
res = requests.get(url).json()
price = res["cardano"]["usd"]
print(f"ADA Price: ${price}")
```

#### 🧩 Example: Append to Google Sheet

```python
from googleapiclient.discovery import build
from google.oauth2 import service_account

creds = service_account.Credentials.from_service_account_file("service-account.json")
sheet = build("sheets", "v4", credentials=creds)
sheet.spreadsheets().values().append(
    spreadsheetId="your-sheet-id",
    range="Sheet1!A:B",
    valueInputOption="RAW",
    body={"values": [["2025-10-22", "ADA Price: $0.45"]]},
).execute()
```

✅ **Best for:**

* Data processing and analytics
* Bots that need to fetch or log blockchain data
* CLI or backend services on Contabo / Google Cloud

***

### 4. JavaScript / Node.js – The Web & Bot Connector

**JavaScript**, and its runtime **Node.js**, dominate web development and API-driven automation.\
It’s the backbone for Telegram bots, Discord bots, and frontend dashboards.

#### ✅ Pros

* Excellent for event-driven bots (Telegram, Discord, Twitter)
* Native support for async APIs and WebSocket connections
* Works with **Vercel**, **Render**, or **Cloud Run** easily
* Large ecosystem: `axios`, `node-fetch`, `telegraf`, `express`, `web3.js`

#### ⚙️ Example: Simple Telegram bot (Node.js)

```javascript
import TelegramBot from 'node-telegram-bot-api';
import fetch from 'node-fetch';

const bot = new TelegramBot('YOUR_BOT_TOKEN', { polling: true });

bot.on('message', async (msg) => {
  const chatId = msg.chat.id;
  const res = await fetch('https://api.koios.rest/api/v1/tip');
  const data = await res.json();
  bot.sendMessage(chatId, `Latest block: ${data[0].block_no}`);
});
```

#### 🧩 Example: Write to Google Sheets via API

```javascript
import { google } from 'googleapis';
import fs from 'fs';

const auth = new google.auth.GoogleAuth({
  keyFile: 'service-account.json',
  scopes: ['https://www.googleapis.com/auth/spreadsheets']
});

const sheets = google.sheets({ version: 'v4', auth });
await sheets.spreadsheets.values.append({
  spreadsheetId: 'your-sheet-id',
  range: 'Logs!A:B',
  valueInputOption: 'RAW',
  requestBody: { values: [[new Date().toISOString(), 'Node bot active']] }
});
```

✅ **Best for:**

* Bots and interactive web services
* Real-time data fetching or notifications
* Apps combining Telegram + API + Sheet

***

### 5. Google Apps Script – Lightweight Cloud Logic

**Google Apps Script (GAS)** is a special, serverless JavaScript variant hosted entirely by Google.\
No need to deploy or host anything — your logic runs inside Google’s infrastructure.

#### ✅ Pros

* 100% serverless, always online
* Easy to integrate with **Google Sheets**, **Gmail**, **Drive**, or **Calendar**
* Free for small workloads
* Ideal for light automation or data logging
* Trigger-based (time, API, event, form, etc.)

#### ⚙️ Example: Fetch Cardano data and log to Google Sheets

```javascript
function fetchCardanoTip() {
  const url = "https://api.koios.rest/api/v1/tip";
  const res = UrlFetchApp.fetch(url);
  const data = JSON.parse(res);
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Tip");
  sheet.appendRow([new Date(), data[0].block_no]);
}
```

#### ⚙️ Set a time trigger

1. In Apps Script Editor → Triggers
2. Add → Time-driven → Every 10 minutes
3. Save and deploy

Now your bot logs blockchain data continuously, hosted and executed entirely on Google servers.

✅ **Best for:**

* Automation workflows using Sheets, Mail, or APIs
* Low-cost, lightweight monitoring
* Educational or internal use

***

### 6. Choosing the Right Language

| Goal                               | Recommended Language        |
| ---------------------------------- | --------------------------- |
| **Quick automation, no hosting**   | Google Apps Script          |
| **Data analysis or research**      | Python                      |
| **Telegram/Discord bots**          | Node.js (JavaScript)        |
| **Web dashboard or REST API**      | Node.js                     |
| **Cross-service automation**       | Python + Google Sheets API  |
| **Prototype DAO tools on Cardano** | Python for logic, JS for UI |

***

### 7. Multi-Language Architecture (Real Use Case)

For a full automation system like **Cardano\_DIY\_bot**, different parts can use different languages:

```
[Python Backend]
  - Fetch blockchain data
  - Write logs to Sheets via API
  - Send status to Telegram API

[Google Apps Script]
  - Sync Sheets and Email Reports
  - Run time-based triggers

[Node.js Bot]
  - Interact with users via Telegram
  - Forward data from backend
```

Each component talks to the others through **APIs** or **Google Sheets** as a shared data layer.\
This modular setup scales easily while staying simple and transparent.

***

### 8. Deployment Overview

| Environment                   | Supported Language | Example Use            |
| ----------------------------- | ------------------ | ---------------------- |
| **Google Cloud VM**           | Python / Node.js   | Full backend or bot    |
| **Vercel / Render / Railway** | Node.js            | Web APIs or dashboards |
| **Google Apps Script**        | GAS (JS variant)   | Serverless automation  |
| **Local + CRON (backup)**     | Python             | Periodic manual runs   |

***

### 9. Summary

Programming languages define how your blockchain logic behaves — they are the **control center** of your entire system.

| Component             | Role                                  |
| --------------------- | ------------------------------------- |
| **API**               | Provides blockchain or market data    |
| **On-chain Data**     | Source of truth                       |
| **Database (Sheets)** | Stores information                    |
| **Hosting**           | Keeps everything running              |
| **Program Language**  | Executes logic and connects all parts |

> 🧠 Choosing the right language isn’t about syntax — it’s about **ecosystem fit** and **maintainability**.\
> In most Cardano projects, **Python + Google Sheets + Apps Script** offers the perfect balance of power, clarity, and accessibility.
