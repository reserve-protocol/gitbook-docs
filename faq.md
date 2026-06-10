# FAQ

## Overview

<details>

<summary><strong>What is Reserve’s mission?</strong></summary>

The Reserve project’s mission is to make it possible for anyone to own and earn their share of the world’s wealth. It does this by enabling people to hold and transfer diversified portfolios of tokenized assets as a single unit, laying the foundation for asset-backed currency backed by real assets rather than inflationary fiat. Over time, earning and holding value this way allows people to build direct ownership of a share of the world’s wealth.

</details>

<details>

<summary><strong>What are Decentralized Token Funds (DTFs)?</strong></summary>

DTFs allow anyone to own a share of everything. They are fully asset‑backed ERC‑20 tokens created with Reserve’s open‑source contracts. A DTF can represent anything from an automatically rebalanced yield basket to a broad crypto market index. Anyone can launch, mint, redeem, and govern a DTF permissionlessly onchain.

</details>

<details>

<summary><strong>What’s the difference between Yield DTFs and Index DTFs?</strong></summary>

[Yield DTFs](core-components/yield-dtfs/) diversify across yield-generating strategies — like lending or staking — that employ a particular asset, such as a particular stablecoin or liquid staking token. Yield DTFs can be protected against default by Reserve Rights (RSR) stakers, who earn a portion of DTF yield in exchange for governance and overcollateralization.

[Index DTFs](core-components/index-dtfs/) focus on efficiently managing diversified portfolios of tens to hundreds of tokens. Their lightweight design eliminates the need for complex collateral management, enabling the creation of large, transparent indexes with broad exposure. Instead of yield-based revenue, index DTFs charge minting and TVL (management) fees.

</details>

<details>

<summary><strong>What are RTokens?</strong></summary>

RTokens are the technical name for all tokens launched on Reserve — whether they’re Yield DTFs or Index DTFs. While these docs use “DTF” for clarity & consistency, you’ll still see “RToken” in the app, videos, and community. Both terms are valid and refer to the same underlying contract standards. Yield DTFs and Index DTFs launched on Reserve can also be called "Yield RTokens" and "Index RTokens."

</details>

***

## Reserve Rights (RSR)

<details>

<summary><strong>What is the RSR token?</strong></summary>

[Reserve Rights (RSR)](core-components/rsr-reserve-rights.md) is the token that allows anyone to own a share in the value of everything. It is an ERC-20 token that unifies governance, risk management, and value accrual across the Reserve ecosystem. RSR has three main roles:

* **Staking on Yield DTFs**: RSR stakers provide governance and first-loss capital (overcollateralization) in exchange for DTF yield.
* **Vote-locking on Index DTFs**: RSR is the default governance token for Index DTFs, controlling basket changes, parameters, and upgrades—and sharing in fees when enabled.
* **Deflationary sink**: A portion of every Index DTF’s mint and TVL fees is used to market-buy RSR and burn it, steadily reducing the circulating supply.

</details>

<details>

<summary><strong>Where can I stake or vote‑lock RSR?</strong></summary>

You can stake or vote-lock RSR by accessing the [Reserve app](https://app.reserve.org/) and selecting a DTF.

For Yield DTFs, RSR staking is necessary in order to participate in governance and earn rewards. Stakers also provide first-loss capital in the event a collateral asset fails in the DTF or a black swan event occurs — such as what happened during USDC’s depeg. [Learn more about staking](https://blog.reserve.org/how-to-stake-reserve-rights-rsr-f5393fe24573).

For Index DTFs, creators can choose any governance token, although RSR is the default. Vote-lockers govern basket changes, parameters, and upgrades—and share in fees when enabled. [Learn more about vote-locking](https://blog.reserve.org/vote-locking-on-reserve-19e19201d78e).

</details>

<details>

<summary><strong>What are the tokenomics of RSR?</strong></summary>

RSR has a fixed max supply of **100 billion** tokens, of which around **60%** are currently in circulation. All remaining RSR emissions follow a deterministic schedule which emulates the emissions curve of Bitcoin.

See [CoinMarketCap's Token Unlock page](https://coinmarketcap.com/currencies/reserve-rights/#token_unlocks) for precise values and timeline, or [this blog post](https://blog.reserve.org/reducing-rsr-emissions-6da7f35917ba) for an overview of the emissions curve and its design motivations.

</details>

<details>

<summary><strong>How can I estimate my RSR staking or vote‑lock returns?</strong></summary>

Current staking APYs (Yield DTFs) and governance reward shares (Index DTFs, if configured) are displayed in the Reserve app next to each DTF's Stake/Lock buttons. These values are estimated based on recent performance and may not be predictive of future performance. Once you have already staked or locked, you will notice rewards accrual in the app's wallet UI.

</details>

<details>

<summary><strong>I have old RSR that I can no longer transfer from my wallet. How can I exchange these for the new RSR tokens?</strong></summary>

Good news: your new RSR is already in your wallet. The bad news is that some wallets do not auto-discover the new RSR, so you have to take a few extra steps. Read [How to Upgrade to the New Reserve Rights (RSR) Contract](https://blog.reserve.org/how-to-upgrade-to-the-new-rsr-contract-7eb338e4261a) for a comprehensive breakdown of your options.

</details>

***

## Reserve app

<details>

<summary>What is the Reserve app?</summary>

The [Reserve app](https://app.reserve.org/) (sometimes called "Reserve Register") is a decentralized app (“dapp”) frontend enabling easy access to the Reserve platform. The app allows anyone to create, mint, or redeem DTFs in a permissionless manner. The Reserve app also allows RSR (or other governance token) holders to stake or vote-lock their tokens onto their preferred DTFs to participate in governance, earn yield, and provide first-loss capital in the event of a depeg event.

</details>

<details>

<summary>How do I bridge DTFs or RSR between Ethereum, Base, and Arbitrum?</summary>

In the Reserve app UI, click **More** and use the built‑in **Bridge** flow. Select the chain, token, and deposit or withdraw. The same interface supports DTFs, RSR, and other common tokens.

</details>

<details>

<summary>Where can I find Reserve ecosystem contract addresses?</summary>

The contract addresses for all DTFs are available in the Reserve app. Navigate to the DTF page you want, then find the contract address(es) featured in the header alongside the DTF’s name.

For more details, see the Smart Contracts sections of the [Yield DTF](https://github.com/reserve-protocol/protocol/tree/master/docs) and [Index DTF](core-components/index-dtfs/smart-contracts.md) docs.

</details>

<details>

<summary>Who can create a DTF?</summary>

Because the Reserve platform is permissionless and no coding experience is needed, anyone can create a DTF. DeFi developers, entrepreneurs, crypto protocols, apps, hedge funds, TradFi, and even rewards programs and video game/metaverse developers are all potential DTF deployers.

</details>

<details>

<summary>Which DTFs are available to mint/redeem? How?</summary>

All DTFs can be minted and redeemed permissionlessly. DTFs are available to explore, mint, stake and govern via the [Reserve app](https://app.reserve.org/).

For detailed walkthroughs, see the minting and redemption docs for [Yield DTFs](core-components/yield-dtfs/minting-and-redeeming.md) and [Index DTFs](core-components/index-dtfs/minting-and-redeeming.md).

</details>

<details>

<summary>I want to deploy my own DTF. Where do I start?</summary>

**Yield DTFs:** Start by reviewing the [Official Documentation](core-components/yield-dtfs/README.md), the [Deployment Guide](core-components/yield-dtfs/deployment-guide/), and the [YouTube Tutorial](https://www.youtube.com/watch?v=sf0PuYpVWRU&ab_channel=Reserve), where you can find everything related to the process of deploying your DTF. Then you can visit the [Reserve app](https://app.reserve.org/deploy/yield-dtf) to deploy.

**Index DTFs:** The Index DTF deployment UI is still under construction. If you want to be first in line to create an Index DTF, put down your contact information [here](https://app.reserve.org/deploy/index-dtf).

</details>

<details>

<summary>How do I list a DTF in the Reserve app UI?</summary>

For your Yield DTF to be listed in the Reserve app, you can create a pull request in this [GitHub repository](https://github.com/reserve-protocol/rtokens).

Index DTF listing coming soon.

</details>

***

## Reserve platform operations

<details>

<summary>What blockchains does Reserve support?</summary>

The Reserve Yield Protocol is currently deployed on Ethereum, Base, and Arbitrum.

The Reserve Index Protocol is currently deployed on Ethereum and Base.

DTFs (and RSR) can be bridged to many popular blockchains.

</details>

<details>

<summary>Has the protocol been audited?</summary>

Yes. All audits can be viewed on the [Yield DTF](core-components/yield-dtfs/security.md) and [Index DTF](core-components/index-dtfs/security.md) Security & Audits pages.

</details>

<details>

<summary>Are Reserve's smart contracts decentralized?</summary>

Yes. The core contracts are only upgradable via governance proposals that get approved by onchain governance. These proposals can either change a single parameter or upgrade a contract.

</details>

<details>

<summary>How are DTFs decentralized?</summary>

Collateral baskets are tokenized onchain, with the smart contract risk diversified over multiple protocols and assets. Each DTF is governed separately by stakers or vote-lockers and each can have an entirely different governance system.

</details>

<details>

<summary>What are the risks of using the Reserve platform and DTFs?</summary>

Smart contracts, depegs, counterparty and governance risks are all applicable to the Reserve platform. Read our [Risks documentation](risks.md) or [Comprehensive Risk Mitigation at Reserve Protocol](https://blog.reserve.org/comprehensive-risk-mitigation-at-reserve-protocol-68a724e9989e) to dive deeper.

</details>

<details>

<summary>What are collateral plugins and why are they important?</summary>

In the Reserve Yield Protocol, collateral plugins wrap ERC-20 tokens into assets that can back Yield DTFs. Without them, native ERC-20 tokens cannot be used to collateralize Yield DTFs. [Learn more about collateral plugins](https://github.com/reserve-protocol/protocol/blob/master/docs/collateral.md).

The Reserve Index Protocol does not require collateral plugins, and natively supports most ERC-20s.

</details>

<details>

<summary>What assets can be used as collateral in DTFs?</summary>

For **Yield DTFs**, any asset with a suitable collateral plugin can be used as a collateral asset within a Yield DTF. Collateral plugins help to price underlying assets and surface properties required for the DTF to ascertain its status. [Learn more about developing collateral plugins](https://github.com/reserve-protocol/protocol/blob/master/docs/collateral.md).

Discover the [currently available collateral options](https://app.reserve.org/explorer/collaterals) in the Reserve app. New collateral assets are constantly being integrated into the protocol, and ambitious deployers can even [create custom collateral plugins](core-components/yield-dtfs/deployment-guide/yield-dtf-deployment-walkthrough.md#appendix).

For **Index DTFs**, nearly any ERC-20 token can be used, no collateral plugin required.

</details>

<details>

<summary>Are DTFs algorithmically backed?</summary>

No, DTFs are not algorithmically backed. DTFs are fully asset-backed 1:1 with exogenous collateral (i.e., external, unrelated assets) that, via smart contracts, are able to be redeemed at any time for the underlying assets.

</details>

<details>

<summary>How can a DTF deployer earn revenue?</summary>

Revenue distribution for DTFs is entirely flexible. From the revenue that is being accrued, any portion can be sent to any number of arbitrary Ethereum addresses, including the DTF deployer. Revenue share percentages are set when deploying the DTF and can only be changed by community governance.

</details>

<details>

<summary>Where does DTF revenue come from?</summary>

**Yield DTFs:** Deposits in DeFi protocols such as Aave, Compound, Uniswap and Convex provide the depositor a receipt token that accrues yield. When these receipt tokens are used in collateral baskets for Yield DTFs, the protocol’s onchain operations harvest this yield to distribute to DTF stakeholders. This is performed 100% onchain. Learn more about [Yield DTF revenue handling](core-components/yield-dtfs/protocol-operations.md#revenue-handling).

**Index DTFs:** Two fee streams generate revenue for Index DTFs. A TVL fee, akin to a management fee, is assessed block-by-block as a percentage (max. 10% annualized) of the token's TVL. A mint fee (max. 5%) is assessed each time a DTF is minted. This is performed 100% onchain. Learn more about [Index DTF fees](core-components/index-dtfs/fees/README.md).

</details>

<details>

<summary>How do Yield DTFs harvest and reflect yield in price?</summary>

As underlying assets appreciate or rewards are earned, more DTF tokens can either be minted or obtained through revenue auctions. These tokens are subsequently sent to the [Furnace](core-components/yield-dtfs/protocol-operations.md#revenue-distribution-to-yield-dtf-holders) and melted. As a result, Yield DTFs become redeemable for more of their base currency unit. DTFs that accrue revenue to their holders do not rebase, which means that yield-bearing dollar-denominated DTFs (for example) often resemble [flatcoins](https://decrypt.co/155775/flatcoins-new-thing-on-the-horizon-coinbase-ceo-brian-armstrong), rather than stablecoins. That is, their price will continue to increase over time ($1.00 → $1.10 → $1.20, and so on). Learn more about [RToken revenue handling](core-components/yield-dtfs/protocol-operations.md#revenue-handling).

</details>

<details>

<summary>How is revenue distributed to DTF holders, RSR stakers, &#x26; vote-lockers?</summary>

For both Yield and Index DTFs, governance determines how protocol revenue is routed—whether to RSR stakers, vote-lockers, DTF holders, or elsewhere.

**Yield DTFs**: Once a threshold value has been met, “revenue auctions” sell accrued rewards to buy RSR, which is subsequently distributed to RSR stakers. This process increases the redemption ratio of the staked RSR token (e.g. eusdRSR) relative to plain RSR. Learn more about [Yield DTF revenue handling](core-components/yield-dtfs/protocol-operations.md#revenue-handling).

**Index DTFs**: Mint fees are charged at issuance and TVL fees accrue continuously. Both are collected in the DTF token itself and routed to governance-selected recipients after a platform fee is applied.

</details>

<details>

<summary>How are DTF pegs maintained and what anti-bank run mechanisms are built in?</summary>

DTF pegs are maintained through permissionless onchain minting and redemption, allowing anyone to arbitrage price discrepancies between the DTF token and its underlying collateral’s net asset value.

Anti-bank run mechanisms include verifiable reserves, predictable recovery, overcollateralization, and proportional funds distribution, all of which are 100% onchain.

**DTFs are not algorithmic (no recursive endogenous collateral)**

DTFs do not have the recursive, negative feedback loops found in certain algorithmic stablecoins because DTFs are not minted from self-referential endogenous collateral. DTFs are 1:1 backed with exogenous assets with verifiable reserves onchain.

**RSR overcollateralization for Yield DTFs**

Yield DTFs can also employ RSR overcollateralization to promote peg protection. In the event that one of the exogenous assets in a Yield DTF basket drops by 10%, 20% or even 100%, the protocol would slash RSR stakers and sell the failing collateral to buy the pre-programmed emergency collateral basket. Briefly the DTF would be below peg, yet the 100% redemption outcome would be predictable and verifiable given the onchain overcollateralization. This mechanism was battle-tested during the Silicon Valley Bank run that played a role in the March 9, 2023 depeg of USDC — learn more about Yield DTFs’ autonomous self-healing.

**Proportional distributions in catastrophic defaults**

Should there be a case where Yield DTF collateral defaults and the RSR overcollateralization pool is spent with net collateral at < 100% of target price, the affected holders receive proportional distributions rather than first-come-first-served exits, eliminating bad debt without causing a hyperinflationary event.

</details>

<details>

<summary>How does DTF governance work?</summary>

The governance process follows a transparent and democratic approach. Stakers and vote-lockers can propose, discuss, and vote on changes to the DTF(s) they're staked or vote-locked on.

For Yield DTFs, Governor Anastasius is the protocol's recommended governor implementation.

For Index DTFs, the Reserve Optimistic Governor provides a dual-path governance model. Standard proposals follow the traditional flow — proposal, voting delay, active voting, timelock queue, and execution — and are used for high-impact decisions like modifying fees or basket composition. Optimistic proposals skip affirmative voting entirely; once proposed, they enter a short veto window and execute automatically unless enough token holders vote Against to meet the veto threshold. If vetoed, the proposal automatically transitions into a standard proposal for full community deliberation. Only whitelisted actions can be proposed optimistically, and governance infrastructure changes are permanently blocked from the fast path.

To participate in the governance of a specific DTF, go to the Reserve app. Select the DTF you wish to govern and select the Governance section. You will be able to view all the governance proposals that have been submitted for the DTF. If there is any active proposal, you can select it and vote for or against it.

</details>
