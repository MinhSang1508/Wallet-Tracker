# How to use Blockfrost API

## **How to use Blockfrost**

**Blockfrost** is a public API service that allows developers to access Cardano blockchain data without running a full node.\
For this personal tracker, we only need a few endpoints to **fetch wallet balances, token information, and transactions** — no need for complex queries.

🔗 **Base URL (Mainnet):**

```
https://cardano-mainnet.blockfrost.io/api/v0/
```

🔗 **Api Endpoint:**

```
https://docs.blockfrost.io/
```

👉 For testnet or preprod environments, replace `mainnet` with `preview` or `preprod`.\
Each request must include a **`project_id`** in the header (your API key for authentication).

***

### 🧠 What You Need

Before you start:

* Create a free account at [Blockfrost.io](https://blockfrost.io/).
* Create a **Project** to get your `project_id` (API key).
* Have your **wallet address** or **stake address** ready (e.g. `addr1...` or `stake1...`).
* Know how to make HTTP requests (`curl`, Python, Node.js, or Google Apps Script).
* Keep your `project_id` private — especially if you’re using a free tier.

***

### ⚙️ Step 1: Get Wallet Address Info

This is the simplest and most useful endpoint for tracking wallet balances and tokens.

**Endpoint:**

```
GET /addresses/{address}
```

**Example (using curl):**

```bash
curl -H "project_id: YOUR_PROJECT_ID" \
  "https://cardano-mainnet.blockfrost.io/api/v0/addresses/addr1qxyz..."
```

**Example response (simplified):**

```json
{
  "address": "addr1qxyz...",
  "amount": [
    {
      "unit": "lovelace",
      "quantity": "123456789"
    },
    {
      "unit": "policyid_assetname",
      "quantity": "50"
    }
  ],
  "stake_address": "stake1uabc...",
  "type": "shelley"
}
```

🔹 **Notes:**

* `lovelace` = smallest ADA unit (1 ADA = 1,000,000 lovelace).
* Each token appears under its `policy_id_assetname`.
* Use this response to track both ADA and token balances.

***

### 📊 Step 2: Get Recent Transactions _(optional but useful)_

To detect balance changes or new transactions, fetch recent transaction hashes.

**Endpoint:**

```
GET /addresses/{address}/transactions
```

**Example:**

```bash
curl -H "project_id: YOUR_PROJECT_ID" \
  "https://cardano-mainnet.blockfrost.io/api/v0/addresses/addr1qxyz.../transactions?count=5&order=desc"
```

**Response (simplified):**

```json
[
  { "tx_hash": "abc123...", "block_height": 8220000, "block_time": 1672760000 },
  { "tx_hash": "def456...", "block_height": 8220012, "block_time": 1672760300 }
]
```

This allows you to detect if any ADA or token movement happened since the last check.

***

### 🔄 Step 3: Integrate into Your Tracker

In your tracker logic (Python, Node.js, or Google Apps Script):

1. Call the `/addresses/{address}` endpoint to fetch wallet data.
2. Parse the JSON to extract `lovelace` (ADA balance) and token data.
3. Optionally, call `/transactions` to see if there are new movements.
4. Store results into **Google Sheets** or **Supabase**.
5. Send Telegram or Email alerts if balance or token counts change.

**Pseudo-code example:**

```python
data = get_blockfrost("addresses", wallet_address)
balance = parse_balance(data)
if balance_changed(balance):
    send_telegram("Balance changed!")
write_to_sheet(wallet_address, balance, timestamp)
```

***

### 🚀 Quick Reference

| Use case            | Endpoint                            | Description                       |
| ------------------- | ----------------------------------- | --------------------------------- |
| Wallet info         | `/addresses/{address}`              | ADA and token balances            |
| Recent transactions | `/addresses/{address}/transactions` | List of recent transactions       |
| Asset info _(opt.)_ | `/assets/{asset}`                   | Details of a specific asset/token |

***

### 🧩 Tips & Best Practices

* Always include your **`project_id`** header in every request.
* Respect rate limits: space out your requests (e.g., every 30s or 5min).
* Use **`preprod`** or **`preview`** network for testing before mainnet.
* Store raw responses locally (or in Google Sheets) for debugging.
* Ideal for **personal use** — lightweight, fast, and no node setup required.
