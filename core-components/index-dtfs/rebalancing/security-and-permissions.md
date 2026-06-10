# Security & permissions

## Permissions <a href="#security-and-permissions" id="security-and-permissions"></a>

* **Only** the `ADMIN` can set the auction length.
* **Only** the `ADMIN` can allow/disallow permissionless bidding.
* **Only** the `ADMIN` can configure the Trusted Fillers registry.
* **Only** the `ADMIN` can add a token to the basket directly (not via rebalance).
* **Only** the `REBALANCE_MANAGER` can start a rebalance.
* **Only** the `AUCTION_LAUNCHER` can open auctions during the restricted window.
  * After the window, anyone can open auctions (if in priced mode).
* **Anyone** can bid in open auctions.
* **Any** of the `REBALANCE_MANAGER`, `AUCTION_LAUNCHER`, or `ADMIN` roles can close an auction.
* **Any** of the `REBALANCE_MANAGER`, `AUCTION_LAUNCHER`, or `ADMIN` roles can end a rebalance.

