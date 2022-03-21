# Moat: Sustainable Liquidity Provisioining

Protocol treasuries hold significant amounts of their native token ($XYZ) and want to provide liquidity for it so that their community can easily trade in and out of it. They do not want to be exposed to variable risks like impermanent loss and are willing to accept a lower, even negative return in exchange.&#x20;

Liquidity providers seek yield on their stablecoins ($USDC) and are willing to accept higher, variable risk in exchange for higher return.&#x20;

The Castle Moat resolves these incentives as follows:

![Visualization of capital flows orchestrated by Castle Moat](https://lh4.googleusercontent.com/azjXxzZr1r02ujcS68WYnFBjaM40jeUWVUsb8oJADN7Bhk9vh6mfpEfUnj8ghQKP0o-cbiVsoPx0P2xH5twXk4Mur89jlNwuhEQdLy7LrLk4XrD7g-v0LcN2ClCJc03kVFyfutke)

![Liquidity curve set by LPs, x set by protocol,
shaded bars indicate capital that is deposited into the AMM pool](https://lh3.googleusercontent.com/R2noih\_kyCYO3wPVWRfImADzHUzC1-fapjodb33y4uU\_Jt1Jf-z4ZudMhq3XJHLJ3NW4g\_YEfqdNQIjTPbf\_7owI0C1k6Wjsdg1gPOrM-eEayIr1Re0Baz9iozsZhIPcHTVr1SdZ)

The reward tranche hurdle rate, x, is the fixed percentage of market-making rewards paid to the protocol treasury, denominated in its native token, XYZ.&#x20;

Liquidity providers deposit USDC into a pool while specifying a maximum hurdle rate, x\_max. LPs receive the rest of the rewards minus any impermanent loss. LPs are able to deposit to either specific token pools or a global pool that allocates automatically.&#x20;

Based on this liquidity curve, the protocol selects x according to how much liquidity they desire.

Every period, t, rewards are paid out as follows:&#x20;

Senior rewards: r\_s = n \* x&#x20;

Junior rewards: r\_j = r\_total - r\_s

The Castle Moat will start by using a CFAMM like Orca or Raydium, but will eventually implement a modified Avellaneda-Stoikov market-making algorithm tuned to maintain inventories associated with the desired reward payout split of both XYZ and USDC.

### FAQ&#x20;

**What are the benefits for LPs?**&#x20;

* They can be adequately compensated for taking on risk of impermanent loss since they can earn a yield higher than LPing independently.&#x20;
* They only need to provide one asset. (If in the managed global pool)&#x20;
* They obtain passive exposure to a large number of AMM pools.&#x20;

**What are the benefits for protocols?**&#x20;

* They provide liquidity for their community to easily swap in and out of their native token, preventing liquidity-driven sell-offs, so-called “death spirals”.&#x20;
* They earn a fixed yield on native tokens in the treasury.&#x20;
* There is no need to account for variable rates or impermanent loss.&#x20;

**How are the AMM pool rewards paid out?**&#x20;

A period, t, is specified for each AMM pool. Funds are locked in the pool for the duration of each period. Users are able to mark funds for deposit or withdrawal at the end of the current period. At that time, junior and senior reward splits are calculated according to the formulas above. Users can specify whether they want to have rewards automatically re-deposited in the next period or if they should be made claimable. Claimable rewards are able to be claimed at any time.&#x20;

**How do LPs earn more in Castle Moat than they would normally?**&#x20;

Lets say yield of XYZ:USDC pool is $100 per $1000 invested (before IL) and x=1% ($10):&#x20;

LP provides $500 USDC and would get $90 of the rewards, which is 80% higher than they would get if they provided both sides. In addition, they don’t have to take on the price risk of XYZ. In exchange, they take on IL risk and can bid for different x values according to how high they think that risk will be. It is even possible that x can be negative and the protocol pays x% to LPs.&#x20;

**How is the hurdle rate, x, chosen?**&#x20;

The hurdle rate is selected by the XYZ protocol according to how much liquidity they require. The more liquidity, the lower x will be set.&#x20;

In the future, non-protocol LPs will be able to deposit to the XYZ pool in a similar fashion to USDC LPs with the only difference being that they set a minimum hurdle rate x\_min, instead of a maximum one.&#x20;

**How is this different from Tokemak?**&#x20;

Tokemak seeks to solve the same problem, but suffers from the following issues:&#x20;

* LPs and treasuries earn yield in TOKE, not the asset they desire Treasuries still earn unpredictable, variable yield&#x20;
* Tokemak requires active participation from both LPs and LDs (TOKE stakers)&#x20;
* Impermanent loss is passed onto TOKE holders, disincentivizing the necessary, aforementioned, active participation&#x20;
* Collusion exploits are introduced by letting non-LP actors direct liquidity. One adverse outcome is that new assets and pool are not able to be permissionlessly added&#x20;

Castle Moat addresses all these issues in addition to being conceptually simpler, reducing the attack surface for both game-theoretical and smart-contract exploits.
