# BitMarket: Decentralized Commerce Protocol

[![Built for Stacks L2](https://img.shields.io/badge/Built_for-Stacks_L2-blue)](https://www.stacks.co)
[![Smart Contracts: Clarity](https://img.shields.io/badge/Smart_Contracts-Clarity-orange)](https://clarity-lang.org)

## Overview

**BitMarket** is a decentralized peer-to-peer commerce protocol designed for trustless trading on **Bitcoin Layer 2**.
It removes intermediaries by leveraging **Clarity smart contracts** and Bitcoin’s security guarantees, enabling merchants and buyers to transact directly with automated escrow, verifiable reputation scoring, and auction-driven price discovery.

The protocol is optimized for:

* Transparent merchant onboarding & verification
* Automated escrow & fee settlements
* Competitive auction systems for dynamic pricing
* Community-driven quality assurance and reviews
* Cross-chain extensibility for future asset support

---

## Key Innovations

* **Zero-Counterparty Risk** – All transactions are cryptographically enforced on Bitcoin L2.
* **Automated Escrow & Disputes** – Funds flow only via contract logic, eliminating intermediaries.
* **Dynamic Auctions** – Competitive bidding with reserve pricing and time-locked expirations.
* **Reputation & Reviews** – Decentralized scoring ensures trust is community-driven.
* **Protocol Fee Model** – Sustainable economics via flexible fee basis points (default: 2.5%).

---

## System Overview

At a high level, the BitMarket protocol operates across **four main layers**:

1. **Merchant Registry** – Verified seller identities with on-chain metadata.
2. **Product Catalog** – Decentralized listings for both direct sale and auction-based items.
3. **Auction Engine** – Competitive bidding framework with escrow, refund, and settlement.
4. **Quality Assurance** – Review and rating system to reinforce credibility.

---

## Contract Architecture

The protocol is implemented as a single Clarity contract with modular components:

### **Constants & Errors**

* Standardized error codes (`ERR_UNAUTHORIZED`, `ERR_PRODUCT_NOT_FOUND`, etc.)
* Configurable protocol fee (`protocol-fee-basis-points`)

### **Core Maps**

* `MerchantRegistry` → Tracks merchant profiles and verification status
* `ProductCatalog` → Inventory of listed products
* `AuctionEngine` → Competitive bidding state for auction listings
* `QualityAssurance` → Product reviews and community ratings

### **State Variables**

* `global-product-counter` → Ensures unique product identifiers
* `protocol-fee-basis-points` → Fee model adjustable by governance/owner

---

## Data Flow (Auction Lifecycle Example)

1. **Merchant Onboarding**

   * Seller calls `register-merchant` → stored in `MerchantRegistry`.
   * Verified by contract owner via `verify-merchant-credentials`.

2. **Auction Initialization**

   * Seller lists product with `initialize-auction`.
   * `ProductCatalog` entry created + auction state set in `AuctionEngine`.

3. **Bidding Process**

   * Bidders submit STX via `submit-bid`.
   * Escrow holds funds under contract custody.
   * Previous bidder automatically refunded.

4. **Auction Finalization**

   * After expiration, seller (or anyone) calls `finalize-auction`.
   * Contract transfers bid (minus protocol fee) to merchant.
   * Product availability updated → ownership confirmed.

5. **Post-Transaction Review**

   * Buyer can leave a rating/review via `submit-product-review`.
   * Stored in `QualityAssurance`, influencing future trust.

---

## Public Functions

### **Merchant**

* `register-merchant (business-name)`
* `verify-merchant-credentials (merchant)`

### **Marketplace**

* `create-product-listing (title details price)`
* `execute-purchase (product-id)`

### **Auction**

* `initialize-auction (title details reserve auction-duration)`
* `submit-bid (product-id bid-amount)`
* `finalize-auction (product-id)`

### **Quality Assurance**

* `submit-product-review (product-id quality-score feedback)`

### **Read-Only Queries**

* `get-product-details`
* `get-merchant-profile`
* `get-product-review`
* `get-auction-status`
* `get-current-product-count`
* `get-protocol-fee-rate`

---

## Example Usage

### Direct Purchase Flow

```clarity
;; Register merchant
(contract-call? .bitmarket register-merchant "CryptoBooks")

;; List product
(contract-call? .bitmarket create-product-listing 
  "Mastering Bitcoin" 
  "Authoritative guide for developers." 
  u1000000)

;; Buyer purchases product
(contract-call? .bitmarket execute-purchase u1)
```

### Auction Flow

```clarity
;; Seller creates auction
(contract-call? .bitmarket initialize-auction 
  "Rare NFT" 
  "Unique digital collectible." 
  u5000000 
  u50)

;; Bidders submit offers
(contract-call? .bitmarket submit-bid u2 u6000000)

;; Auction finalization
(contract-call? .bitmarket finalize-auction u2)
```

---

## Security & Governance

* **Protocol Owner** – Currently manages merchant verification and fee rates.
* **Escrow Enforcement** – All value transfers use `stx-transfer?` under strict assertions.
* **Immutable Audits** – Every action (listing, bid, review) is timestamped on-chain.

Future versions may integrate:

* DAO-based governance for protocol upgrades
* Cross-chain settlements (e.g., BTC, sBTC, stablecoins)
* Decentralized arbitration systems

---

## License

MIT License. Open for community contributions, audit feedback, and ecosystem integration.
