# Cardano Asset

## 1. Introduction

This page explains how Cardano native assets work and how your Asset Tracker interacts with them.  
Before configuring scanning, indexing, or automation logic, it’s important to understand how assets are structured on-chain and how they appear inside UTxOs.

Understanding this section is required for:

- fetching asset balances  
- reading policy IDs  
- decoding token names  
- tracking mint/burn events  
- analyzing holders  
- parsing UTxO values in your tracker backend  

---

## 2. What Is a Cardano Native Asset?

A **Cardano native asset** is any token that exists directly on the Cardano ledger.  
Unlike Ethereum tokens, native assets do **not** require smart contracts — they are handled at the ledger level.

For an asset tracker, this means:

- predictable behavior  
- no contract ABI required  
- uniform representation inside UTxO values  
- consistent tracking across all tokens  

Every native asset is identified by:

<policy_id>.<asset_name>

makefile
Copy code

Example:

4f89ac...91aa.TokenA

yaml
Copy code

---

## 3. How Assets Appear in UTxOs

Your tracker fetches UTxOs from a node, Koios, Blockfrost, Ogmios, or Kupo.  
Each UTxO includes a **value object** that lists all assets it contains.

Example:

```json
{
  "lovelace": 2500000,
  "a1b2c3d4f0...": {
    "434f494e": 1500,
    "4d594e4654": 1
  }
}
A tracker must handle:

policy ID

asset name (hex-encoded)

quantity

Key notes for implementation
Decode asset name from hex → UTF-8 (if printable)

Some assets use an empty asset name

A policy may contain multiple asset names

Large value maps require optimized parsing

4. Policy ID and Minting Rules
Every asset is governed by a monetary policy, defined by a script that controls minting/burning.
Your tracker must be able to:

identify the policy ID

detect mint/burn events

determine if the policy is locked

Mint events appear in:

pgsql
Copy code
transaction.body.mint
Example:

json
Copy code
{
  "policy_id": {
    "TokenA": 500
  }
}
A positive number = minting
A negative number = burning

5. Asset Metadata
Some assets include metadata describing the token:

name

ticker

decimals

description

image/logo

Metadata sources:

Transaction metadata (CIP-25 / CIP-68)

Reference NFT metadata (CIP-68)

Cardano Token Registry (CIP-10, off-chain)

Your tracker should:

fetch metadata when available

fall back to registry metadata

cache metadata for performance

Note: Not all assets include metadata.

6. Types of Assets Your Tracker Must Handle
1. Fungible Tokens (FT)
multiple units

similar to ERC-20

quantity-based tracking

2. NFTs
usually quantity = 1

metadata-heavy

CIP-25 or CIP-68 based

3. SFTs & CIP-68 Assets
semi-fungible tokens

mutable metadata

require parsing reference NFT structure

Your tracker must not assume:

decimals always exist

tickers always exist

metadata is always present

7. How Your Tracker Uses Asset Data
The Asset Tracker uses asset information for multiple features:

a. Balance tracking
Parse UTxO → extract value → aggregate by asset.

b. Holder analysis
Scan:

UTxOs

transfers

mint/burn events

c. Mint/Burn detection
Read the mint field in each transaction.

d. Supply & history analytics
Compute:

supply over time

holder changes

movement charts

e. Alerts & automation
Trigger alerts when:

supply changes

large transfers occur

a specified wallet receives a token

8. Implementation Notes
For a reliable tracker:

decode asset names correctly

normalize token identifiers

store both hex and UTF-8 names

track negative mint values (burn)

paginate UTxO/API queries

pre-index relevant policy IDs

cache metadata

9. Summary
Cardano native assets are simple and consistent, making them ideal for tracking and automation.

Your tracker will rely on:

UTxO parsing

policy ID and asset name handling

mint/burn recognition

metadata retrieval

With these fundamentals, your Asset Tracker can provide accurate balance tracking, analytics, and automation for all Cardano assets.
