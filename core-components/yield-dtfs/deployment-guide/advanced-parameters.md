# Advanced parameters

When deploying a Yield DTF, the deployer has the ability to configure many different advanced parameters. The following list goes into detail about what these parameters do and some of the factors the deployer should keep in mind to set them.

As many of these parameters concern the [Protocol Operations](../protocol-operations.md), we advise reading through that section of the documentation first — it will give the deployer the necessary context to fully understand all parameters.

## Backing Manager parameters

### Trading delay (s)

The trading delay defines how many seconds should pass after the basket has been changed before a trade can be opened. It can be useful to have some buffer time before rebalancing in order to ensure sufficient auction participation.

However, in the modern MEV day it's a bit unnecessary. Some new RTokens may still find it useful to set a nonzero value, but in general it can be safely set to 0 for most Yield DTFs.

Default value: `0` = 0s

### Warmup period

The warmup period defines how many seconds should pass after a collateral asset resumes being SOUND before issuance or trading can resume.

This parameter is useful to defend against short-term oracle manipulation attacks and in cases where asset prices may be experiencing larger fluctuations.

Default value: `900` = 15 minutes

### Batch auction length (s)

The length of a Gnosis EasyAuction, one of two trading methods supported by the Reserve Protocol.

Considerations when determining this value:

* If set too low, back-to-back batch auctions may not give arbitrageurs enough time to complete arbitrage loops that involve centralized exchanges. We don’t want capital-constrained traders to have to sit out every-other auction.
* If set too high, fewer auctions will fill in general due to organic price movement, and the protocol will be much slower to rebalance. This is because the price can swing more than maximum trade slippage in the unfavorable direction.

Default value: `900` = 15 minutes

### Dutch auction length (s)

The length of the custom Dutch Auction, one of two trading methods supported by the Reserve Protocol.

Considerations:

* If set too low, more slippage will be encountered due to precision loss.
* If set too high, the protocol will be slow to rebalance.

Default value (mainnet): `1800` = 30 minutes\
Default value (Base/Arbitrum): `900` = 15 minutes

### Backing buffer (%)

The backing buffer is a percentage value that describes how much extra collateral to hold as backing in the Backing Manager. It is important for preventing RSR seizure during normal rebalancing.

* Too low a backing buffer can result in RSR seizure during normal rebalancing. While unpleasant, the stronger reason to avoid this is to prevent situations where RSR stakers are incentivized to unstake before rebalancing proposals in order to avoid paying their fair share. The backing buffer should be set high enough to prevent this outcome.
* Too high a backing buffer can cause RToken and RSR staker yields to drop during large supply increases as new issuance creates additional debt burdens that must be filled in with new appreciation. In general the amount of time this takes is short, but when an RToken grows fast enough, backing-buffer shortfall growth can outpace yield accrual and result in no revenue being recognized for a while.

Note: It is not important to consider the backing buffer for _default_ scenarios, only governance-led rebalances.

There is no one-size-fits-all — each RToken should customize its backing buffer parametrization based on:

{% stepper %}
{% step %}
### How many days of yield it will take to fill the backing buffer

Consider expected revenue cadence and how long it takes to refill the buffer with normal yield.
{% endstep %}

{% step %}
### Expected loss due to slippage if the least-liquid backing collateral is removed

Assess the liquidity and potential slippage cost when removing least-liquid assets.
{% endstep %}

{% step %}
### Sensitivity of DTF holders to variance in their yield

Consider how much yield volatility holders will tolerate.
{% endstep %}
{% endstepper %}

Default value: `1e15` = 0.1%

### Max trade slippage (%)

The maximum trade slippage is a percentage value that describes the maximum deviation from oracle prices that any trade that the protocol performs can clear at. Oracle prices have ranges of their own; the maximum trade slippage permits additional price movement beyond the worst-case oracle price.

Setting this percentage too high could cause the protocol to take high losses if auctions are illiquid.

Default value: `0.005e18` = 0.5%

## Supply throttles

To restrict the system to organic patterns of behavior, Yield DTFs maintain two supply throttles, one for net issuance and one for net redemption.

When a supply change occurs, a check is performed to ensure this does not move the supply more than an acceptable range over a period; a period is fixed to be an hour.

The throttling mechanism works as a battery: after a large issuance/redemption, the limit recharges linearly to the defined maximum at a defined speed of recharge.

Limits can be defined (for issuance and redemption) in DTF token amounts and/or as a percentage of the DTF token supply.

Important: The redemption throttle must always exceed the issuance throttle.

### Issuance throttle rate (%)

A fraction of the RToken supply that indicates how much net issuance to allow per hour. Can be set to 0 to solely rely on the issuance throttle amount.

Default value: `1e17` = 10% per hour

### Issuance throttle amount

A quantity of DTF token that serves as a lower-bound for how much net issuance to allow per hour. Defined in DTF token amounts.

Must be at least 1 whole DTF token. Can be set to 0 to solely rely on the issuance throttle rate.

Default value: `2e24` = 2,000,000 tokens

### Redemption throttle rate (%)

A fraction of the RToken supply that indicates how much net redemption to allow per hour. Can be 0 to solely rely on the redemption throttle amount.

Default value: `12.5e16` = 12.5% per hour

### Redemption throttle amount

A quantity of RToken that serves as a lower-bound for how much net redemption to allow per hour. Defined in RToken amounts.

Must be at least 1 whole RToken. Can be set to 0 to solely rely on the redemption throttle rate.

Default value: `2.5e24` = 2,500,000 RToken

## Other parameters

### Short freeze duration (s)

The number of seconds a short freeze lasts.

Governance can freeze forever.

Default value: `259200` = 3 days

### Long freeze duration (s)

The number of seconds a long freeze lasts.

Long freezes can be disabled by removing all addresses associated with the role.

Default value: `604800` = 1 week

### Withdrawal leak (%)

Most RSR withdrawals will not trigger a refresh of asset prices (which can determine whether collateral has defaulted), allowing for cheaper withdrawals. The tradeoff of not refreshing prices with every withdrawal is that the system runs the risk of prices becoming stale, and the system not being able to react sufficiently swiftly to market conditions.

This parameter attempts to balance these tradeoffs, by setting a fraction of RSR stake that should be permitted to withdraw without a refresh of asset prices. When cumulative withdrawals (or a single large withdrawal) exceed this fraction, gas must be spent to refresh all assets. Setting this number larger allows unstakers to save more on gas at the cost of allowing more RSR to exit improperly in the event of a default.

Default value (mainnet): `5e16` = 5%\
Default value (Base/Arbitrum): `1e16` = 1%

### Unstaking delay (s)

The unstaking delay is the number of seconds that all RSR unstakings must be delayed in order to account for stakers trying to frontrun defaults.

Whenever an RSR staker chooses to withdraw their RSR, they must submit a transaction, wait X amount of time, and then submit another transaction to complete the withdrawal. During the waiting period, their RSR continues to be subject to forfeiture in the case of a collateral token default, but stops earning its pro-rata share of the RToken’s revenue.

The goal of this delay is to make it so that at any point in time, staked RSR that has not had a withdrawal transaction initiated is at least X time away from being withdrawn.

Default value: `1209600` = 2 weeks

### Reward ratio (decimals)

The reward ratio is the percentage of the current reward amount that should be handed out per second. It applies to both the Furnace (RToken melting) and stRSR (RSR rewards).

Default value: `1146076687500` = a half-life of 7 days.

Mainnet reasonable range: `1e11` to `1e14`

### Minimum trade volume ($)

The minimum trade volume represents the smallest amount of value that is worth executing a trade for.

* Setting this too high will result in auctions happening infrequently or the RToken taking a haircut when it cannot be sure it has enough staked RSR to succeed in rebalancing at par.
* Setting this too low may result in more slippage or allow griefers to delay important auctions. The variable should be set such that donations of size minTradeVolume would be worth delaying trading auctionLength seconds.

We expect auction bidders to pass through any gas fees they pay during trading to the protocol. They are under competition, so those that do not will find themselves with less capital over time relative to those that do.

Warning: Every collateral in the basket should be a large enough portion of the basket that it is worth trading at the configured minTradeVolume at typical supply levels.

Default value: `1e21` = $1k

### DTF max trade volume ($)

This represents the maximum sized trade for any trade involving RToken, in terms of value.

Note: Each collateral plugin will also have its own max trade volume defined. Any trade will be sized to be less than or equal to the minimum of both plugins' max trade volumes.

Default value: `1e24` = $1m
