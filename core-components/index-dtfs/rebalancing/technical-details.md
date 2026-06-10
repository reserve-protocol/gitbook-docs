# Technical details

## Rebalance lifecycle <a href="#rebalance-lifecycle" id="rebalance-lifecycle"></a>

In order to rebalance a DTF, the `REBALANCE_MANAGER` must first start a rebalance. This will set the new basket, per-token limit ranges, and price ranges. The `REBALANCE_MANAGER` will also set the restricted window during which only the `AUCTION_LAUNCHER` can open auctions, and the total time-to-live for the rebalance.

Once a rebalance auction is opened by the `AUCTION_LAUNCHER`, every surplus token in the auction will be sold down to its target basket ratio, and every deficit token will be bought up to its target basket ratio. An auction can include every surplus and deficit token in the basket, or a subset of them. Bidders can bid on any surplus:deficit pair in the auction until they reach their respective target ratios.

1. **Rebalance Initiation**: The `REBALANCE_MANAGER` starts a new rebalance, specifying new basket tokens, target ratios (limits), and price ranges for each asset.
2. **Auction Launcher Window**: For a restricted period, only the `AUCTION_LAUNCHER` can open auctions, with the ability to fine-tune parameters within governance-set bounds. The `AUCTION_LAUNCHER` can:
   * select a sell limit within the approved sell limit range
   * select a buy limit within the approved buy limit range
   * raise starting price by up to 100x
   * raise ending price arbitrarily (can cause auction not to clear, same end result as closing auction)
3. **Permissionless Auctions**: After the restricted window, anyone can open auctions using the pre-approved parameters.
4. **Bidding**: Anyone can bid in open auctions, exchanging tokens at a price determined by an exponential decay curve. Every successful bid will move the DTF closer to the target basket ratio.
5. **Auction Close**: Auctions end when their time elapses or their limits are reached. The rebalance technically ends when the rebalance period expires.

### Key concepts and structs <a href="#key-concepts-and-structs" id="key-concepts-and-structs"></a>

* **BasketRange**: `{ spot, low, high }` — Target, minimum, and maximum ratios of a token to DTF (Folio) shares (D27 precision).
* **Prices**: `{ low, high }` — Price range for each asset (D27 precision).
* **RebalanceDetails**: `{ inRebalance, limits, prices }` — Per-token rebalance state.
* **Auction**: `{ rebalanceNonce, sellToken, buyToken, sellLimit, buyLimit, startPrice, endPrice, startTime, endTime }` — State of a single auction.

### Auction pricing <a href="#auction-pricing" id="auction-pricing"></a>

For each token supplied to the rebalance, the `REBALANCE_MANAGER` provides a `low` and `high` price estimate. These should be set such that in the vast majority (99.9%+) of scenarios, the asset's price on secondary markets lies within the provided range. The maximum allowable price range for a token is 1e2: `high / low` must be <= 1e2.

If the price of an asset rises above its `high` price, this can result in a loss of value for Folio holders due to the auction price curve on a token pair starting at too low a price.

When an auction is started, the `low` and `high` prices for both assets are used to calculate a `startPrice` and `endPrice` for the auction.

There are 2 ways to price assets, depending on the risk tolerance of the `REBALANCE_MANAGER`:

1. **Priced**

* The `REBALANCE_MANAGER` provides a list of NONZERO prices for each token. No token price can be 0.
* The `AUCTION_LAUNCHER` can always choose to raise the starting price up to 100x from the natural starting price, or raise the ending price arbitrarily (up to the starting price). They cannot lower either price.

2. **Unpriced**

* The `REBALANCE_MANAGER` provides ALL 0 prices for each token. This allows the `AUCTION_LAUNCHER` to set prices freely and prevents auctions from being opened permissionlessly.

Overall the price range for any Dutch auction (`startPrice / endPrice`) must be less than `1e6` to prevent precision issues.

#### Price curve <a href="#price-curve" id="price-curve"></a>

Note: The first block may not have a price of exactly `startPrice` if it does not occur on the `start` timestamp. Similarly, the price may not be exactly `endPrice` in the final block if it does not occur on the `end` timestamp.

All auctions use an exponential decay price curve from `startPrice` to `endPrice` over the auction duration. The price at any time `t` is:

```
P(t) = startPrice * exp(-k * t)
```

Where `k` is calculated so that `P(auction.endTime) = endPrice`.

#### Lot sizing <a href="#lot-sizing" id="lot-sizing"></a>

Auctions are sized by the difference between current balances and the basket ratios provided by the `limits`.

The auction `sellAmount` represents the single largest quantity of sell token that can be transacted without violating the `limits` of either token in the pair.

In general, it is possible for the `sellAmount` to either increase or decrease over time, depending on whether the surplus of the sell token or deficit of the buy token is the limiting factor.

1. If the surplus of the sell token is the limiting factor, the `sellAmount` will increase over time.
2. If the deficit of the buy token is the limiting factor, the `sellAmount` will decrease over time.

#### Auction participation <a href="#auction-participation" id="auction-participation"></a>

Anyone can bid in any auction up to and including the `sellAmount` size, as long as the `price` exchange rate is met.

```
/// @return sellAmount {sellTok} The amount of sell token on sale in the auction at a given timestamp
/// @return bidAmount {buyTok} The amount of buy tokens required to bid for the full sell amount
/// @return price D27{buyTok/sellTok} The price at the given timestamp as a 27-decimal fixed point
function getBid(
    uint256 auctionId,
    uint256 timestamp,
    uint256 maxSellAmount
) external view returns (uint256 sellAmount, uint256 bidAmount, uint256 price);
```

