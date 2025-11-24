# Input

The **Input Layer** defines how your tracker retrieves blockchain data and communicates with Cardano’s public data providers.\
It acts as the _entry point_ of the entire system, where raw **on-chain data** is fetched and converted into structured information for later processing.

***

#### 🔗 Related Knowledge

Before diving into API usage, review the basics in:

* **On-chain Data** — learn what blockchain data represents and why it’s complex.
* **API** — understand how APIs act as bridges between your code and Cardano nodes.

***

#### Why not fetch raw on-chain data?

On-chain data is **massive, complex, and unstructured**.\
It includes every transaction, metadata, and token record ever produced — far beyond what a personal tracker needs.\
Using public APIs lets you focus only on essential wallet or token information while keeping your app fast and simple.

***

#### ⚙️ Why Use Public APIs?

Public APIs such as Blockfrost and [Koios](https://api.koios.rest) are perfect for personal tracking tools because they are:

* 🆓 **Free or freemium** — no node setup required.
* ⚙️ **Developer-friendly** — simple REST endpoints.
* 📦 **Efficient** — fetch only the wallet or token data you need.
* 🔒 **Secure** — API keys replace direct node connections.

They provide clean access to **on-chain information** without running your own Cardano node — saving both time and resources.
