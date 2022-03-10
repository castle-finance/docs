# Vault: DeFi Savings Account

### Goal

An investment product for USD stablecoins that safely earns yield across the Solana ecosystem, similar to a money market fund in traditional finance. Reducing risk of principal loss is prioritized over optimizing returns.&#x20;

When stablecoins are deposited into the Castle Vault (CV), they are forwarded to a diversified set of safe yield-earning sources. Periodically, the contract will rebalance between sources to optimize the yield earned.&#x20;

### Yield sources

Stablecoin lending: Jet, Port, Solend&#x20;

LPing stableswap AMM pools: Saber

Margin lending: Mango

### Diagram

![](https://lh4.googleusercontent.com/Y7U1dfMUS2flo40Tu7JI\_ODmgWmpJJkG66KBfJ\_6q9PF8bDEewzfG4ZSDowZryw3mtoNwrxPiZQDartAv38GtKMtGkv0aqOkjsxlJZHdfHxzIY3YIChJOoXi9iw\_P8GSe\_4Wjs-K)

### Risk Management

* The CV will only allocate to protocols which (1) are open-source, (2) have verifiable builds, (3) decentralized deployment keys, and (4) have been audited by a reputable security firm.&#x20;
* The CV will never have more than x% of exposure to any single underlying protocol.&#x20;
* Castle will initially cap the amount of funds deposited into the CV. This will be raised and eventually removed as more confidence is gained in the implementation.&#x20;
* The CV will monitor the price of stablecoins it holds. If it detects unusual behavior, it will rebalance out of the stablecoin in question.&#x20;
* When swapping between tokens in the process of rebalancing or harvesting, the CV will never incur more than y% of slippage.&#x20;
* The CV will ensure that there is liquidity at all times by never keeping lending funds to a reserve near a 100% utilization rate.&#x20;
* The Castle team will be able to “emergency brake”, which immediately withdraws all funds from underlying protocols into vault reserves. This will only be used in extraordinary circumstances such as Solana network congestion.

### Future Work

* Continue integrating with yield sources to increase strategy capacity as well as further diversify and reduce risks&#x20;
* Add multi-asset support to further optimize yield earned&#x20;
  * USD Vault can switch between multiple stablecoins&#x20;
  * SOL Vault can switch between multiple staking derivatives&#x20;
* Use an insurance provider to protect against stablecoin de-pegging and smart contract exploits of underlying protocols
* Use (limited) leverage if there is an arbitrage opportunity between the lending and borrowing rates of two stable pairs&#x20;
* Go cross-chain with LayerZero or Wormhole
  * Send funds to yield opportunities on other blockchains&#x20;
  * Let investors deposit funds from any blockchain
