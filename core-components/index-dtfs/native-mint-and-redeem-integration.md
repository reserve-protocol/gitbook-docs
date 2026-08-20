# Native mint and redeem integration

Use direct Folio calls when an aggregator, solver, market maker, or advanced application wants to construct its own Index DTF routes. Minting and redemption are permissionless and atomic at the protocol level, but the caller must supply or receive every current basket token.

This guide applies to Index DTFs. Yield DTFs use a different protocol and contract interface.

## Before integrating

1. Confirm the chain ID and canonical DTF proxy address.
2. Read the proxy's current implementation and verified ABI from the chain explorer.
3. Do not assume the implementation on the GitHub `main` branch matches the deployed proxy.
4. Read the basket and fee state for every new quote; both can change through governance.

## Native mint

### 1. Choose gross shares

The `shares` argument is the gross number of DTF shares issued. The receiver gets fewer shares after the mint fee.

Call:

```solidity
toAssets(shares, Math.Rounding.Ceil)
```

The returned ordered `assets` and `amounts` are the exact basket tokens and quantities required for that gross issuance at the quoted state.

Once a day, TVL fee accrual can result in the `amounts` changing in the downward direction. This can make a preceding view-only quote differ slightly at execution time; tightly bounded allowances, a fresh simulation, and atomic routing should cause stale quotes to fail safely.

### 2. Calculate net shares and `minSharesOut`

Read `mintFee()` directly from the Folio and mirror the contract's upward rounding:

```text
ceilDiv(x, y) = x == 0 ? 0 : (x - 1) / y + 1

totalFeeShares = ceilDiv(shares × mintFee, 1e18)
expectedSharesOut = shares - totalFeeShares
```

Set:

```text
minSharesOut = expectedSharesOut
```

for strict protection, or subtract a small, explicitly disclosed tolerance if the integration intentionally accepts a fee change between quote and execution.

`minSharesOut` protects only the net DTF shares received. It does not protect the price or slippage of swaps used to acquire the basket.

Use integer arithmetic rather than `shares × (1 - mintFee)` so the result matches the contract's rounding exactly.

### 3. Acquire and approve the basket

Acquire every component token, approve the Folio proxy for no more than the quoted/tightly bounded amount, and keep the acquisition plus mint in one router transaction when offering a single-token UX.

### 4. Mint

```solidity
mint(uint256 shares, address receiver, uint256 minSharesOut)
```

The Folio transfers each required component from `msg.sender` and mints the net shares to `receiver`. Refund any unused input or component-token dust from the integration router.

## Native redemption

### 1. Quote the current basket

For the DTF shares being redeemed, call:

```solidity
toAssets(shares, Math.Rounding.Floor)
```

Use the returned asset order exactly. Do not sort the addresses or reuse an array from an earlier quote.

### 2. Set component minimums

Create `minAmountsOut` by applying the integration's tolerance to each expected component amount:

```text
minAmountsOut[i] = floor(expectedAmounts[i] × (10_000 - toleranceBps) / 10_000)
```

These minimums protect the protocol-level component outputs. If components are subsequently swapped into one token, enforce a separate aggregate minimum output and deadline for those swaps.

### 3. Redeem

```solidity
redeem(
  uint256 shares,
  address receiver,
  address[] assets,
  uint256[] minAmountsOut
)
```

The caller's DTF shares are burned and the basket tokens are sent to `receiver`. For a single-token exit, set the integration router as `receiver`, swap all components, enforce the user's final minimum output, and transfer the result to the user in the same transaction.

Similar to minting, once a day, TVL fee accrual can result in the `amounts` changing in the downward direction. Use `minAmountsOut` for protection. 

## Router and quote requirements

* Re-read `toAssets` for each quote and use the correct rounding direction.
* Treat the returned asset ordering as part of the quote.
* Bound component approvals and revoke or reset reusable approvals where appropriate.
* Enforce swap-level slippage and a deadline separately from Folio protections.
* Simulate the complete transaction immediately before submission.
* Return residual input and component-token dust.
* Requote after a basket, fee, allowance, balance, or block-state change.
* Test stale baskets, insufficient shares out, insufficient component output, expired routes, and deprecated Folios.

## Contract references

* [Index DTF minting and redemption](minting-and-redeeming.md)
* [Index DTF pricing and `toAssets`](pricing.md)
* [Protocol deployment addresses](smart-contracts.md)
* [Reserve Index Protocol source](https://github.com/reserve-protocol/reserve-index-dtf)
