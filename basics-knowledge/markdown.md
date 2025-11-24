# API

### 1. Introduction

In the connected world, every modern application — from blockchain wallets and chatbots to data platforms and exchanges — needs to **exchange data**.\
The technology that makes this communication possible is called an **API (Application Programming Interface)**.

In simple terms, an API is a **common language between software systems**.\
It works like a restaurant menu: you place an order (a request), and the kitchen (the system) prepares the dish (the response).\
You don’t need to know how it’s cooked — just how to ask for it.

***

### 2. What is an API?

**API (Application Programming Interface)** is a set of rules, methods, and data formats that allow one software component to **access or interact** with another.

Examples:

* A Cardano wallet uses **Koios API** to fetch ADA balances.
* A Telegram bot sends messages through the **Telegram API**.
* A price-tracking dashboard queries **CoinGecko API** for ADA price updates.

With APIs, developers can reuse existing services instead of building everything from scratch.\
This increases **development speed**, **compatibility**, and **scalability** across systems.

***

### 3. Common Types of APIs

| API Type                    | Description                                                       | Example                           |
| --------------------------- | ----------------------------------------------------------------- | --------------------------------- |
| **Web API (HTTP API)**      | Communicates over HTTP using methods like GET, POST, PUT, DELETE. | Koios, Blockfrost, CoinGecko      |
| **Library API**             | Functions exposed by software libraries.                          | React API, Python Pandas          |
| **Operating System API**    | Lets apps interact with the OS.                                   | Windows API, POSIX                |
| **Hardware API**            | Interfaces between software and hardware.                         | GPU API, camera API               |
| **Wallet / Blockchain API** | Connects dApps and wallets.                                       | CIP-30 Wallet API, Blockfrost API |

***

### 4. Anatomy of a Web API

Most APIs today are **Web APIs**, which use HTTP and exchange data in **JSON** format.

Example:

```bash
curl -X GET "https://api.koios.rest/api/v1/account_info?_address=addr_test1..." \
     -H "Accept: application/json"
```

Every HTTP request has three key parts:

1. **Endpoint** – the URL path for the resource, e.g. `/api/v1/account_info`.
2. **Method (HTTP verb)** – defines the type of action:
   * `GET`: retrieve data
   * `POST`: send new data
   * `PUT`: update data
   * `DELETE`: remove data
3. **Headers / Body** –
   * Headers: metadata (authentication, content type, etc.)
   * Body: data payload (usually for POST or PUT requests)

A typical API response looks like:

```json
{
  "address": "addr_test1...",
  "balance": 2000000,
  "stake_address": "stake_test1..."
}
```

***

### 5. How APIs Work

The communication process is simple:

1. The **client** (user, app, or bot) sends a **request** to a **server**.
2. The server processes it, retrieves the required data, and returns a **response**.
3. The client displays or uses the returned data.

Example (checking a Cardano wallet balance):

```
[User] → [Telegram Bot] → [Koios API] → [Cardano Node]
```

The bot requests the wallet address data, Koios returns the balance, and the bot replies to the user.

***

### 6. API Key and Security

Most APIs require an **API Key** — a unique token that identifies and authorizes the client.

Example with Blockfrost:

```bash
-H "project_id: your_blockfrost_api_key"
```

Common API security measures:

* **HTTPS**: encrypts communication between client and server.
* **Token / JWT**: temporary access credentials.
* **OAuth 2.0**: used for user authentication (Google, Discord, etc.).
* **Rate limiting**: prevents spam by restricting request frequency.

***

### 7. REST, GraphQL, and gRPC

| Type        | Description                                 | Advantages          | Disadvantages             |
| ----------- | ------------------------------------------- | ------------------- | ------------------------- |
| **REST**    | Uses URLs and HTTP methods                  | Simple, universal   | Can return redundant data |
| **GraphQL** | Client specifies exactly what data to fetch | Flexible, efficient | Complex to cache/debug    |
| **gRPC**    | Binary protocol                             | Fast, lightweight   | Requires special setup    |

Most Cardano-related services (Koios, Blockfrost, Koilab, etc.) are **RESTful APIs** — simple, human-readable, and widely supported.

***

### 8. APIs in the Cardano Ecosystem

| API                   | Function                                                          | Use Case                               |
| --------------------- | ----------------------------------------------------------------- | -------------------------------------- |
| **Koios API**         | Fetches on-chain data: UTxOs, transactions, pools, staking info.  | Wallet analytics, custom explorers     |
| **Blockfrost API**    | REST service providing full-node blockchain data.                 | DApps, dashboards, monitoring tools    |
| **CIP-30 Wallet API** | Allows web dApps to connect to user wallets (Eternl, Lace, Nami). | Web3 integration, sending transactions |
| **CoinGecko API**     | Provides market data and token prices.                            | Price alert bots, dashboards           |
| **Telegram API**      | Manages messaging, updates, and callbacks for bots.               | Cardano\_DIY\_bot notifications        |

Together, these APIs create a connected architecture linking **frontend → backend → blockchain → user**.

***

### 9. Common API Errors

| Status Code                   | Meaning                              | How to Fix                  |
| ----------------------------- | ------------------------------------ | --------------------------- |
| **400 Bad Request**           | Malformed JSON or missing parameters | Check body and syntax       |
| **401 Unauthorized**          | Invalid or missing API key           | Add or refresh API key      |
| **404 Not Found**             | Endpoint doesn’t exist               | Verify the URL path         |
| **429 Too Many Requests**     | Exceeded rate limit                  | Wait or upgrade your plan   |
| **500 Internal Server Error** | Server-side issue                    | Retry or report the problem |

***

### 10. APIs and Automation

In the **Cardano\_DIY\_bot** project, APIs form the backbone of the automation pipeline:

* **Input Layer:** Fetch blockchain data from Koios or Blockfrost (transactions, tokens, stake info).
* **Middle Control:** Process logic in Python/Node.js and store results in a database.
* **Output Layer:** Send messages or alerts using Telegram API or Email API.

This architecture allows the entire workflow — checking wallet balances, monitoring transactions, sending alerts — to run **automatically**, without manual intervention.

***

### 11. API Documentation

A reliable API always provides comprehensive documentation, usually including:

* A list of available endpoints
* Input parameters
* Example responses
* Authentication details
* Rate limits or quotas

Many modern APIs follow the **OpenAPI / Swagger** standard, which allows users to **try out requests directly in the browser**.

Examples:

> https://api.koios.rest\
> https://docs.blockfrost.io\
> https://core.telegram.org/bots/api

Documentation is as important as the API itself — it ensures consistency, collaboration, and easier debugging.

***

### 12. Conclusion

APIs are the **connective tissue** of the digital ecosystem.\
In blockchain, they enable dApps, wallets, bots, and analytics tools to work together seamlessly.

Understanding APIs helps developers:

* **Access and interpret on-chain data.**
* **Automate backend and bot operations.**
* **Integrate wallets and third-party services** into dApps.

APIs don’t just connect code — they connect ecosystems.

> 🧠 “AI speaks human language.\
> Developers speak the language of systems — through APIs.”
