# Overview

The Reserve Index Protocol provides a lightweight framework for wrapping handfuls to hundreds of ERC-20 tokens into single fungible assets. The protocol enables permissionless access, efficient rebalancing, and customizable onchain governance for Index Decentralized Token Funds (DTFs):

* **Permissionless NAV-based issuance.** Anyone can mint or redeem directly at real-time asset value—no authorized participants or intermediaries required.
* **Light footprint & broad asset universe.** No price oracles or collateral plugins, so virtually any ERC-20 (including LSTs, LP tokens, and bridged assets) can be indexed.
* **Dutch-auction rebalancing & onchain price discovery.** Dutch auctions minimize MEV and slippage by sourcing deep onchain liquidity from DEXs and solver networks (e.g., CoW Swap) for rebalancing.
* **Continuous fees & protocol revenue.** Block-by-block TVL (management) and Mint fees accrue transparently in DTF shares, routed according to governance specification.
* **Custom onchain governance.** Flexibly-structured onchain governance can be governed by any token.

This document surveys the core mechanics that keep an Index DTF reliable and decentralized, from efficient rebalancing to fee flows and onchain governance.

## Rebalancing & tracking

Index DTFs are periodically rebalanced to target weights through onchain Dutch auctions. Rebalance timing is set by governance. Auctions are executed onchain through an autonomous process:

{% stepper %}
{% step %}
### Measure the live basket

Current token proportions are calculated onchain.
{% endstep %}

{% step %}
### Open auctions

Any surplus token is offered along a declining-price curve in exchange for a deficit token.
{% endstep %}

{% step %}
### Clear via open markets

Solvers such as CoW Swap and direct DEX takers compete to fill orders, minimizing slippage and MEV.
{% endstep %}

{% step %}
### Settle and update weights

Whenever an auction fills a bid the basket’s composition is updated atomically.
{% endstep %}
{% endstepper %}

Because every step is deterministic and transparent, arbitrageurs quickly remove price discrepancies, keeping the market price close to NAV and limiting tracking error. Auction cadence & duration are adjustable by governance, enabling different index strategies without touching core code.

## Fees & revenue

Unlike Yield DTFs, Index DTFs do not rely on yield-bearing collateral for revenue. Instead, two fee streams compensate governance, RSR holders, and governance-chosen addresses.

| Fee type             | Basis             | Range    | Accrual method                     |
| -------------------- | ----------------- | -------- | ---------------------------------- |
| **TVL (management)** | Basket NAV        | < 5% APY | Mint new DTF shares block-by-block |
| **Mint**             | Incoming issuance | < 1%     | Fee deducted from each mint        |

All newly minted shares flow first to the platform take-rate, with the remainder split between addresses set by governance. Currently, the platform fee is used to automatically buy and burn RSR.

## Governance

Each Index DTF acts like its own miniature protocol with governance rules chosen at deployment.

The deployer selects any ERC-20 token—RSR by default—for vote-locking, and holders of that locked token steer the DTF’s evolution via onchain proposals.

The Reserve Optimistic Governor provides a dual-path model: a **standard path** for high-impact decisions that require full community voting, and an **optimistic path** for routine operations that execute quickly unless vetoed by token holders.

### What governance decides

| Area                        | Typical settings                                                                                |
| --------------------------- | ----------------------------------------------------------------------------------------------- |
| **Assets & target weights** | Add, remove, or re-weight assets to reflect the desired index methodology                       |
| **Fee schedule**            | TVL (management) and Mint fees, plus the split between the platform and DTF-specific recipients |
| **Rebalancing parameters**  | Auction cadence & duration                                                                      |
| **Revenue routing**         | Addresses and allocations for fee revenue                                                       |

### How it works in practice

* **Standard proposals** are created, voted on, and executed entirely onchain, with timelocks to ensure time to react. This path is used for high-impact changes such as fees, basket composition, and role assignments.
* **Optimistic proposals** skip affirmative voting and execute automatically after a short veto window, unless enough token holders vote Against. This path is used for routine operations like launching rebalance auctions.
* Every proposal, vote, and execution is permanently recorded onchain, giving users, auditors, and regulators a transparent change history.

Read more about how **Reserve Optimistic Governance** works [here](optimistic-governance.md).

Because governance is modular and token-agnostic, one Index DTF might run a simple one-token DAO, while another could delegate voting power across multiple stakeholders or adopt more sophisticated vote-lock mechanics over time.

{% hint style="warning" %}
As with any smart contract application, the actual behavior may vary from the intended behavior, and it's safest to wait for an application to be in use for a long period of time before trusting it to behave as expected. This overview and subsequent documentation describe the intended behavior.
{% endhint %}

{% hint style="info" %}
The Reserve Register does not yet support permissionless creation of Index DTFs.
{% endhint %}

## Non-compatible ERC20 assets

The following types of ERC20s are not supported to be used directly in an Index DTF. These tokens should be wrapped into a compatible ERC20 token to be used within the protocol.

* Tokens that take a "fee" on transfer
* Tokens that do not expose the decimals() function in their interface. Decimals should always be between 1 and 27.
* ERC777 tokens which could allow reentrancy attacks
* Tokens with multiple entry points (multiple addresses)
* Tokens that do not adhere to the ERC20 standard in general
