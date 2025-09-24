# 🟧 BitLend Protocol

**BitLend** is a decentralized lending and borrowing protocol on **Stacks**, enabling users to unlock liquidity from their Bitcoin-backed assets while maintaining long exposure to BTC via **STX-collateralized borrowing**.

It provides overcollateralized loans, automated liquidations, dynamic interest calculations, and permissioned protocol governance — all in a secure, MEV-resistant, and trust-minimized manner.

---

## 🧭 System Overview

BitLend bridges the gap between **Bitcoin capital holders** and **DeFi liquidity**, using **STX** as collateral to support borrowing directly within the Stacks ecosystem. The protocol ensures:

* **Capital Efficiency**: Borrowers maximize loan value through adjustable collateral ratios.
* **Risk Protection**: Lenders are safeguarded via liquidation thresholds and protocol-defined parameters.
* **Non-Custodial Design**: All collateral and debt positions are held on-chain, permissionlessly accessible and managed by smart contract logic.

---

## 🏗️ Contract Architecture

The protocol is implemented as a single smart contract written in [Clarity](https://docs.stacks.co/write-smart-contracts/clarity), designed for **security, transparency, and upgradability** of key parameters without altering core logic.

### ✅ Core Functionalities

| Function    | Description                                                              |
| ----------- | ------------------------------------------------------------------------ |
| `deposit`   | Users deposit STX to be used as collateral for loans                     |
| `borrow`    | Borrowers mint debt against their collateral within defined ratio limits |
| `repay`     | Repay borrowed STX to reduce outstanding debt                            |
| `withdraw`  | Withdraw collateral if it does not breach collateral requirements        |
| `liquidate` | Liquidate undercollateralized positions to protect protocol solvency     |

---

### 🔐 Data Models

#### User Position Map

```clojure
user-positions: {
  user: principal,
  total-collateral: uint,
  total-borrowed: uint,
  loan-count: uint,
}
```

#### Loan Map (Future extensibility)

```clojure
loans: {
  loan-id: uint,
  borrower: principal,
  collateral-amount: uint,
  borrowed-amount: uint,
  interest-rate: uint,
  start-height: uint,
  last-interest-update: uint,
  active: bool,
}
```

---

### ⚙️ Protocol Parameters

| Variable                   | Description                                     | Default |
| -------------------------- | ----------------------------------------------- | ------- |
| `minimum-collateral-ratio` | Minimum allowed ratio between collateral & debt | 150%    |
| `liquidation-threshold`    | Liquidation is triggered if ratio falls below   | 130%    |
| `protocol-fee`             | Fee applied on borrow/repay actions             | 1%      |
| `MAX-COLLATERAL-RATIO`     | Hard cap for governance changes                 | 500%    |
| `MIN-COLLATERAL-RATIO`     | Minimum safe ratio enforced                     | 110%    |

---

## 🔁 Governance & Risk Controls

The contract owner (initially `tx-sender` at deployment) can update key parameters through public governance functions:

* `set-minimum-collateral-ratio`
* `set-liquidation-threshold`
* `set-protocol-fee`

All governance functions include **validation guards** to ensure protocol safety and parameter consistency.

---

## 🔒 Security Considerations

BitLend is built with Clarity’s security model in mind and includes:

* **Reentrancy protection** via Clarity’s transaction model
* **Safe math** with Clarity's fixed-point arithmetic
* **Access Control** on privileged functions (owner-only)
* **Error Handling** using consistent error codes and assertions
* **Immutable Core Logic** to protect protocol invariants

---

## 📦 Installation

Clone the repository and use the [Clarinet](https://docs.stacks.co/clarity/clarinet) toolchain to build, test, and deploy the contract.

```bash
git clone https://github.com/your-org/bitlend-protocol
cd bitlend-protocol
clarinet check
clarinet test
```

---

## 🧪 Usage (Testing via Clarinet Console)

```lisp
;; Deposit 1000 STX
(contract-call? .bitlend deposit)

;; Borrow 500 STX
(contract-call? .bitlend borrow u500)

;; Repay 250 STX
(contract-call? .bitlend repay u250)

;; Withdraw 300 STX collateral
(contract-call? .bitlend withdraw u300)

;; Liquidate user (if under threshold)
(contract-call? .bitlend liquidate 'SPXXXX...')
```

---

## 📊 Read-only Functions

* `get-user-position (user principal)` — Returns collateral, borrowed amounts, and loan count.
* `get-protocol-stats` — Returns total deposits, borrows, and current protocol parameters.

---

## 📁 Future Extensions

* **Loan NFTs**: For transferability and secondary market trading
* **Dynamic Interest Rates**: Based on utilization ratios
* **Multi-collateral support**: Add other assets on Stacks (e.g., USDA, xBTC)
* **Decentralized Governance**: Transfer ownership to DAO contracts

---

## 📜 License

This protocol is open-source under the [MIT License](LICENSE).
