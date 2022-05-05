# Program

Program Code Repository: [https://github.com/castle-finance/castle-vault](https://github.com/castle-finance/castle-vault)

Functions in the program are created by bundling one or more instructions into a transaction. This document will first explain what each instruction does on its own and then how they are bundled together into various user-facing functions in the SDK.

### Instructions

#### Initialize

Creates a new vault

#### Deposit

Exchanges reserve tokens for vault LP tokens

#### Withdraw

Exchanges vault LP tokens for reserve tokens

#### Rebalance

Calculates and stores the desired allocations to integrated lending markets based on the vault strategy type

Also emits and anchor event containing the new allocations

Required before any reconcile IX is called

#### Refresh

Refreshes the reserves of all integrated lending markets, which is required before any deposit or withdrawal from them

Calculates the total value of the vault, denominated in the reserve token. This is required before any deposit or withdrawal from the vault

#### Reconcile \[Jet | Port | Solend]

Deposits and withdraws from the given lending market according to the allocations stored by the rebalance IX

### Functions

#### Initialize

IXs

* Initialize

#### Deposit

IXs

* (if reserve token is SOL) Wrap SOL into SPL token
* (if doesn't exist already) Create account for LP token
* Refresh
* Deposit
* (if reserve token is SOL) Close intermediate account created during wrapping

#### Withdraw

IXs

* Refresh
* (if withdraw amount is less than in vault reserves) Reconcile&#x20;
* (if doesn't exist already) Create account for reserve token
* Withdraw
* (if reserve token is SOL) Unwrap from SPL token to SOL

#### Rebalance

IXs

* Refresh
* Rebalance
* Reconcile IXs
