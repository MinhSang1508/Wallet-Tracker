# On-chain data

### 1. Introduction

In traditional systems, data is stored in **private databases**, managed and controlled by centralized entities.\
In blockchain, however, every transaction, balance, and token movement is **publicly verifiable and immutable**.\
This transparent and tamper-proof record is called **on-chain data**.

Understanding on-chain data is fundamental for anyone building tools, bots, or analytics platforms on Cardano.\
It represents the “raw truth” of the network — everything that actually happens on the blockchain.

***

### 2. What Is On-chain Data?

**On-chain data** refers to all information **recorded directly on the blockchain ledger** — information that is validated, stored, and replicated across nodes.\
Once added to a block, it becomes permanent and cannot be modified or deleted.

Examples include:

* Transactions and metadata
* ADA balances and native tokens
* Smart contract states (UTxO datums)
* Stake keys, delegations, and pool data
* Block details: height, hash, slot, timestamp

This data is **public**, **traceable**, and **auditable** by anyone through blockchain explorers or APIs.

***

### 3. Structure of Blockchain Data

At its core, Cardano is a **UTxO-based ledger** (Unspent Transaction Output model).\
Each transaction consumes existing UTxOs and creates new ones.

Here’s how it looks conceptually:

```
[Wallet A] --- sends ADA ---> [Wallet B]

Transaction:
  Inputs: previous UTxOs owned by A
  Outputs: new UTxOs assigned to B
```

Each **UTxO** can contain:

* ADA amount
* Optional **native tokens**
* Optional **datum** (for smart contracts)

A **block** groups multiple transactions, and each block is added sequentially to the chain.\
Together, these form the immutable **on-chain ledger**.

***

### 4. Types of On-chain Data on Cardano

| Category                | Description                                 | Example                          |
| ----------------------- | ------------------------------------------- | -------------------------------- |
| **Transaction Data**    | Records transfers of ADA or tokens          | TX ID, inputs, outputs, metadata |
| **Address Data**        | Tracks ADA or token balance of each address | Wallet holdings                  |
| **Block Data**          | Details of each block                       | Block height, hash, epoch, slot  |
| **Stake Data**          | Delegation and staking pool info            | Stake key, rewards, pool ID      |
| **Smart Contract Data** | Script addresses and datums                 | Plutus contract states           |
| **Token Data**          | Minting/burning records for native assets   | Policy ID, token name, supply    |

This multi-layered structure makes Cardano rich for analytics and automation.

***

### 5. How to Access On-chain Data

While you could run a **full Cardano node**, it’s often easier to use **public APIs** that expose this data directly.

#### 🛰️ Option 1: Koios API

Koios provides **RESTful endpoints** that query live blockchain data efficiently.\
Example:

```bash
curl -X GET "https://api.koios.rest/api/v1/account_info?_address=addr_test1..." \
     -H "Accept: application/json"
```

Response:

```json
{
  "address": "addr_test1...",
  "balance": 2000000,
  "stake_address": "stake_test1...",
  "delegated_pool": "pool1xyz..."
}
```

#### 🧱 Option 2: Blockfrost API

Blockfrost offers a similar service with extended features (pagination, metadata, project tracking).\
Example:

```bash
curl -X GET "https://cardano-preprod.blockfrost.io/api/v0/addresses/addr_test1..." \
     -H "project_id: your_blockfrost_api_key"
```

#### 🧩 Option 3: Direct Node (Ogmios / Koilab)

For deeper use cases, you can connect directly to a Cardano node using **Ogmios (WebSocket bridge)** or **Koilab (GraphQL)** for streaming real-time data.

***

### 6. Why On-chain Data Matters

| Use Case                    | Purpose                                               |
| --------------------------- | ----------------------------------------------------- |
| **Wallet balance tracking** | Monitor addresses and token movements                 |
| **Analytics dashboards**    | Visualize transaction volume, staking stats, etc.     |
| **Bot automation**          | Send Telegram alerts when specific transactions occur |
| **DeFi and DAO tools**      | Verify deposits, votes, or token transfers on-chain   |
| **Compliance and auditing** | Trace wallet activity transparently                   |

In short: **off-chain logic depends on on-chain truth**.\
The on-chain data is the _source of truth_ that drives logic in apps, bots, and smart contracts.

***

### 7. On-chain vs Off-chain Data

| Feature          | On-chain Data                   | Off-chain Data                                 |
| ---------------- | ------------------------------- | ---------------------------------------------- |
| **Storage**      | On the blockchain ledger        | External database (e.g., PostgreSQL, Firebase) |
| **Immutability** | Permanent and transparent       | Mutable and private                            |
| **Verification** | Cryptographically verifiable    | Depends on source trust                        |
| **Speed / Cost** | Slower, incurs transaction fees | Faster, cheaper                                |
| **Examples**     | Transactions, UTxOs, datums     | User settings, cache, logs                     |

Most real-world apps combine both:

* On-chain for **trust and transparency**
* Off-chain for **performance and flexibility**

***

### 8. Tools to Explore On-chain Data

* **Koios Explorer** – REST endpoints for account, pool, and transaction data.
* **Blockfrost Dashboard** – Web-based analytics and project API.
* **CardanoScan** – Public explorer for checking blocks and transactions.
* **Adastat** – Community explorer with staking data.
* **Ogmios** – Lightweight bridge for real-time node queries.

These tools help developers verify, monitor, and test data flows before integrating them into bots or dApps.

***

### 9. Using On-chain Data in Automation (Example)

For the **Cardano\_DIY\_bot**, on-chain data is the foundation of the workflow:

1. **Bot Input:**
   * Fetch wallet UTxO and transaction data via Koios or Blockfrost API.
2. **Middle Logic:**
   * Parse the JSON data and identify if a transaction matches specific conditions (e.g., deposit > 10 ADA).
3. **Output:**
   *   Send Telegram alert:

       > “💰 Wallet X received 10 ADA from Y at 12:03 UTC.”

Example pseudocode:

```python
import requests

address = "addr_test1..."
url = f"https://api.koios.rest/api/v1/account_info?_address={address}"
data = requests.get(url).json()
balance = data[0]["balance"]
print(f"Current ADA balance: {balance / 1_000_000} ADA")
```

***

### 10. Challenges in Working with On-chain Data

| Issue                  | Description                         | Solution                                   |
| ---------------------- | ----------------------------------- | ------------------------------------------ |
| **Large datasets**     | Blockchain grows continuously       | Use pagination or snapshot APIs            |
| **Data latency**       | Node sync delay between networks    | Use reliable endpoints (Koios, Blockfrost) |
| **Complex UTxO model** | One address can hold many UTxOs     | Aggregate results carefully                |
| **Testnet vs Mainnet** | Different endpoints and ADA amounts | Always confirm network type                |

***

### 11. Summary

On-chain data is the **core of blockchain transparency**.\
Every transaction, stake, and token interaction is permanently recorded and publicly accessible.\
By learning to read and process this data, developers can:

* Build analytics dashboards
* Power automated bots
* Validate contract logic
* Audit blockchain activity

In the Cardano ecosystem, tools like **Koios** and **Blockfrost** make this process straightforward through standard REST APIs.

> 🌍 On-chain data is not just information — it’s the public memory of the blockchain.
