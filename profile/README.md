<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/ducatprotocol/.github/main/assets/banner-dark.png">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/ducatprotocol/.github/main/assets/banner-light.png">
  <img alt="DUCAT Protocol" src="https://raw.githubusercontent.com/ducatprotocol/.github/main/assets/banner-light.png">
</picture>

<p align="center">
  <strong>The first Bitcoin L1-native, decentralized stablecoin.</strong>
</p>

<p align="center">
  <a href="https://ducatprotocol.com">Website</a> •
  <a href="https://docs.ducatprotocol.com">Documentation</a> •
  <a href="https://twitter.com/Ducatstable">Twitter</a> •
  <a href="https://discord.gg/r6Q8vPdtr3">Discord</a>
</p>

---

## Money Should Be Free

For too long, the promise of Bitcoin has been constrained by an impossible choice: **integrate into the financial system, or maintain sovereign custody.**

We reject this false dichotomy.

The current stablecoin landscape is dominated by centralized custodians—offshore treasuries, opaque collateral, and off-chain control. These models carry unlimited regulatory liability, censorship risk, and fundamental trust assumptions that betray everything Bitcoin was built to overcome.

An estimated **89% of Bitcoin—roughly $1.9 trillion—sits completely idle and unleveraged.** Not because holders don't want access to liquidity, but because every existing option demands they forfeit the very property rights that make Bitcoin valuable.

**DUCAT changes this.**

We believe a truly decentralized economy requires truly decentralized money. Money that can't be frozen. Collateral that can't be seized. A protocol that runs on Bitcoin itself—no bridges, no wrapped tokens, no custodians holding the keys to your freedom.

DUCAT is built for the 89%. For the holders who refuse to compromise. For those who understand that self-custody isn't a feature—it's the entire point.

> *"The centralised custody of collateral inherent in existing models undermines everything that makes Bitcoin unique."*

---

## What is DUCAT?

**DUCAT** is a meta-protocol built directly on Bitcoin Layer 1. It enables users to borrow **UNIT**—a USD-pegged stablecoin—against their BTC as collateral, without ever giving up custody of their coins.

| | |
|---|---|
| 🔒 **Self-Custodial** | Your keys, your coins. 2-of-2 multi-sig means no one can spend without you. |
| ⛓️ **Bitcoin-Native** | No sidechains. No bridges. No wrapped tokens. Just Bitcoin. |
| 🔍 **Fully Transparent** | All protocol state is recorded on-chain and independently verifiable. |
| ⚡ **Permissionless** | Open liquidations, open participation, open governance. |

### How It Works

```
┌─────────────────┐     deposit BTC      ┌─────────────────┐
│                 │ ──────────────────►  │                 │
│   You (Client)  │                      │  Vault (2-of-2) │
│                 │ ◄──────────────────  │                 │
└─────────────────┘    receive UNIT      └─────────────────┘
```

1. **Deposit** Bitcoin into a vault (multi-sig with a guardian)
2. **Borrow** UNIT against your collateral (minimum 160% collateral ratio)
3. **Use** UNIT freely—trade, spend, hold
4. **Repay** when ready to reclaim your Bitcoin

If your collateral ratio drops below 135%, your vault becomes eligible for liquidation—an open, permissionless process that keeps UNIT fully backed.

---

## Quick Links

| Resource | Description |
|----------|-------------|
| [📖 Documentation](https://docs.ducatprotocol.com) | Full protocol documentation |
| [🚀 Launch App](https://app.ducatprotocol.com) | Start using DUCAT |
| [📊 Protocol Stats](https://stats.ducatprotocol.com) | Live metrics and TVL |
| [🏛️ Governance](https://docs.ducatprotocol.com/governance/governance) | Participate in protocol governance |

---

## Repositories

| Repository | Description |
|------------|-------------|
| [`ducat-sdk`](https://github.com/ducatprotocol/ducat-sdk) | Client SDK for building on DUCAT |
| [`ducat-node`](https://github.com/ducatprotocol/ducat-node) | Validator node implementation |
| [`ducat-contracts`](https://github.com/ducatprotocol/ducat-contracts) | Protocol contract specifications |
| [`ducat-docs`](https://github.com/ducatprotocol/ducat-docs) | Documentation source |

---

## Get Involved

**Run a Node** — Help secure the network by running a validator

**Become a Guardian** — Earn fees by co-signing vault transactions

**Participate in Governance** — Stake DUCAT tokens and vote on protocol changes

**Build on DUCAT** — Integrate UNIT into your wallet, exchange, or application

---

<details>
<summary><h2>📋 Technical Specification</h2></summary>

# DUCAT Protocol: Executive Summary

## Table of Contents

1. [Overview](#overview)
2. [What is DUCAT?](#what-is-ducat)
3. [The UNIT Stablecoin](#the-unit-stablecoin)
   - [Example: Opening a Vault](#example-opening-a-vault)
   - [UNIT Lifecycle](#unit-lifecycle)
   - [Why UNIT?](#why-unit)
4. [Key Participants](#key-participants)
   - [Clients](#clients)
   - [Guardians](#guardians)
   - [Oracles](#oracles)
   - [Validators](#validators)
   - [Foundation](#foundation)
   - [Fee Structure](#fee-structure)
5. [How Vaults Work](#how-vaults-work)
   - [Custody Model](#custody-model)
   - [Vault Actions](#vault-actions)
   - [Transaction Flow](#transaction-flow)
6. [Custody and Security Model](#custody-and-security-model)
   - [Protocol Governance](#protocol-governance)
   - [Asset Security](#asset-security)
   - [Vault Security](#vault-security)
   - [Trust Assumptions](#trust-assumptions)
   - [Risks and Mitigations](#risks-and-mitigations)
7. [Liquidation Mechanism](#liquidation-mechanism)
   - [How Liquidation Works](#how-liquidation-works)
   - [Post-Liquidation](#post-liquidation)
   - [Liquidation Economics](#liquidation-economics)
   - [Example: Liquidation](#example-liquidation-continuing-alices-story)
   - [Foundation Backstop](#foundation-backstop)
8. [For Businesses](#for-businesses)
9. [Summary](#summary)
10. [Glossary](#glossary)

---

## Overview

The DUCAT Protocol enables users to borrow a USD-pegged stablecoin called **UNIT** against their Bitcoin as collateral. Rather than selling Bitcoin to access liquidity, users lock their BTC in a vault and receive UNIT in return. When they're ready, they repay the UNIT to reclaim their Bitcoin.

DUCAT is a **meta-protocol** — it operates entirely on the Bitcoin blockchain using standard Bitcoin transactions. There are no sidechains, bridges, or wrapped tokens. All protocol state is recorded on-chain and can be independently verified.

Custody is shared between the user and a protocol-approved counter-party (the **guardian**) using a 2-of-2 multi-signature arrangement. Neither party can spend vault funds unilaterally. The user retains control, while the guardian enforces protocol rules. The user can enroll multiple guardians for redundancy.

To maintain the stability of the UNIT/USD peg, vaults must remain over-collateralized. If a vault's collateral ratio falls below the minimum threshold, a liquidation mechanism is triggered. This allows liquidators to claim and recapitalize the vault, ensuring that UNIT remains fully backed.

Liquidation is regulated using a hash-lock mechanism provided by a protocol-approved price oracle. When the target BTC/USD price is reached, the price oracle publishes a "liquidation key". Liquidators and vault guardians use this key to remove the lock and process the liquidation.

The protocol is governed by holders of the **DUCAT** token, who can propose and vote on changes to protocol parameters, elect guardians and oracles, and direct the foundation responsible for protocol management.

---

## What is DUCAT?

DUCAT is a **meta-protocol** built on top of Bitcoin. It is an opt-in consensus layer where all protocol state is stored on the Bitcoin blockchain. Rather than creating a separate layer or chain, DUCAT uses standard Bitcoin transactions to:

- Lock Bitcoin as collateral in a "vault" transaction.
- Issue and track the UNIT stablecoin (using the Runes protocol).
- Record debt positions, price commitments, and protocol state on-chain.
- Enable decentralized governance through token-based voting.

**Key Characteristics:**

| Feature | Description |
|---------|-------------|
| **Bitcoin-Native** | All operations occur on the Bitcoin blockchain |
| **No Bridges** | No wrapped tokens or cross-chain dependencies |
| **Transparent** | Protocol state is fully verifiable on-chain |
| **Self-Governing** | Rules can be updated through on-chain governance |

The protocol is initialized through a bitcoin transaction called the **Anchor Contract**. This transaction publishes data that establishes the initial rules, authorized participants, and asset definitions. Validator nodes index this contract and all subsequent transactions to maintain a consistent view of the protocol state.

> *Note: Validators identify which contract they follow by its transaction ID (txid), ensuring that all participants can verify they are operating on the same contract.*

---

## The UNIT Stablecoin

UNIT is a **USD-pegged stablecoin** backed by Bitcoin collateral, where **1 UNIT = 1 USD**. It is designed to maintain price stability through over-collateralization and automatic liquidation mechanics.

| Parameter | Default Value | Description |
|-----------|---------------|-------------|
| **Minimum Collateral Ratio** | 160% | Required collateral to borrow UNIT |
| **Liquidation Threshold** | 135% | Collateral ratio at which vault becomes liquid |
| **Interest Rate** | 0% | No ongoing interest on UNIT debt |

> *Note: These parameters are defined in the anchor contract and can be updated through governance.*

**How UNIT is Created:**

1. A user deposits Bitcoin into a vault.
2. The vault locks the Bitcoin in a multi-signature contract.
3. UNIT is issued against the collateral at a defined collateral ratio.
4. The debt is recorded on-chain in the vault transaction.

### Example: Opening a Vault

Alice wants to borrow UNIT against her Bitcoin without selling it.

**Setup:**
- Current BTC price: **$100,000**
- Alice deposits: **1 BTC** ($100,000 collateral value)
- Alice borrows: **50,000 UNIT** ($50,000)

**Collateral Ratio Calculation:**
```
Collateral Ratio = Collateral Value / Debt
                 = $100,000 / $50,000
                 = 200%
```

Since 200% > 160% minimum, the vault is healthy.

**Liquidation Price Calculation:**
```
Liquidation occurs when: Collateral Value / Debt = 135%

Collateral Value at liquidation = 50,000 × 1.35 = $67,500
Liquidation Price = $67,500 / 1 BTC = $67,500
```

**Summary:**

| Metric | Value |
|--------|-------|
| Deposited | 1 BTC |
| Borrowed | 50,000 UNIT |
| Collateral Ratio | 200% |
| Liquidation Price | $67,500 |
| Maximum Borrow (at 160%) | 62,500 UNIT |

Alice receives 50,000 UNIT to use freely. As long as BTC stays above $67,500, her vault remains safe. If BTC drops to $67,500 (and Alice did not deposit more collateral), her vault becomes eligible for liquidation.

**How UNIT is Tracked:**

UNIT uses the Runes protocol for basic on-chain accounting of alternative assets. This allows assets such as UNIT to be custodied and exchanged freely on the bitcoin blockchain.

The DUCAT protocol extends Runes by creating an additonal layer of validation. This layer distinguishes between "active" and "reserve" (inactive) rune balances. Only properly issued UNIT is marked as active and recognized by validators as having value. Reserve UNIT is considered worthless.

### UNIT Lifecycle

The following diagram shows how UNIT flows through the protocol:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              UNIT LIFECYCLE                                 │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │  FULL SUPPLY │      All UNIT minted up-front at protocol genesis
    │    MINTED    │      Entire supply starts in RESERVE status
    └──────┬───────┘
           │ rune mint tx
           ▼
    ┌──────────────┐
    │  FOUNDATION  │      Holds reserve UNIT in the Treasury
    │  (reserve)   │      Distributes to Guardians
    └──────┬───────┘
           │ allocate reserves
           ▼
    ┌──────────────┐
    │  GUARDIANS   │      Receive reserve allocations
    │  (reserve)   │      Issue to Clients via vault transactions
    └──────┬───────┘
           │ issue via vault open/borrow
           ▼
    ┌──────────────┐
    │   CLIENTS    │      UNIT becomes ACTIVE upon valid issuance
    │   (active)   │      Can transfer, trade, or use freely
    └──────┬───────┘
           │
           ├─────────────────────────────────────────┐
           │ transfer/trade                          │ repay debt
           ▼                                         ▼
    ┌───────────────┐                         ┌─────────────┐
    │  OPEN MARKET  │                         │   BURNED    │
    │   (active)    │                         │ (destroyed) │
    └───────────────┘                         └─────────────┘
```

**Key States:**
- **Reserve**: Minted but not yet issued—economically inactive, cannot be traded as valid UNIT.
- **Active**: Properly issued through a vault transaction—recognized by validators as valid UNIT.
- **Burned**: Destroyed via repayment or liquidation—permanently removed from circulation.

### Why UNIT?

UNIT provides utility for several use cases:

| Use Case | Description |
|----------|-------------|
| **Stable USD Position** | Hold a USD-denominated asset on Bitcoin, insulated from BTC/USD volatility |
| **Borrow Without Selling** | Access liquidity against your BTC holdings without triggering a taxable sale event (requires UNIT/USD exchange) |
| **Long/Short BTC Exposure** | Participate in an open, auditable market for BTC/USD price speculation |

Unlike centralized stablecoins, UNIT issuance and redemption is fully transparent and verifiable on the Bitcoin blockchain.

**How UNIT Maintains the Peg:**

- **Over-collateralization**: Vaults must maintain a minimum collateral ratio
- **Liquidation**: Under-collateralized vaults are liquidated before debt exceeds collateral
- **Market incentives**: Liquidators profit by acquiring collateral at a discount
- **Treasury**: Accumulated liquidation proceeds fund backstop operations

---

## Key Participants

The DUCAT protocol involves five types of participants:

```
                       ┌─────────────────────────────────────────────────┐
                       │                   VALIDATORS                    │
                       │   (Index transactions, enforce consensus,       │
                       │    track asset balances, serve data)            │
                       └─────────────────────────────────────────────────┘
                                               ▲
                                               │ observe all
                                               │ transactions
         ┌─────────────────────────────────────┼────────────────────────────────────┐
         │                                     │                                    │
         ▼                                     ▼                                    ▼
┌─────────────────┐   UNIT reserves   ┌─────────────────┐    issue UNIT     ┌─────────────────┐
│                 │ ────────────────► │                 │ ────────────────► │                 │
│   FOUNDATION    │                   │    GUARDIANS    │                   │     CLIENTS     │
│                 │ ◄──────────────── │                 │ ◄──────────────── │                 │
└─────────────────┘   liquidation $   └─────────────────┘   co-sign vaults  └─────────────────┘
         ▲                                   ▲   │                                   ▲
         │                                   │   │                                   │
         │                                   │   │                           price contracts &
         │                                   │   │                           liquidation keys
         │                                   │   │  protocol info &                  │
         │       election / replacement      │   │  breached contracts      ┌────────┴────────┐
         └────────────────┬──────────────────┘   └─────────────────────────►│      PRICE      │
                          ├────────────────────────────────────────────────►│     ORACLES     │
                   ┌──────┴──────┐    election / replacement                └─────────────────┘
                   │    DUCAT    │
                   │   HOLDERS   │
                   └─────────────┘
                    also control:
                    - protocol terms
                    - issuance & repayment
```

**Key Relationships:**
- **Foundation → Guardians**: Distributes reserve UNIT to guardians for issuance.
- **Guardians → Oracles**: Provides info for price contract generation and breached contracts.
- **Guardians ↔ Clients**: Co-sign vault transactions, issue active UNIT to clients.
- **Oracles → Clients**: Publish price contracts and liquidation keys for vault transactions.
- **Validators**: Observe all transactions, maintain consensus, validate asset states
- **DUCAT Holders**: Elect Foundation and Guardians; set protocol terms for issuance, repayment and liquidation

### Clients (Borrowers / Liquidators)

End-users who interact with the protocol to:
- Open vaults and deposit Bitcoin collateral.
- Borrow UNIT against their collateral.
- Manage their positions (deposit more, repay debt, withdraw).
- Close vaults and reclaim their Bitcoin.
- Participate in liquidations and claim unhealthy vaults.

Clients use the DUCAT Client SDK to construct transactions and interact with guardians.

### Guardians

Protocol-approved entities that:
- Act as co-signers on vault transactions (2-of-2 multi-sig).
- Validate that vault operations follow protocol rules.
- Issue UNIT to clients from reserve allocations provided by the Foundation.
- Coordinate with oracles (provide rate bucket info, notify of breached contracts).
- Execute liquidations when price oracles release keys.

Guardian public keys are listed in the anchor contract, and their eligibility is controlled by protocol governance. Guardians cannot unilaterally spend vault funds under normal operation.

**Multiple Guardian Support:**

Clients can include multiple guardians in their vault for redundancy. Each guardian has their **own independent 2-of-2 spending path** with the client:

```
Vault Spending Paths:
├── Path 1: Client + Guardian A  ─► Can spend
├── Path 2: Client + Guardian B  ─► Can spend
└── Path 3: Client + Guardian C  ─► Can spend
```

- **Only one guardian** needs to participate in any vault operation.
- If one guardian becomes unavailable, the client can use another.
- Guardian keys can be **added or removed** by the client through a vault transaction.
- This provides resilience without requiring multi-party coordination.

> *Note: Discovery of guardian servers is outside the protocol scope — guardians may publish their connection details on external platforms, using their public key as an identifier.*

**Oracle Coordination:**

Price oracles do not interact with the DUCAT protocol directly—they simply observe BTC/USD prices and publish hash-locked price contracts.

Guardians provide protocol information to the oracles so they can generate these contracts according to protocol rules, and notify oracles when price contracts are breached:

```
┌────────────────┐       protocol info          ┌───────────────┐
│                │ ───────────────────────────► │               │
│   GUARDIANS    │                              │    ORACLES    │
│                │ ───────────────────────────► │               │
└────────────────┘     breached contracts       └───────────────┘
        ▲                                               │
        │                                               │ generate price contracts
        │ vault actions                                 │ at each bucket level
        │ and liquidations                              │ 
        │                                               ▼
┌────────────────┐       price contracts        ┌───────────────┐
│                │ ◄─────────────────────────── │               │
│    CLIENTS     │                              │    RELAYS     │
│                │ ◄─────────────────────────── │               │
└────────────────┘       liquidation keys       └───────────────┘
```

**How Rate Bucketing Works:**
1. The protocol contract defines **rate buckets**—discrete collateral ratio thresholds.
2. Guardians provide these bucket definitions to the price oracles.
3. Oracles **generate price contracts** for each bucket level, and publish them online.
4. When clients open vaults, they retrieve contracts at their specific liquidation price bucket.
5. When a price threshold is breached, guardians notify oracles to **release liquidation keys** for all vaults within that bucket.

**Example Rate Bucket Configuration:**

| Parameter | Value | Description |
|-----------|-------|-------------|
| Minimum Rate | 160% | Lowest collateral ratio (highest leverage) |
| Maximum Rate | 500% | Highest collateral ratio (lowest leverage) |
| Step Size | 1% | Granularity between buckets |

This creates buckets at 160%, 161%, 162%, ... up to 500%—a total of 341 discrete levels. Each bucket corresponds to a specific liquidation price for a given debt amount.

This system eliminates the need for oracles to process individual client requests, dramatically reducing latency and removing the oracle as a communication bottleneck.

### Oracles

Independent parties that:
- Observe the BTC/USD exchange rate
- Issue cryptographic **price contracts** with specific threshold prices
- Release **liquidation keys** when the price threshold is breached

Oracles use hash-locks to control liquidation paths. When the BTC price drops to a vault's liquidation threshold, the oracle releases the corresponding key, enabling guardians to execute the liquidation.

**Price Contract Lifecycle:**
- Price contracts are **valid indefinitely** once created
- Contracts are stored **on-chain** within the vault transaction
- No expiration or renewal required—the contract remains active until the vault is updated or closed

### Validators

Node operators that:
- Index all protocol transactions from the anchor contract forward
- Track asset balances, vault states, and governance decisions
- Provide clients with current protocol data
- Enforce protocol rules through consensus

Validators determine which UNIT balances are considered "active" based on whether their issuance followed protocol rules.

### Foundation

The protocol-elected entity that performs management functions for the DUCAT protocol:

**Core Responsibilities:**
- **Reserve Distribution**: Allocating reserve UNIT to guardians (for issuance to clients).
- **Treasury Management**: Managing the Treasury (liquidation tax profits)
- **Market Stabilization**: Providing UNIT/BTC exchange liquidity
- **Backstop Operations**: Purchasing stale and underwater vaults to defend the peg
- **Protocol Enhancement**: Funding initiatives that benefit the protocol

**Authority and Accountability:**

The Foundation can be a single signer or a multi-signature arrangement. While the Foundation has discretionary authority over reserve distribution and Treasury spending (including taking compensation for their services), they are accountable to governance token holders who can vote to replace them.

**Foundation Replacement Mechanism:**
- Governance can **freeze** existing reserve assets (preventing new issuance).
- New reserve assets are minted and assigned to the replacement Foundation.
- Liquidation profits redirect to the new Foundation's on-chain address.
- Existing active UNIT in circulation is still valid and tracked by validators.

> *Note: Previously accumulated Treasury funds remain with the outgoing Foundation.*

### Fee Structure

The protocol has no built-in interest rate on UNIT debt (0%). However, participants are compensated through the following mechanisms:

| Participant | Compensation Source | Details |
|-------------|---------------------|---------|
| **Foundation** | Treasury profits | Takes compensation from accumulated liquidation proceeds |
| **Guardians** | Multiple sources | First-access to liquidations; optional direct client fees; optional cut of liquidation tax (governance-controlled) |
| **Oracles** | Treasury (via Foundation) | Compensated by Foundation for providing price contract services |
| **Validators** | None (public good) | Low resource requirements; incentive is trustless protocol visibility |

> *Note: Guardian fees and liquidation tax distribution are controlled by governance and defined in the anchor contract.*

---

## How Vaults Work

A **vault** is a Bitcoin transaction that locks collateral in a multi-signature contract. The vault records the debt balance, price contracts, and other metadata in an OP_RETURN output.

### Custody Model

Vaults use a **2-of-2 multi-signature** structure:

```
Spending requires: Client signature + Guardian signature
```

This ensures:
- The client cannot spend without guardian approval (protocol enforcement).
- The guardian cannot spend without client approval (user protection).

**Important**: There is no unilateral exit. Clients should include multiple guardian keys when opening a vault to ensure they can always find a co-signer.

### Vault Actions

| Action | Description |
|--------|-------------|
| **Open** | Create a new vault, deposit BTC, borrow UNIT |
| **Deposit** | Add more BTC collateral to an existing vault |
| **Borrow** | Issue additional UNIT against collateral |
| **Repay** | Burn UNIT to reduce outstanding debt |
| **Withdraw** | Remove BTC collateral (subject to ratio limits) |
| **Close** | Repay all debt, withdraw all collateral |

### Transaction Flow

Most vault actions follow this pattern:

1. **Create Estimate** - Calculate expected costs and fees
2. **Fetch Funding** - Collect UTXOs from the user's wallet
3. **Create Quote** - Generate final transaction parameters
4. **Fetch Price Contracts** - Obtain oracle attestations for liquidation
5. **Create PSBTs** - Build partially-signed Bitcoin transactions
6. **Sign PSBTs** - User signs with their wallet
7. **Submit Request** - Send to guardian for co-signing and broadcast

Actions that issue UNIT (open, borrow) require two linked transactions, while simple actions (deposit, withdraw) use a single transaction.

---

## Custody and Security Model

### Protocol Governance

All users must agree on the same **anchor contract**, which defines:
- Authorized signers (guardians and oracles).
- Asset definitions (UNIT parameters).
- Protocol terms (collateral ratios, fees, etc.).

The anchor contract can be amended through on-chain governance using the **DUCAT governance token**.

**DUCAT** is managed similarly to UNIT: it uses the Runes protocol, is minted up-front, and requires a governance-controlled issuance process. To participate in governance, holders stake their DUCAT in a time-locked transaction—this proves commitment and prevents vote manipulation.

The anchor contract defines the requirements for proposals (how much DUCAT must be staked, how long the lock period lasts). Validators tabulate votes and automatically update protocol rules when proposals pass.

Like UNIT, DUCAT reserves are distributed by the Foundation for issuance. The Foundation and DUCAT mint can themselves be replaced through governance, with validators ultimately controlling consensus.

> *Note: The specific mechanism for distributing DUCAT tokens to end-users for governance participation is currently undefined and will be specified in a future protocol update.*

### Asset Security

- The full supply of UNIT (and DUCAT) is minted up-front, starting in "reserve" (inactive) status.
- Only protocol-compliant issuance can activate reserve assets.
- Validators track active vs. reserve balances for all protocol assets.
- Invalid issuance attempts remain in reserve (economically worthless).

### Vault Security

- 2-of-2 custody prevents unilateral spending.
- Multiple guardian support provides redundancy.
- Price oracles are independent of guardians.
- Liquidation is automatic and transparent.

### Trust Assumptions

| Assumption | Mitigation |
|------------|------------|
| Guardian cooperation | Include multiple guardians |
| Oracle accuracy | Use multiple oracles |
| Validator consensus | Run your own validator |
| No unilateral exit | Careful guardian selection |
| Foundation stewardship | Governance can freeze reserves and elect replacement |

### Risks and Mitigations

**Liquidation Speed Risk:**

Liquidations process at the speed of the Bitcoin blockchain. This means speculators face more timing risk than on centralized exchanges, but no more than other participants relying on on-chain transactions. Users should maintain comfortable collateral ratios to buffer against rapid price movements.

**Guardian/Oracle Collusion Risk:**

A malicious guardian and oracle pair could potentially:
- Steal the bitcoin collateral from vaults they control.
- Debase the UNIT currency by stealing collateral from vaults.

However, they **cannot inflate the UNIT supply**—the supply is based on verified issuance.

**Collusion Mitigation:**

| Strategy | Effect |
|----------|--------|
| Use multiple guardians | Limits exposure to any single guardian |
| Use multiple oracles | Reduces reliance on any single price source |
| Balanced issuance | Protocol distributes UNIT issuance across unique guardian/oracle pairs |
| Compartmentalized risk | If one guardian/oracle pair becomes malicious, other vaults remain unaffected |
| automatic eviction | If validators detect a malicious guardian or oracle, they can eject them from the protocol immediately |
| Foundation as backstop | If collateral is stolen, foundation can provide capital to make vaults whole |

Ultimately, governance must exercise care when electing guardians and oracles into the protocol.

---

## Liquidation Mechanism

Vaults carrying UNIT debt are subject to **automatic liquidation** if their collateral ratio falls below the threshold.

### How Liquidation Works

1. **Price Contracts**: When a vault is created or updated, it includes price contracts from oracles. Each contract specifies a threshold price that would trigger liquidation.

2. **Breach Discovery**: Validators observe price attestations from new vault transactions (on-chain and in the mempool). These prices are used to check existing vaults for price contracts that have been breached. Vaults with breached contracts are marked as "liquid."

3. **Liquidation Keys**: Guardians continuously monitor the BTC/USD price and on-chain vaults, and forward breached price contracts to the price oracles. The oracle verifies each breach, then publishes the corresponding liquidation key.

4. **Recapitalization**: Liquidators subscribe and collect liquidation keys, then submit transactions to claim the liquid vaults. Liquidators must:
   - Claim both the debt and collateral tied to the vault.
   - Collect the protocol "liquidation tax" and send it to the treasury.
   - Deposit Bitcoin to raise the vault collateral to a healthy level.

5. **Withdraw Profits**: Liquidators aquire UNIT on the open market, then use it to repay the debt and withdraw bitcoin from the vault.

### Post-Liquidation

When a vault is liquidated, the original owner:
- **Loses**: Their Bitcoin collateral (transferred to the liquidator).
- **Keeps**: The UNIT they borrowed (their debt is cleared).
- **Retains**: The vault UTXO with a minimum deposit (currently 1,000 sats, defined by anchor contract).

The vault owner can continue using their vault for future operations after adding new collateral.

### Liquidation Economics

| Term | Description |
|------|-------------|
| **Liquidation Price** | BTC/USD price at which vault becomes liquid |
| **Vault Premium** | Collateral value above outstanding debt |
| **Liquidator Reward** | Portion of premium paid to the liquidator |
| **Liquidation Tax** | Portion of premium sent to the Treasury |

### Example: Liquidation (Continuing Alice's Story)

Recall Alice's vault from earlier:
- Deposited: **1 BTC**
- Borrowed: **50,000 UNIT**
- Liquidation Price: **$67,500**

**The price drops to $67,500.** Alice's vault is now eligible for liquidation.

**Premium Calculation:**
```
Collateral Value = 1 BTC × $67,500 = $67,500
Debt Value       = 50,000 UNIT     = $50,000
Premium          = $67,500 - $50,000 = $17,500
```

**Distribution (assuming 20% liquidation tax):**

| Recipient | Amount | Calculation |
|-----------|--------|-------------|
| Treasury | $3,500 | 20% × $17,500 |
| Liquidator Profit | $14,000 | 80% × $17,500 |

**What the liquidator does:**
1. Takes ownership of the vault debt and collateral.
2. Deposits Bitcoin to recapitalize the debt (bringing it back to healthy levels).
3. Performs a repay action by acquiring and burning UNIT, then withdraws collateral.
4. **Net profit: ~$14,000** (the premium minus liquidation tax)

**What happens to Alice:**

| Asset | Before | After |
|-------|--------|-------|
| BTC in vault | 1 BTC | 0 BTC (lost to liquidation) |
| UNIT held | 50,000 | 50,000 (keeps the borrowed UNIT) |
| Vault UTXO | - | 1,000 sats (minimum deposit retained) |
| Debt | 50,000 UNIT | 0 UNIT (cleared by liquidator) |

Alice lost her Bitcoin collateral but keeps the UNIT she borrowed. Her vault remains open with a minimal balance, ready for future use if she deposits more collateral.

**Market Stabilization Effect:**

This creates a market-based stabilization mechanism: when UNIT is under-pegged, liquidators profit by buying cheap UNIT to burn, driving the price back up.

### Foundation Backstop

In extreme market conditions, some vaults may become unprofitable for market liquidators:

| Condition | Description |
|-----------|-------------|
| **At/Below 1:1** | No premium exists - liquidators would lose money |
| **Stale Vaults** | Slightly above 1:1 but unliquidated past timeout threshold |

When these conditions occur, the **Foundation** steps in as the backstop:
- Purchases underwater vaults to prevent bad debt accumulation.
- Uses Treasury reserves to recapitalize positions.
- Absorbs temporary losses to defend the UNIT peg.
- Parameters (timeout thresholds, etc.) are defined in the protocol contract.

This backstop function is critical during rapid price declines where cascading liquidations could otherwise destabilize the protocol.

---

## For Businesses

### Integration

**Wallet Providers**
- Integrate vault management into existing Bitcoin wallets
- Offer UNIT as a stablecoin option for users
- Provide a seamless borrow-against-BTC experience

**Exchanges**
- List UNIT for trading
- Offer BTC/UNIT and USD/UNIT pairs
- Enable UNIT as a settlement currency

**Payment Processors**
- Accept UNIT for merchant payments
- Provide price-stable settlement options
- Reduce volatility exposure for merchants

### Node Operation

**Running a Guardian Node**
- Earn fees by co-signing vault transactions
- Requires capital for UNIT reserve accounts
- Must follow protocol rules to maintain validity

**Running a Validator Node**
- Index protocol state from the blockchain
- Serve data to clients
- Participate in consensus

**Running a Price Oracle**
- Provide price attestations
- Earn fees for price contract creation
- Independent operation from other participants

**Serving as Foundation**
- Elected by governance token holders
- Manage reserve UNIT distribution to guardians
- Operate the Treasury for market stabilization and backstop
- Compensation through Treasury allocation
- Requires trust and accountability to the protocol community

### Liquidation Participation

Liquidation is **open to the public** and can be a profitable activity:
- Monitor for breached vaults.
- Acquire UNIT from the market.
- Claim liquid vaults at a discount.
- Profit from the liquidation premium.

The protocol provides APIs for discovering liquid vaults and constructing liquidation transactions.

---

## Summary

The DUCAT Protocol enables **Bitcoin-backed borrowing** through a decentralized, self-custodial system. Key differentiators include:

- **Bitcoin-Native**: No sidechains, bridges, or wrapped tokens.
- **Self-Custodial**: Users maintain control through multi-sig.
- **Transparent**: All state verifiable on-chain.
- **Stable**: Automatic liquidation maintains the UNIT peg.
- **Resilient**: Foundation backstop protects against extreme market conditions.
- **Open**: Participation in guardianship, validation, liquidation, and regulation.

---

## Glossary

| Term | Definition |
|------|------------|
| **Anchor Contract** | A Bitcoin transaction that establishes the protocol's initial rules, authorized signers, and asset definitions. Identified by its transaction ID (txid). |
| **Client** | An end-user who opens vaults, deposits Bitcoin collateral, and borrows UNIT. |
| **Collateral Ratio** | The ratio of collateral value to debt value. Must stay above the liquidation threshold. |
| **DUCAT** | The governance token used to propose and vote on protocol changes. Staked in time-locked transactions. |
| **Guardian** | A protocol-approved entity that co-signs vault transactions, issues UNIT to clients from reserve allocations, and coordinates with oracles for price contracts. |
| **Hash-lock** | A cryptographic lock that requires a specific secret value (preimage) to unlock. Oracles use hash-locks to control liquidation spending paths—the lock can only be opened when the oracle reveals the corresponding key. |
| **Liquidation** | The process by which an under-collateralized vault is claimed by a liquidator who recapitalizes it. |
| **Liquidation Tax** | A portion of the liquidation premium sent to the Treasury. |
| **Liquidation Threshold** | The collateral ratio (default 135%) at which a vault becomes eligible for liquidation. |
| **Meta-Protocol** | A protocol layer built on top of Bitcoin, using standard Bitcoin transactions to record state. |
| **Oracle** | An independent party that observes BTC/USD prices and issues price contracts with liquidation keys. |
| **OP_RETURN** | A special Bitcoin transaction output that stores arbitrary data on the blockchain. DUCAT uses OP_RETURN outputs to record vault metadata such as debt balances and price contracts. |
| **Premium** | The collateral value above the 1:1 debt backing in a liquidated vault. Distributed between liquidator and Treasury. |
| **Price Contract** | A cryptographic commitment from an oracle specifying a price threshold and associated liquidation key. |
| **Rate Bucket** | A discrete collateral ratio level at which oracles pre-generate price contracts. |
| **Foundation** | The protocol-elected entity managing reserve distribution, Treasury operations, and backstop functions. |
| **Reserve (UNIT)** | UNIT that has been minted but not yet issued through a valid vault transaction. Economically inactive. |
| **Active (UNIT)** | UNIT that has been properly issued through a vault transaction. Recognized by validators as valid. |
| **Runes Protocol** | A token standard on Bitcoin that enables fungible tokens to be issued and transferred using standard Bitcoin transactions. DUCAT uses Runes for on-chain accounting of UNIT and DUCAT assets. |
| **Time-lock** | A transaction that locks funds until a specified block height or timestamp. Used in DUCAT governance to stake tokens when proposing or voting on protocol changes. |
| **Treasury** | The pool of reserve assets and accumulated liquidation proceeds managed by the Foundation. |
| **UNIT** | The USD-pegged stablecoin (1 UNIT = 1 USD) issued against Bitcoin collateral in vaults. |
| **Validator** | A node operator that indexes protocol transactions, tracks asset balances, and enforces protocol rules. |
| **Vault** | A Bitcoin transaction that locks collateral in a multi-signature contract, recording debt and price contracts on-chain. |

</details>

---

<p align="center">
  <sub>Built for Bitcoiners who refuse to compromise.</sub>
</p>

<p align="center">
  <a href="https://ducatprotocol.com">ducatprotocol.com</a>
</p>
