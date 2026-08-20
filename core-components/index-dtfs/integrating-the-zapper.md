# Integrating the Zapper

Use the Reserve Zapper when users should be able to buy or sell an Index DTF with a single token. It quotes across supported execution providers and abstracts the component swaps plus the Folio mint or redemption. Native Zapper and aggregator routes return an atomic transaction; RFQ/intent routes such as CoW Swap instead require a signed order that is filled asynchronously by solvers.

For most wallets and fintech applications, embedding the maintained React component is the shortest integration path. Applications with a custom interface can consume the same quote API directly.

## Option A: React component

### 1. Install

```bash
pnpm add @reserve-protocol/react-zapper
pnpm add react@^18 react-dom@^18 @tanstack/react-query@^5 wagmi@^2 viem@^2
```

The package uses the application's existing Wagmi and TanStack Query providers.

### 2. Render the Zapper

```tsx
import { Zapper } from '@reserve-protocol/react-zapper'
import '@reserve-protocol/react-zapper/styles.css'

export function DtfTrade({ dtfAddress }: { dtfAddress: `0x${string}` }) {
  return <Zapper chain={8453} dtfAddress={dtfAddress} mode="inline" />
}
```

Available display modes are `inline`, `modal`, and `simple`. The component handles quotes, input-token approvals, transaction simulation or intent signing, execution status, slippage controls, and both buy and sell flows. It consumes the host application's Wagmi account; provide the optional `connectWallet` callback when the component should open the host's wallet-connection UI.

See the [React Zapper repository](https://github.com/reserve-protocol/react-zapper) and [live demo](https://react-zapper.reserve.org/) for provider setup and additional props.

## Option B: Zapper API

Use this path for a non-React UI or backend-generated transaction flow. This endpoint selects the Reserve-native Zapper route and returns an atomic transaction rather than an RFQ/intent order.

### 1. Request a quote

Send a `GET` request to:

```text
https://api.reserve.org/api/zapper/{chainId}/swap
```

Required query parameters:

| Parameter | Description |
| --- | --- |
| `chainId` | Base is `8453` |
| `signer` | Wallet that will execute the transaction |
| `tokenIn` | ERC-20 input token, or Viem's `ethAddress` native-token sentinel: `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE` |
| `amountIn` | Input amount in token base units |
| `tokenOut` | DTF address for a mint; desired output token for a redemption |
| `slippage` | Inverse-fraction convention: `200` = 1/200 = 0.5% |
| `trade` | `true` to include component swaps |

Example: quote 100 USDC into an Index DTF on Base:

```text
GET https://api.reserve.org/api/zapper/8453/swap?chainId=8453&signer={WALLET}&tokenIn=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&amountIn=100000000&tokenOut={DTF_ADDRESS}&slippage=200&trade=true
```

To redeem, reverse `tokenIn` and `tokenOut`: use the DTF as `tokenIn`, the desired receive token as `tokenOut`, and express `amountIn` in the DTF's base units.

### 2. Inspect the response

A response uses this envelope:

```json
{
  "status": "success",
  "result": {}
}
```

When `status` is `error`, handle the top-level `error` and do not submit a transaction. A successful `response.result` includes:

* `amountOut` and `minAmountOut`
* `approvalAddress` and `approvalNeeded`
* `gas`, `priceImpact`, and possible component-token `dust`
* `tx.to`, `tx.data`, and `tx.value`
* `validUntil` when the quote has an explicit expiry

Reject an error response, a missing `result.tx`, or an expired quote. Bind the response to the local request context for chain, tokens, amount, and signer because the result does not echo every request field. Applications with stricter transaction policies should also decode or allowlist the returned target and calldata before presenting it for signature.

If `validUntil` is absent, apply a short local TTL and requote before submission; the React package uses 60 seconds.

### 3. Approve when required

For an ERC-20 input, read the wallet's allowance to `approvalAddress`. If it is insufficient, ask the user to approve at least `amountIn`. Native-token inputs do not require an ERC-20 approval.

Do not assume the spender address is constant; use the `approvalAddress` returned by the current quote.

### 4. Simulate and execute

Simulate the returned transaction from `signer`, then submit:

```text
to    = result.tx.to
data  = result.tx.data
value = result.tx.value
```

Fetch a new quote if simulation fails, the quote expires, or any input changes. After confirmation, determine the actual received amount from transaction logs or the wallet's balance change rather than assuming it equals the estimate.

{% hint style="warning" %}
A Zapper quote is wallet-specific and time-sensitive. Never reuse calldata for a different signer, and never weaken the returned minimum-output protection.
{% endhint %}

## Production checklist

* Use base units for every token amount.
* Show `amountOut`, `minAmountOut`, price impact, estimated gas, and dust before execution.
* Use the response's current `approvalAddress`.
* Simulate every executable quote.
* Refresh after approval and before expiry.
* Preserve `tx.value` for native-token inputs.
* Handle quote, approval, simulation, submission, and receipt failures independently.
