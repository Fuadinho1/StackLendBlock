
# StackLendBlock - Lending Pool Smart Contract

## Overview

This Clarity smart contract implements a **decentralized lending pool** where users can **deposit, withdraw, borrow, and repay STX tokens**. The system enforces collateralization requirements to ensure solvency and includes mechanisms for liquidation and interest accrual.

The contract is designed to support:

* **Deposits** with event logging
* **Collateral-backed borrowing**
* **Repayment of loans**
* **Withdrawals restricted by collateral ratios**
* **Liquidation logic for undercollateralized positions**
* **Interest accrual** (placeholder structure for extensibility)

---

## Features

* **Collateralized Borrowing**

  * Borrowers must maintain at least a **150% collateralization ratio**.
  * Accounts falling below **120% collateralization** are eligible for liquidation.

* **Deposit & Withdraw**

  * Users can deposit STX to earn collateral power.
  * Withdrawals are only allowed if the user maintains the required collateral ratio.

* **Borrow & Repay**

  * Borrowers can take out loans backed by their deposits.
  * Repayments reduce outstanding debt and free up collateral.

* **Liquidation**

  * Underwater positions (< 120% collateral) can be liquidated by whitelisted liquidators.
  * Liquidators earn a **5% liquidation fee**.

* **Governance & Safety**

  * Contract owner can pause the protocol in emergencies.
  * Interest rates and ratios are configurable.

---

## Constants

| Name                    | Default | Description                            |
| ----------------------- | ------- | -------------------------------------- |
| `min-collateral-ratio`  | `u150`  | Minimum collateral ratio (150%)        |
| `liquidation-threshold` | `u120`  | Threshold ratio for liquidation (120%) |
| `interest-rate`         | `u5`    | Annual interest rate (5%)              |
| `liquidation-fee`       | `u5`    | Liquidator fee (5%)                    |

---

## Errors

| Code   | Meaning                 |
| ------ | ----------------------- |
| `u100` | Unauthorized            |
| `u101` | Insufficient balance    |
| `u102` | Insufficient collateral |
| `u103` | Invalid amount          |
| `u104` | Contract paused         |
| `u105` | Invalid ratio           |
| `u106` | Invalid rate            |
| `u107` | Liquidation failed      |

---

## Functions

### Public Functions

* `deposit (amount uint)` → deposit STX into the pool.
* `withdraw (amount uint)` → withdraw deposits if collateral ratio remains safe.
* `borrow (amount uint)` → borrow STX against deposits.
* `repay (amount uint)` → repay outstanding loans.

### Read-Only Functions

* `get-deposit (user principal)` → get user’s deposit balance.
* `get-borrow (user principal)` → get user’s borrow balance.
* `check-collateral-ratio (user principal collateral uint debt uint)` → validate ratio.
* `calculate-collateral-ratio (collateral uint debt uint)` → calculate ratio.
* `get-total-deposits` → total pool deposits.
* `get-total-borrows` → total outstanding loans.
* `is-underwater (user principal)` → check if a user is under liquidation threshold.

### Events

* Deposit, Withdraw, Borrow, Repay, Liquidation, Interest Accrual

---

## Example Usage

```clarity
;; User deposits 100 STX
(contract-call? .lending-pool deposit u100)

;; User borrows 50 STX
(contract-call? .lending-pool borrow u50)

;; User repays 20 STX
(contract-call? .lending-pool repay u20)

;; User checks if they are underwater
(contract-call? .lending-pool is-underwater tx-sender)
```

---

## Security Considerations

* Withdrawals and borrows are strictly collateral-checked.
* Pausable contract to handle emergencies.
* Interest accrual is currently basic but can be extended.
* Liquidations require whitelisted liquidators.

---
