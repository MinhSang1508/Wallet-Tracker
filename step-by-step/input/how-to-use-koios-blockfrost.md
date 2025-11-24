# How to use Koios API

## **How to use Koios**

The **Koios API** is a community-maintained, open API for querying the Cardano blockchain.\
In this tracker, we use only a few simple endpoints to **fetch wallet and asset data** — perfect for personal tracking or lightweight automation.

🔗 **Koios API Base URL:**

> https://api.koios.rest/api/v1/

***

### 🧠 What You Need

Before using Koios, make sure you have:

* A **stake address** or **wallet address** (starts with `addr1...` or `stake1...`).
* A way to make HTTP requests — e.g. `curl`, Python, Node.js, or Google Apps Script.
* Internet connection (Koios is a public REST API, no API key required).

***

### ⚙️ Step 1: Get Wallet Balance

Use this endpoint to get the current balance and token list in your wallet:

```
GET /address_info
```

**Example (using `curl`):**

```bash
curl -X POST https://api.koios.rest/api/v1/address_info \
  -H "Content-Type: application/json" \
  -d '{"_addresses": ["addr1q9xyz..."]}'
```

**Response (simplified):**

```json
[
  {
    "address": "addr1q9xyz...",
    "balance": "18374520",
    "stake_address": "stake1u9abc...",
    "assets": [
      { "policy_id": "asset123...", "asset_name": "TOKEN1", "quantity": "50" },
      { "policy_id": "asset456...", "asset_name": "TOKEN2", "quantity": "10" }
    ]
  }
]
```

💡 _You’ll receive both ADA and native token balances._

***

### 📊 Step 2: Get Stake Info (optional)

If your wallet is staked, you can check delegation status and rewards.

```
GET /stake_accounts
```

**Example:**

```bash
curl -X POST https://api.koios.rest/api/v1/stake_accounts \
  -H "Content-Type: application/json" \
  -d '{"_stake_addresses": ["stake1u9abc..."]}'
```

**Response:**

```json
[
  {
    "stake_address": "stake1u9abc...",
    "delegated_pool": "pool1xyz...",
    "reward_balance": "284920",
    "active_epoch": 456
  }
]
```

***

### 🔄 Step 3: Integrate into the Tracker

In your tracker logic (Python, Node.js, or Apps Script):

1. Query Koios using your wallet address.
2. Parse the JSON response to extract `balance`, `assets`, and `rewards`.
3. Store them into **Google Sheets** or send alerts if there’s a change.

Example structure in pseudocode:

```python
data = get_koios("address_info", wallet)
store_to_sheet(data)
if balance_changed(data):
    send_telegram("Balance changed!")
```

***

### 🚀 Quick Notes

| Use case                      | Endpoint          | Method | Description               |
| ----------------------------- | ----------------- | ------ | ------------------------- |
| Wallet balance                | `/address_info`   | POST   | ADA & token balance       |
| Stake info                    | `/stake_accounts` | POST   | Delegation / rewards      |
| Transaction list _(optional)_ | `/address_txs`    | POST   | Fetch recent transactions |

***

### 🧩 Tips

* Koios endpoints are **free** and don’t need authentication.
* Always add short delays between API calls (e.g., 0.5–1s) to avoid rate limits.
* For personal trackers, Koios is **lighter and easier** than using a full Cardano node.
