# Output

## 📤 Output – How Your System Communicates

### 1. Introduction

After setting up your logic (programming language), hosting, and database,\
you need a way to **deliver information to users** — that’s where **Output** comes in.

In any blockchain automation, **output** is the visible result of everything your system does:

* a Telegram alert,
* an email report,
* a dashboard update, or
* a simple console log.

Without output, even the smartest backend would be invisible.

***

### 2. What Is Output?

**Output** means any **data, message, or visualization** that your system produces and shares with users or other systems.\
It’s the _frontline_ of communication — how you know your bot is alive and doing its job.

Common output channels include:

* **Telegram / Discord Bots**
* **Email notifications**
* **Google Sheets (as dashboards)**
* **Web dashboards or APIs**
* **Console or log files** (for developers)

Each method serves a different purpose — you can mix and match depending on your needs.

***

### 3. Relationship with Other Components

| Component             | Role in Output                                |
| --------------------- | --------------------------------------------- |
| **On-chain Data**     | Provides the real information to be displayed |
| **API Layer**         | Fetches data from blockchain sources          |
| **Program Logic**     | Processes and formats the output              |
| **Database (Sheets)** | Stores output history or summaries            |
| **Hosting**           | Ensures your output system runs continuously  |

> 🧩 Output is the **final bridge** connecting blockchain data to real human users.

***

### 4. Telegram Output

**Telegram** is the most popular output channel for blockchain bots —\
instant, reliable, and widely used in Web3 communities.

#### ✅ Pros

* Real-time notifications
* Simple setup with `BotFather`
* Supports text, emojis, buttons, and inline commands
* Can broadcast to groups or private users
* Integrates easily with Python, Node.js, or Apps Script

#### ⚙️ Example (Python)

```python
import requests

BOT_TOKEN = "your_bot_token"
CHAT_ID = "your_chat_id"
MESSAGE = "🚀 New Cardano transaction detected!"

requests.get(f"https://api.telegram.org/bot{BOT_TOKEN}/sendMessage",
             params={"chat_id": CHAT_ID, "text": MESSAGE})
```

#### ⚙️ Example (Node.js)

```javascript
import TelegramBot from 'node-telegram-bot-api';
const bot = new TelegramBot('YOUR_BOT_TOKEN', { polling: false });
bot.sendMessage('YOUR_CHAT_ID', '💡 ADA wallet updated: 500.25 ADA');
```

#### Use Cases

* Notify when a transaction occurs
* Alert when stake rewards arrive
* Send price updates or governance vote summaries
* Report daily data logs

***

### 5. Email Output

**Email** is perfect for daily or weekly summaries instead of real-time alerts.\
It’s asynchronous, but reliable — ideal for **aggregated reports**.

#### ✅ Pros

* Universal and accessible
* Works well for summaries and reports
* Integrates directly with Google Apps Script
* Can attach CSV/Sheets or HTML content

#### ⚙️ Example (Apps Script)

```javascript
function sendDailyReport() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Wallets");
  const lastRow = sheet.getLastRow();
  const balance = sheet.getRange(`C${lastRow}`).getValue();
  const subject = "Cardano Daily Wallet Report";
  const body = `Today's wallet balance: ${balance} ADA`;
  
  MailApp.sendEmail("you@example.com", subject, body);
}
```

Add a **time-based trigger** → runs automatically every 24 hours.\
This creates a zero-maintenance reporting pipeline.

***

### 6. Google Sheets Dashboard

**Google Sheets** isn’t just a database — it’s also a visual output layer.\
By combining formulas, charts, and conditional formatting, you can build dashboards directly inside the same spreadsheet that stores your data.

#### Example dashboard components:

| Metric       | Formula / Source              |
| ------------ | ----------------------------- |
| Total ADA    | `=SUMIF(Token,"ADA",Balance)` |
| Last Update  | `=MAX(Timestamp)`             |
| Wallet Count | `=COUNTA(Wallet Address)`     |

You can create:

* Trend charts for balance changes
* Leaderboards for multi-wallet setups
* Transaction logs with auto color highlights

Sheets act as a **living dashboard** — always synchronized with the latest bot updates.

***

### 7. Web Dashboard / API Output

For larger setups, you can expose your data via a **small web API** or **React dashboard**.

Example architecture:

```
[Backend Bot] → writes to Database
           ↓
[Express / Flask API] → exposes endpoint
           ↓
[Frontend Dashboard] → visualizes data
```

#### Example (Python Flask)

```python
from flask import Flask, jsonify
app = Flask(__name__)

@app.route('/wallets')
def wallets():
    return jsonify({"wallets": [{"addr": "addr1...", "balance": 320.5}]})

app.run(host='0.0.0.0', port=8080)
```

Then you can connect your React/Vue dashboard to display it publicly or privately.

✅ **Best for:** DAO dashboards, public explorers, or analytics tools.

***

### 8. Logging and Developer Output

Not all outputs are meant for users —\
sometimes you need **internal logs** for debugging, performance tracking, or auditing.

#### Example: Logging in Python

```python
import logging

logging.basicConfig(filename='bot.log', level=logging.INFO)
logging.info("New transaction detected for addr1...")
```

#### Example: Console Debugging (Node.js)

```javascript
console.log(`[INFO] ${new Date().toISOString()} - Balance updated`);
```

These logs are invaluable when running bots on VPS or Cloud environments.

> 🧩 Always log key actions — it’s your “black box recorder” when debugging blockchain automation.

***

### 9. Combining Multiple Output Channels

Most real-world systems use **hybrid output**:

* Telegram → instant alerts
* Email → daily summaries
* Google Sheets → visual dashboard
* Logs → internal tracking

Example structure for **Cardano\_DIY\_bot**:

```
[Blockchain API]
     ↓
[Program Logic]
     ↓
[Google Sheets] ←→ [Email Report]
     ↓
[Telegram Bot]  ← real-time alerts
```

This multi-channel design ensures users always stay informed —\
even if one channel fails or goes offline.

***

### 10. Output Layer Summary

| Output Type                | Tool / Method      | Use Case           | Frequency  |
| -------------------------- | ------------------ | ------------------ | ---------- |
| **Telegram Bot**           | Bot API            | Real-time alerts   | Instant    |
| **Email Report**           | Apps Script / SMTP | Daily summary      | 1x per day |
| **Google Sheet Dashboard** | Built-in charts    | Data visualization | Continuous |
| **Web Dashboard / API**    | Flask / Express    | Public interface   | On demand  |
| **Console / Log File**     | Logging module     | Developer tracking | Continuous |

***

### 11. Summary

Output is the **voice of your system** —\
it transforms raw blockchain data into something users can understand and act upon.

| Component            | Role                                      |
| -------------------- | ----------------------------------------- |
| **API**              | Collects blockchain or price data         |
| **Database**         | Stores the raw information                |
| **Program Language** | Processes and formats it                  |
| **Hosting**          | Keeps the app online                      |
| **Output**           | Delivers the result to users in real time |

> 💡 The best output is **clear, actionable, and automated** —\
> so users get the insight _before they even think to check_.
