---
description: Vault provides the safest yield for risk-conscious DAOs and their communities.
---

# Vault: Safe Stablecoin Yield

### What is the Vault?

The Castle Vault is an investment product for USD stablecoins that safely earns yield across the Solana ecosystem, similar to a money market fund in traditional finance. Minimizing risk of principal loss is prioritized over maximizing returns. DAOs utilize Vault for various goals, from funding operating expenditures to growing corporate savings accounts and protocol insurance funds.&#x20;

When stablecoins are deposited into Vault, they are forwarded to a diversified set of yield-earning sources. Every minute, the contract rebalances between sources to optimize the yield earned. Vault is built on top of the most reputable lending primitives in the Solana ecosystem.&#x20;

We source yield from the following protocols:&#x20;

* Solend&#x20;
* Port Finance&#x20;
* Jet Protocol&#x20;

Additional integrations with Mango, Saber, Mercurial, and others, are under development.

<img src="https://lh5.googleusercontent.com/YklczSrnTOxmPOEO5h00OTbPhDn0KbHgYXi-l3295_xKyDPqrWZctK2ZWudJxlmP3voObHaIL7xH2kiydV7zFZezbzpGkPP-PjbiwD8-b_V5UyKVJDURh71ijae_g2QJ_PfHZZVF" alt="" data-size="original">

### Risk Management

In order to minimize the likelihood of principal loss, Vault implements the following risk management measures:

* **Overexposure Control:** Vault limits exposure to any single underlying protocol.
* **Underlying Liquidity:** Vault ensures that there is liquidity at all times by never lending funds to money markets near a 100% utilization rate.
* **Protocol Security:** Underlying protocols are open-source, audited, have verifiable builds, and have decentralized deployment keys.
* **Audit:** Bramah Systems, an expert in distributed ledger security, has conducted a full audit of our protocol’s codebase and found no unresolved issues. As we develop additional features, future builds will be reviewed in the same manner.
* **Emergency Brake:** Castle’s team will be able to “emergency brake”, which immediately withdraws all funds from underlying protocols into Vault’s reserves. This will only be used in extraordinary circumstances such as Solana network congestion.

### Roadmap

* **Additional Yield Sources:** Continue integrating with DeFi protocols to increase capacity and further diversify risks.
* **Insurance:** Protection against stablecoin de-pegging and smart contract exploits of underlying protocols.
* **Unusual Behavior Detection:** Monitor the price of stablecoins and rebalance out if unusual behavior is detected.
* **Yield Enhancement:** Execute arbitrage opportunities between the lending and borrowing rates of two stable pairs.
* **Cross-chain:** Expand reach to other blockchains, allowing for a wider universe of deposit assets and yield opportunities.

****