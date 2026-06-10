# Minting & redeeming

Minting/redeeming directly converts between Index DTFs and their underlying tokens—no authorized participants required. Anyone can mint or redeem permissionlessly.

![](<../../.gitbook/assets/issuance and redemption.svg>)

For convenience, the protocol offers zapper helpers that let users enter or exit using almost any ERC‑20 token or native ETH in one transaction. Manual redemptions and direct contract calls enable more flexibility and provide an escape hatch if other methods are down, ensuring users are not reliant on offchain tools to exit positions.

## How to mint or redeem

There are three main ways to mint or redeem. The [Reserve app](https://app.reserve.org/) provides a convenient interface, but any front‑end can access the same permissionless contracts.

{% stepper %}
{% step %}
### Zapper (one‑step)

_When to use:_ You want to use a single ERC‑20 or ETH and let the app handle the swaps

* Click the **Buy** or **Sell** button on any Index DTF page
* In the **You use** or **You receive** field, pick any supported token (ETH, USDC, wBTC, etc.)
* Enter an amount and review the quote

Due to the volatile nature of DEXs (decentralized exchanges) and the way the zapper routes tokens, it is common to receive dust amounts of certain tokens. Generally, the amount of dust is on the order of 1–10 bps of the input value.
{% endstep %}

{% step %}
### Manual mint / redeem

_When to use:_ You hold/want the exact collateral tokens or need precise slippage control

* Click **Switch to manual mode** on the DTF’s Mint/Redeem page
* Review per‑token amounts to deposit/receive
* Approve any necessary tokens and submit
{% endstep %}

{% step %}
### Direct contract call

_When to use:_ Integrations, scripts, advanced use cases

Call `mint` or `redeem` on the DTF proxy. Basket ratios are calculated and enforced onchain.

See the [smart contracts](roles.md) section for contract details and addresses.
{% endstep %}
{% endstepper %}
