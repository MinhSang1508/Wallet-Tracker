# Hosting

### 1. Introduction

After exploring **on-chain data** and **APIs**, the next key concept is **hosting** — the environment that keeps your bot or application running 24/7.\
Hosting ensures your system stays online, connected to the blockchain, and ready to respond to users or events in real time.

Without hosting, your script only runs on your personal computer. Once it shuts down, everything stops — no data fetching, no automation, no alerts.

For blockchain-based automation like **Cardano\_DIY\_bot**, hosting provides:

* Continuous access to blockchain APIs
* Stable execution of bot logic
* Real-time notifications and data sync

***

### 2. What Is Hosting?

**Hosting** means deploying and running your application code on a **remote server** connected to the internet.\
Instead of running your Python or Node.js file locally, it’s executed on a system that never sleeps.

Think of it like **renting a computer in the cloud**.\
That computer runs your bot, connects to APIs, interacts with users, and stores data — all automatically.

Hosting environments can be:

* **Cloud servers** (Google Cloud, AWS, DigitalOcean)
* **VPS (Virtual Private Server)** platforms (Contabo, Hetzner)
* **Serverless platforms** (Vercel, Render, Railway)
* **Scripting environments** like **Google Apps Script**

***

### 3. How Hosting Connects to Other Components

| Component                         | Relation to Hosting                                             |
| --------------------------------- | --------------------------------------------------------------- |
| **API**                           | Hosting keeps your app online to send/receive API requests.     |
| **On-chain Data**                 | Hosted scripts can poll blockchain data in real time.           |
| **Database**                      | Hosting bridges your app with external storage or Supabase.     |
| **Bot Outputs (Telegram, Email)** | Hosting ensures your bot stays responsive to incoming messages. |

> 🧠 Hosting is the _bridge between blockchain logic and user interaction._

***

### 4. Types of Hosting

| Type                             | Description                                         | Example Providers           | Best For                             |
| -------------------------------- | --------------------------------------------------- | --------------------------- | ------------------------------------ |
| **VPS (Virtual Private Server)** | Dedicated virtual machine with root access.         | Contabo, Hetzner            | Always-on bots, backend logic        |
| **Cloud Hosting (IaaS)**         | Scalable infrastructure managed by cloud providers. | Google Cloud Compute Engine | Long-running applications            |
| **Serverless / PaaS**            | Auto-managed platform; deploy code directly.        | Railway, Render, Vercel     | Lightweight APIs, dashboards         |
| **Script Hosting**               | Run automation scripts without managing servers.    | Google Apps Script          | Data automation, email/Telegram bots |

***

### 5. Typical Hosting Workflow

```
[User / Telegram]
       ↓
[Hosted Bot (Python/JS)]
       ↓
[API (Koios, Blockfrost)]
       ↓
[Database (Supabase, Sheets)]
       ↓
[Output → Notification or Dashboard]
```

The hosted environment acts as the **automation hub** — constantly connecting blockchain data, logic, and users.

Example use case for **Cardano\_DIY\_bot**:

* Bot checks wallet activity via **Koios API** every 10 minutes.
* When a new transaction appears, it sends an alert via **Telegram**.
* Hosting ensures this happens 24/7, even when you’re offline.

***

### 6. VPS Hosting (e.g., Contabo, Hetzner)

**VPS (Virtual Private Server)** gives you full control over your environment — perfect for bots or persistent background jobs.

**Pros**

* Always-on performance (ideal for continuous polling or monitoring).
* Full access: install Python, Node.js, Docker, or database.
* Can handle multiple processes or bots simultaneously.

**Cons**

* Requires basic Linux/SSH setup.
* You manage security, logs, and updates.

**Example setup:**

```bash
ssh user@your-vps-ip
sudo apt update && sudo apt install python3 pip git
git clone https://github.com/yourrepo/cardano_diy_bot
cd cardano_diy_bot
pip install -r requirements.txt
python main.py
```

Keep it running:

```bash
tmux new -s bot
python main.py
# Detach: Ctrl+B, then D
```

***

### 7. Google Cloud Hosting

**Google Cloud** offers both **virtual machines (Compute Engine)** and **serverless services (App Engine, Cloud Run)**.\
It’s a powerful choice for blockchain or automation tools that need scalability and reliability.

#### **Option 1: Google Compute Engine**

* Create a VM (Ubuntu 22.04, 2vCPU, 4GB RAM)
* Connect via SSH directly from browser
* Install your bot dependencies and start running it
* Use `systemd` or startup scripts to auto-restart after reboot

```bash
gcloud compute ssh my-vm
sudo apt install python3 pip git
git clone https://github.com/yourrepo/cardano_diy_bot
cd cardano_diy_bot && python main.py
```

✅ **Best for:** Long-running bots, full backend control.\
💡 **Tip:** New accounts get \~$300 credits for 90 days.

***

#### **Option 2: Google Cloud Run**

Run your app as a container (Docker-based).\
It scales automatically based on traffic or scheduled events.

**Steps:**

1. Create a `Dockerfile` for your bot
2. Deploy via Cloud Run console or `gcloud run deploy`
3. Set environment variables (API keys, DB credentials)
4. Optional: connect to Pub/Sub or Cloud Scheduler for triggers

✅ **Best for:** APIs, lightweight web services.\
💡 **Note:** Cloud Run sleeps when idle, but wakes instantly.

***

### 8. Google Apps Script Hosting

**Google Apps Script (GAS)** is a cloud-based scripting environment that runs directly inside Google’s infrastructure — no servers, no VMs.

It’s perfect for lightweight automation tasks that connect:

* **APIs** (Koios, Blockfrost, CoinGecko, etc.)
* **Google Sheets / Gmail / Telegram / Discord**
* **Cardano\_DIY\_bot** data workflows

#### Example: Check ADA price and log to Google Sheets

```javascript
function logADA() {
  const url = "https://api.coingecko.com/api/v3/simple/price?ids=cardano&vs_currencies=usd";
  const data = UrlFetchApp.fetch(url);
  const price = JSON.parse(data).cardano.usd;
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("ADA_Price");
  sheet.appendRow([new Date(), price]);
}
```

Set a **time-based trigger** to run every hour, and your script becomes a live data logger — hosted by Google automatically.

#### Integration with Telegram

You can even use Apps Script to:

* Send messages to Telegram via API
* Fetch data from blockchain explorers
* Combine Sheets as your mini database

✅ **Best for:**

* Lightweight automation and daily jobs
* Bots that post updates or summary reports
* Rapid prototypes (no DevOps needed)

***

### 9. Key Hosting Concepts

| Term                      | Description                                         |
| ------------------------- | --------------------------------------------------- |
| **Uptime**                | How long your service stays online (target: 99.9%+) |
| **SSH**                   | Secure login to remote servers                      |
| **Deployment**            | Process of publishing your code to a host           |
| **Environment Variables** | Hidden credentials like API keys or tokens          |
| **Logs**                  | Console output or error tracking from hosted apps   |
| **Cron / Scheduler**      | Time-based jobs (poll blockchain, post updates)     |

When running blockchain-integrated bots, ensure your hosting setup includes **secure storage for keys**, **scheduled jobs**, and **reliable uptime**.

***

### 10. Summary

Hosting is the **engine that powers your automation**.\
It connects the logic, data, and user interaction into one continuous loop.

| Layer             | Function                                                 |
| ----------------- | -------------------------------------------------------- |
| **On-chain Data** | The verified information on Cardano blockchain           |
| **API**           | The gateway to access that information                   |
| **Database**      | Stores results, logs, or processed data                  |
| **Hosting**       | Keeps your app running and connected to all other layers |

> 🚀 Hosting transforms a local script into a live service —\
> from “just code” to a **running bot that interacts with the blockchain** 24/7.
