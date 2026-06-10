# Overview

The Reserve Yield Protocol allows anyone to create diversified baskets backed by yield-bearing ERC-20 tokens on Ethereum, Base, and Arbitrum. The protocol enables permissionless launch, onchain governance, and autonomous operations for yield-bearing Decentralized Token Funds (DTFs):

* **Permissionless creation & NAV-based issuance.** Anyone can deploy a Yield DTF and let users mint or redeem at real-time asset value, with no reliance on offchain custodians.
* **Built-in yield capture & routing.** Collateral earns staking, lending, or other rewards; smart contracts harvest and distribute them automatically based on governance-defined routing.
* **RSR-backed overcollateralization.** Staked Reserve Rights (RSR) can create a first-loss safety buffer, insulating the basket from collateral shortfalls and aligning incentives through shared fees.
* **Transparent, token-holder governance.** Parameters like fee rates, collateral mixes, and risk limits are managed onchain, keeping control in the community’s hands.

Together, these capabilities enable anyone to launch a fully collateralized, yield-accruing basket with robust decentralized infrastructure.

![](<../../.gitbook/assets/simple representation of a yield dtf.svg>)

Once a Yield DTF configuration has been deployed, the DTF's tokens can be minted by depositing the entire basket of collateral backing tokens, and redeemed for the entire basket as well. Thus, a DTF will tend to trade at the market value of the entire basket that backs it, as any lower or higher price could be arbitraged.

### Overcollateralization

Yield DTFs can be overcollateralized, which means that if any of their collateral tokens defaults, there's a pool of value available to make up for the loss. Overcollateralization is provided by RSR holders, who may choose to stake their RSR on any Yield DTF. Staked RSR can be seized in the case of a collateral default, in a process that is entirely mechanistic based on onchain price-feeds, and does not depend on any governance votes or human choices.

Yield DTFs can generate revenue, and this revenue is the incentive for RSR holders to stake. Revenue can come from yield from lending collateral tokens onchain, revenue shares with collateral token issuers, or any other source of onchain yield. Governance can direct any portion of revenue to RSR stakers, to incentivize RSR holders to stake and provide overcollateralization. If a Yield DTF generates no revenue, or if none of it is directed to RSR stakers, there is no incentive to stake RSR on it, so it is unlikely to be protected by overcollateralization.

For the kinds of DTFs the core team is imagining, default will be an extremely rare "black‑swan" situation, not something that occurs regularly. But still, it could happen, and the purpose of the overcollateralization mechanism is to preserve Yield DTF holders’ value if it does.

### Revenue handling

Yield‑bearing collateral—such as interest‑bearing tokens or LP positions—accrues value over time. The protocol’s Backing Manager periodically harvests this surplus and converts it into liquid assets.

Governance determines how the harvested revenue is divided among three outlets: DTF holders, RSR stakers, and any additional addresses specified by governance. Once the accumulated surplus crosses a configurable threshold, the Backing Manager initiates an onchain auction to exchange the excess collateral for more shares of the DTF and/or RSR.

Newly acquired DTF shares are burned, increasing the token’s redeemable value. Proceeds routed to stakers are swapped for RSR and deposited into the stRSR contract, raising the stRSR/RSR exchange rate. Because the entire cycle is autonomous, revenue compounds transparently without manual intervention.

### Governance

Each Yield DTF is governed separately, and each can have an entirely different governance system. Initial Yield DTFs will likely have relatively simple governance systems, which will probably evolve over time. That evolution is up to those that hold power in the initial governance systems.

Governance defines not just the basket to back a Yield DTF, but also an ordered list of emergency collateral. So there's a current basket, and a series of assets that can be deployed to the basket in the case of a default. A collateral token is considered to have defaulted if it's gone down in value more than a defined amount for a long enough time, relative to its reference or target unit (described in the Monetary Units section of the [collateral documentation](https://github.com/reserve-protocol/protocol/blob/master/docs/collateral.md)).

Governance can update the basket configuration regularly. When the current basket is updated, or in the case of a default, the protocol makes onchain trades to reach the new basket composition. This trading is modularized so that it can happen in different ways over time as onchain liquidity evolves. Currently, trading plugins support Gnosis Auctions (batch auctions) and Dutch auctions, but in the future additional methods can be incorporated.

{% hint style="info" %}
As with any smart contract application, the actual behavior may vary from the intended behavior, and it's safest to wait for an application to be in use for a long period of time before trusting it to behave as expected. This overview and subsequent documentation describe the intended behavior.
{% endhint %}
