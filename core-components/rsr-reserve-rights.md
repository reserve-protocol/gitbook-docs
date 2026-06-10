# RSR (Reserve Rights)

RSR is the token that allows anyone to own a share in the value of everything. It enables participation in DTF governance and returns a portion of protocol fees to RSR holders through token burns.

Below is a detailed overview of RSR and how it functions within the Reserve ecosystem.

***

## Technical overview

Reserve Rights (RSR) is an ERC-20 token that unifies governance, risk management, and value accrual across the Reserve ecosystem. RSR has three main roles:

* **Staking on Yield DTFs**: RSR stakers provide governance and first-loss capital (overcollateralization) in exchange for DTF yield.
* **Vote-locking on Index DTFs**: RSR is the default governance token for Index DTFs, controlling basket changes, parameters, and upgrades—and sharing in fees when enabled.
* **Deflationary sink**: A portion of every Index DTF’s mint and TVL fees is used to market-buy RSR and burn it, steadily reducing the circulating supply.

All the relevant information regarding RSR’s supply, audits, etc. can be found through the links below:

* [Source code](https://github.com/reserve-protocol/rsr-mainnet)
* [Live contract](https://etherscan.io/address/0x320623b8e4ff03373931769a31fc52a4e78b5d70#code)
* [Audit](https://github.com/reserve-protocol/rsr-mainnet/blob/master/audits/solidified/Audit%20Report%20-%20Reserve%20Token%20%5B3%20Jan%202022%5D-2.pdf)
* [Supply dashboard](https://coinmarketcap.com/currencies/reserve-rights/#token_unlocks)
* Markets: [CMC](https://coinmarketcap.com/currencies/reserve-rights/markets/) / [CoinGecko](https://www.coingecko.com/en/coins/reserve-rights#markets)

### Yield DTFs

#### Staking on Yield DTFs

In the Reserve Yield Protocol, staked Reserve Rights (stRSR) provides an overcollateralization mechanism to protect DTF holders in the unlikely event of a collateral token default. RSR holders can stake on any one Yield DTF, or split their tokens across multiple DTFs, or choose not to stake.

In return for providing this overcollateralization, RSR stakers can expect to receive a portion of the revenue from the specific DTF that they stake on. As a general rule, RSR stakers will receive more revenue as the market cap of the DTF they stake on increases.

When RSR is staked on a DTF, it is deposited into a staking contract specific to that DTF, and the staker receives a corresponding ERC-20 token representing their staked RSR position on that DTF. This token is transferable and fungible with other staked RSR balances for that DTF, so you can send any portion of the staked position to someone else or trade it, and the new holder can unstake it if they choose to.

![](<../.gitbook/assets/staking rsr on a yield dtf.svg>)

Staked RSR can earn rewards, based on three factors:

1. The amount of revenue the DTF generates
2. The portion of revenue that governance has directed to RSR stakers
3. Your portion of the total RSR staked on that DTF

Example calculation (illustrative values):

* DTF revenue: $100
* Revenue designated for RSR stakers: 20%
* Total RSR staked: 1,000
* Your stake: 100

Reward: $100 \* 0.2 \* (100/1000) = $2

The protocol stores revenue for a particular Yield DTF in different ERC-20s (including DTFs). When staking rewards are distributed, it market-buys RSR via auctions with these ERC-20s and deposits it into the staking contract to distribute the rewards to RSR stakers. Thus, as rewards are earned, the exchange rate of staked RSR to RSR increases.

When RSR is staked, it is actually at stake. Staked RSR can be seized by the protocol in the event of a collateral token default, in order to cover losses for DTF holders. It is seized pro-rata if this happens.

Unstaking RSR comes with a delay, configurable by governance and typically between about 7 and 30 days. This delay ensures staked RSR remains available long enough to cover potential defaults. During the unstaking delay period, the staker does not earn any rewards. Users can cancel unstakings at any time to resume staking and earning rewards.

The easiest way to stake your RSR is to use a user interface that interacts with the Reserve Protocol smart contracts, such as the [Reserve app](https://app.reserve.org/). For a tutorial, see [this article](https://blog.reserve.org/how-to-stake-reserve-rights-rsr-f5393fe24573).

**Slashing**

If a collateral defaults, RSR will be seized to cover the loss, causing the stRSR exchange rate to RSR to decrease. In most cases this will not impact balances, but in the rare event of a 100% slashing, balances can be zeroed to allow the staking pool to continue operating under new stakers.

### Vote-locking

#### Vote-locking on Index DTFs

With the Reserve Index Protocol, RSR gained vote-locking. By default, Index DTFs use RSR as their governance token (a creator may designate any ERC-20 instead). Vote-locking commits the chosen token to a specific Index DTF for a minimum period—currently a one-week unlock delay—where it carries voting weight over that DTF’s parameters and upgrades.

When tokens are locked, the entire balance counts 1-for-1 toward governance power. This allows participants to influence basket composition, weighting rules, fee splits, and rebalance cadence proportionally to their economic stake.

For DTFs that enable revenue sharing, vote-lockers earn a pro-rata slice of the DTF’s minting and TVL fees—creating a reward stream independent of staking yields in Yield DTFs. Although any ERC-20 can be the locking asset, Index DTF fees are used to market-buy and burn RSR, contributing to a deflationary sink as adoption grows.

For a tutorial on vote-locking, see [this article](https://blog.reserve.org/vote-locking-on-reserve-19e19201d78e).

### Fee burn

#### Index Protocol fee burn

Every Index DTF levies platform fees on new mints and on the value locked in the contract. Those fees are swept into a protocol-level contract that automatically uses them to market-buy RSR and then sends the purchased tokens to the burn address, permanently removing them from circulation. Because the burn mechanism lives at the protocol layer, it applies to every Index DTF—regardless of which governance token the fund chooses for vote-locking—so indexes across a diverse ecosystem all contribute to the same burn stream. The percentage of each fee that feeds the burn contract is governed onchain.

### Governance

While each DTF can have its own governance system, most DTFs are expected to use the default configuration where the amount of RSR a participant stakes or locks serves as voting weight.

Both Yield DTFs and Index DTFs follow the same community-driven governance flow:

1. Proposal: Any address holding the proposal threshold of voting weight may submit a change.
2. Vote: Holders cast votes for, against, or abstain (directly or by delegation) over a fixed voting period.
3. Execution: Successful proposals queue in a timelock, giving stakeholders and markets time to react before the update is applied onchain.

Anyone can propose changes to modify or improve a DTF.

#### Governor Anastasius

The Reserve team recommends Governor Anastasius for Yield DTFs (a slightly modified version of [OpenZeppelin Governor](https://docs.openzeppelin.com/contracts/4.x/api/governance)). It allows RSR holders to propose, vote on, and execute proposals using delegation.

Configurable governance parameters include:

* Proposal Threshold
* Quorum
* Voting snapshot delay
* Voting period
* Execution delay

Default end-to-end timing (8 days total):

* Voting snapshot delay: 2 days
* Voting period: 3 days
* Execution delay: 3 days

In addition to the Owner role, each Yield DTF has assignable roles: Pauser, Short Freezer, Long Freezer, and Guardian. These can be granted by the DTF deployer/owner and put the DTF into protected states in case of an attack, exploit, or bug:

* Paused: All interactions besides redemption, ERC-20 functions, staking of RSR, and rewards payout are disabled.
* Frozen: All interactions besides ERC-20 functions and staking of RSR are disabled.

For more details, see [System States + roles](https://github.com/reserve-protocol/protocol/blob/master/docs/pause-freeze-states.md).

### Supply

Reserve Rights (RSR) has a fixed total supply of 100 billion tokens, of which currently 53.5b are in circulation. The remaining 46.5b belong to the Slow and Slower Wallets.

The [Slow Wallet](https://etherscan.io/address/0x4903dc97816f99410e8dfff51149fa4c3cdad1b8#tokentxns) is a locked wallet controlled by the Reserve project team, used to fund DTF adoption initiatives. It has a hard-coded 4-week delay after initiating each withdrawal transaction onchain.

In January 2024, organizational changes named Confusion Capital as the entity to manage funding for the Reserve Ecosystem (including Best Friend Finance and ABC Labs). The [_Slower Wallet_](https://etherscan.io/address/0x0774dF07205a5E9261771b19afa62B6e757f7eF8#tokentxns) is administered by Confusion Capital and receives a portion of funds from the Slow Wallet. It maintains the 4-week withdrawal delay and adds a throttle: no more than 1% of the total supply of RSR can be withdrawn in any 4-week period. The change reduces the trust required in Confusion Capital.

Illustrative example of the throttle behavior:

{% stepper %}
{% step %}
### Initial state

No withdrawals are made for a while.
{% endstep %}

{% step %}
### Throttle limit calculation

Throttle limit (max available): 1,000,000,000 RSR
{% endstep %}

{% step %}
### Withdrawal initiated

Withdrawal initiated for 1,000,000,000 RSR → Throttle limit becomes 0 RSR
{% endstep %}

{% step %}
### Time passes

2 weeks pass → Throttle limit: 500,000,000 RSR
{% endstep %}

{% step %}
### Second withdrawal

Withdrawal initiated for 250,000,000 RSR → Throttle limit: 250,000,000 RSR
{% endstep %}

{% step %}
### More time

1 week passes → Throttle limit: 500,000,000 RSR
{% endstep %}
{% endstepper %}

Future RSR emissions follow a deterministic schedule that emulates Bitcoin's emissions curve. For more details see [Reducing RSR emissions](https://blog.reserve.org/reducing-rsr-emissions-6da7f35917ba) or view the unlock timeline on [CoinMarketCap's Token Unlock page](https://coinmarketcap.com/currencies/reserve-rights/#token_unlocks).

![](<../.gitbook/assets/rsr unlock schedule 1.png>)
