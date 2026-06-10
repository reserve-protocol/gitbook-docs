# Risks

This section is dedicated to explaining risks associated with using the Reserve platform. Whenever you interact with the Reserve platform, **you as the user assume the risk of doing so**. We believe that making best efforts to comprehensively outline risks creates accountability and empowers users. Any time you are uncertain about a particular risk, we invite you to ask on [Telegram](https://t.me/reservecurrency).

We encourage all users to familiarize themselves with these risks and continually stay updated about changes that may affect the protocol.

## Reserve platform risks - smart contracts

The Reserve platform is built using smart contracts. If there were undiscovered bugs or vulnerabilities in these contracts, they could be exploited leading to loss of user funds. Although the protocol's contracts have undergone multiple security audits, no audit can guarantee complete security.

Smart contract-related risks can manifest in a variety of ways. A number of pertinent examples and categories are detailed below.

### Oracle risks

The Reserve Yield Protocol uses oracles to fetch real-time price data. If these oracles fail or deliver incorrect information, or if the usage of oracles on Reserve is otherwise incorrect, it could impact the protocol's ability to maintain accurate accounting and lead to other operational disruptions. For example, if Chainlink misreports the price of USDC, a Yield DTF may consider a collateral asset to have defaulted and attempt to swap to emergency collateral, potentially at a loss.

### Sandwich attacks and MEV

[MEV](https://ethereum.org/en/developers/docs/mev/) searchers are constantly scanning the Ethereum blockchain to look for profitable opportunities to extract value. When interacting with AMMs (such as Curve Finance for trading DTFs), users should exercise caution and consider the slippage, which dictates the degree to which searchers can extract from users’ transactions. There are other ways for users to independently seek MEV protection, such as through the [Flashbots RPC](https://docs.flashbots.net/flashbots-protect/rpc/quick-start).

### Governance risks

The Reserve team has deployed a suggested governance system for DTFs, which enables fully onchain governance to DTFs. The scope of the powers is broad, so it is possible for governance attacks to happen. These potential attacks could involve an attacker accumulating enough governance power to enact a malicious upgrade allowing them to steal funds.

This type of attack is mitigated through the existence of special roles; however, these special roles must be wielded effectively and benevolently in order to offer meaningful protection. When using a DTF, it’s a good idea to familiarize yourself with who holds each of these special roles, which you can do on the Details + Roles page and the Governance page on the Reserve app.

## Collateral asset risks

### Issuer/custodian

It is possible for stablecoin (and other) issuers to impose restrictions on transferability, through blacklists. Although commonly intended to target sanctioned individuals, it is possible for these powers to extend to DeFi protocols. Additionally, stablecoin issuers are relied on for maintaining healthy reserves of real-world assets. The strategies used to maintain these reserves may sometimes fail, or the custodians themselves may not always act in good faith. The best way we are aware of to familiarize yourself with the risks for any given underlying stablecoin is to review the report on that asset published on [Bluechip](https://bluechip.org/), if they cover that asset.

### Price

DTFs inherit the weighted price of all of their backing collateral assets. Where one or more underlying assets deviate in price (even where they are meant to be pegged), the aggregate price of the DTF will be affected. Fluctuations in DTF price should be anticipated where volatile collateral is present.

A slightly distinct price consideration involves that of the RSR overcollateralizing Yield DTFs. Based on the amount of RSR that must be sold to re-collateralize a default, significant market movements may neutralize the effectiveness of the overcollateralization stemming from a weaker mark to market price.

### Underlying protocols

Reserve DTFs leverage assets from external protocols, like Compound, Aave, Flux, and many more. Users therefore assume all of the risks of these underlying protocols (smart contract, governance, or otherwise) when holding DTFs.

## Reserve platform risks - interfaces

### Frontend operator risks

The Reserve platform can be interacted with directly through its smart contracts, or through third-party-created user interfaces. [The Reserve app](https://app.reserve.org/), for example, is run by a third-party company, and has not yet undergone a technical audit. Users must always be vigilant for malicious or compromised frontends, such as in [Badger’s case](https://rekt.news/badger-rekt/). Even in normal operation, bugs in frontend code may be responsible for requesting erroneous transactions which could result in user losses.
