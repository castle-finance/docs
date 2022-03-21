# Keep: Custom Diversified Staking

Staking offers an attractive return on investment for anyone who holds SOL, while also contributing to greater network security. The risk is minimized due to the non-custodial system design. Today validators are mostly fungible, however this will change with the addition of slashing rules and MEV opportunities.

Slashing is the process by which validators acting against the interest of the network are punished. Once introduced, a validator can lose assets if they contravene, whether intentionally or not. Anyone looking to stake their SOL should diversify across multiple validators to reduce this risk and their chances of principal loss.&#x20;

MEV is ”the maximum value that can be extracted from block production in excess of the standard block reward and gas fees by including, excluding, and changing the order of transactions in a block.” The infrastructure around MEV on Solana is still nascent, but actively being built out. Once it is widespread, validators will be able to receive payments from searchers, boosting the rewards of their staking. Validators will likely approach this in different ways, particularly regarding MEV that is morally-ambiguous.&#x20;

In addition to these two factors, there are numerous other criteria one may choose to consider when selecting validators. One may want to optimize for network decentralization by staking with validators who are geographically distanced or who currently have lesser amounts of SOL. Or one may want to arbitrarily support only those validators who are in a particular region, have supported particular runtime features, or use only green energy for their operations.&#x20;

Currently, it is prohibitively difficult to express these preferences. One would need to collate data about the various validators, run their own optimization to determine weightings, and manually delegate staking authority to each one. Because the validator ecosystem changes frequently, this would need to be repeated every rebalance period.&#x20;

We believe the ability for anyone to set their staking preferences and easily have them expressed via fund flows is a necessary part of any healthy proof-of-stake ecosystem. Towards that end goal, we introduce the Castle Keep, a permissionless, generalized stake bot for Solana.&#x20;

The Castle Keep allows anyone to construct an arbitrary staking strategy and have it be executed on their behalf. Users can filter and sort validators based on any publicly available criteria. As more funds are directed by the Keep, validators seeking stake allocations will disclose more information to differentiate themselves, increasing transparency and driving change towards community preferences.&#x20;

Strategies can be constructed at the individual validator level or by generalized factors and characteristics. The Castle Keep will periodically rebalance funds to ensure that a user’s funds are always staked according to their desired strategy.&#x20;

Users will access the Keep through an easy-to-use web interface where they will be able to create new strategies and monitor the performance of existing ones. They will be able to view current validators that funds are staked with, changes made in previous rebalance events, and overall performance of their pool. In addition, reporting tools will be provided for both individuals seeking statements for tax purposes and for DAOs seeking analytics to report back to their communities on treasury performance.

![](https://lh6.googleusercontent.com/Nhoq8x3\_LdCu461n7PWf56uuLQ2WUwPIflHIPs0\_bZ0HJ275di-PsTMBywPRSKM9Q3zXarH5pNw2uW9zE4AKokcFsGqPP7iyL-Q9I8L7UZjzA8xvOYtO33lNtCIEwjZgI8EaK5pb)

### FAQ&#x20;

**How is this different from a stake pool like Socean, Lido, or Marinade?**&#x20;

Stake pools provide the diversification that is preferred by many SOL holders, however it is not possible for an individual user to express their preference of validators. A given pool executes a single staking strategy on behalf of the entire pool. In exchange, stake pools are able to offer a common liquid staking derivative that is able to be used across the DeFi ecosystem. This would be difficult for the Castle Keep to offer because of the fragmentation across various strategies.&#x20;

**What are the risks involved with staking via the Castle Keep?**&#x20;

Because of the design of the SPL staking program, the user maintains control over their funds at all times. They are able to undelegate staking authority and withdraw funds. Castle Keep is able to utilize this program, introducing no additional smart contract risk. Staking in the Castle Keep is just as safe as staking manually via Solflare or Phantom.
