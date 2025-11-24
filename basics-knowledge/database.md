# Database

### 1. Introduction

Every bot, automation, or dApp needs a **place to store data** — whether it’s transaction logs, wallet balances, or user activity.\
That place is called a **database**.

In traditional applications, databases like **PostgreSQL**, **MySQL**, or **MongoDB** store structured information.\
But for smaller blockchain bots or community tools like **Cardano\_DIY\_bot**, we can use **Google Sheets** as a lightweight, free, and serverless database.

Why?\
Because it’s:

* Simple to set up
* Free to use
* Accessible via **Google Apps Script or API**
* Easy to visualize, share, and edit collaboratively

***

### 2. What Is a Database?

A **database** is a structured way to store, retrieve, and update data.\
For example, your bot might need to remember:

* Which wallets it monitors
* What transactions were already processed
* Token balances, or last update times
* Telegram user settings or admin lists

Without a database, your bot would “forget” everything when restarted.

***

### 3. Google Sheets as a Database

**Google Sheets** can function as a simple, cloud-hosted, and collaborative database for light applications.

Advantages:

* ✅ Free, no server or setup needed
* ✅ Real-time collaboration and version history
* ✅ Easy integration with Apps Script
* ✅ API access via `sheets.googleapis.com`
* ✅ Visual overview (rows = records, columns = fields)

***

#### Example: Sheet Structure for Cardano\_DIY\_bot

| Timestamp        | Wallet Address | Token  | Balance | Tx Count | Last Check |
| ---------------- | -------------- | ------ | ------- | -------- | ---------- |
| 2025-10-22 13:00 | addr1qxyz...   | ADA    | 304.11  | 142      | OK         |
| 2025-10-22 13:00 | addr1qabc...   | TOKEN1 | 503.22  | 88       | OK         |

Each row is one record.\
Each column is a field that your bot reads or updates automatically.

***

### 4. How It Connects to Hosting

| Component             | Role                                         |
| --------------------- | -------------------------------------------- |
| **Hosting**           | Keeps the bot running and calling APIs       |
| **API**               | Provides data from blockchain sources        |
| **Database (Sheets)** | Stores, logs, and tracks the data            |
| **Output**            | Uses stored data to send messages or reports |

Hosting ensures the bot can **write to the sheet continuously** without needing your computer online.

***

### 5. Using Google Apps Script to Access the Sheet

Google Apps Script (GAS) gives you direct access to Google Sheets from JavaScript-like code.

#### Example: Writing data to Google Sheets

```javascript
function saveBalance(addr, balance) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Wallets");
  sheet.appendRow([new Date(), addr, balance]);
}
```

You can trigger this function manually or automatically (time-based or API-triggered).

#### Example: Reading data

```javascript
function getWalletList() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Wallets");
  const data = sheet.getRange("A2:A").getValues().flat().filter(x => x);
  return data;
}
```

Now your hosted script can easily fetch a list of wallet addresses to monitor.

***

### 6. Accessing Sheets from Python or Node.js

If your bot is hosted on a VPS or Google Cloud (not in Apps Script),\
you can still read/write Google Sheets data through the **Google Sheets API**.

#### Python Example

```python
from google.oauth2 import service_account
from googleapiclient.discovery import build

SCOPES = ["https://www.googleapis.com/auth/spreadsheets"]
creds = service_account.Credentials.from_service_account_file(
    "service-account.json", scopes=SCOPES
)
sheet = build("sheets", "v4", credentials=creds)

SPREADSHEET_ID = "your-sheet-id"
RANGE = "Wallets!A:F"

# Read
result = sheet.spreadsheets().values().get(
    spreadsheetId=SPREADSHEET_ID, range=RANGE
).execute()
rows = result.get("values", [])
print(rows)

# Write
sheet.spreadsheets().values().append(
    spreadsheetId=SPREADSHEET_ID,
    range="Wallets!A:F",
    valueInputOption="RAW",
    body={"values": [["2025-10-22", "addr1...", "ADA", 304.11]]},
).execute()
```

✅ Works seamlessly with hosted environments (Google Cloud, Contabo, etc.)\
✅ Perfect for bots that collect data from on-chain APIs and log them automatically.

***

### 7. When to Use Google Sheets vs Traditional Databases

| Use Case                                          | Recommended Database                 |
| ------------------------------------------------- | ------------------------------------ |
| Simple logs, small-scale data, 1–5 users          | **Google Sheets**                    |
| Event logs, Telegram bot settings, quick scripts  | **Google Sheets + Apps Script**      |
| Advanced analytics, large datasets, heavy queries | **PostgreSQL / Supabase**            |
| Real-time dashboards with charting                | **Google Sheets or Supabase**        |
| Encrypted sensitive data                          | **Supabase / Cloud SQL (with auth)** |

For **Cardano\_DIY\_bot**, Google Sheets is more than enough.\
It can track all wallet activities, bot usage, and proposal results dynamically.

***

### 8. Tips for Organizing Data in Google Sheets

* **One sheet per data type**
  * Example: `Wallets`, `Transactions`, `Settings`, `Logs`
* **Use headers consistently**
  * Always define column names in Row 1
* **Keep timestamps**
  * Helps with debugging and automation tracking
* **Avoid overwriting old data**
  * Use `appendRow()` instead of replacing cells
* **Limit sheet size**
  * Archive or clear logs periodically if you exceed \~100k rows

***

### 9. Connecting Google Sheets to Other Layers

| Layer             | How It Interacts                                                       |
| ----------------- | ---------------------------------------------------------------------- |
| **On-chain Data** | Data fetched from Koios or Blockfrost APIs is logged here              |
| **API Layer**     | Reads from and writes to the sheet                                     |
| **Hosting**       | Ensures the script runs automatically (Apps Script, VPS, or Cloud Run) |
| **Output Layer**  | Uses data from Sheets to send messages or generate reports             |

Example flow:

```
[Koios API]
   ↓
[Hosted Bot (Google Cloud / Apps Script)]
   ↓
[Google Sheet]
   ↓
[Telegram / Email Output]
```

This architecture is **simple, transparent, and efficient** — perfect for Web3 community tools.

***

### 10. Summary

Google Sheets serves as a **lightweight cloud database** for your blockchain applications.\
It’s serverless, free, and integrates perfectly with **Google Cloud** and **Apps Script**.

| Concept               | Role                                      |
| --------------------- | ----------------------------------------- |
| **Hosting**           | Runs the code that interacts with data    |
| **API**               | Fetches live blockchain or external data  |
| **Database (Sheets)** | Stores and visualizes data over time      |
| **Output**            | Uses stored data to inform or alert users |

> 🧩 Google Sheets turns your blockchain bot into a transparent, auditable, and collaborative system —\
> no complex backend, just clean logic and shared visibility.
