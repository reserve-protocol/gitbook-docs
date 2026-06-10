# Minting & redeeming

Minting/redeeming directly converts between Yield DTFs and their underlying collateral—no custodians or order books required. Anyone can mint or redeem permissionlessly.

![issuance and redemption](<../../.gitbook/assets/issuance and redemption.svg>)

For convenience, the protocol offers zapper helpers that let users enter or exit using almost any ERC‑20 token or native ETH in one transaction.

## How to mint or redeem

There are three main ways to mint or redeem. The [Reserve app](https://app.reserve.org/) provides a convenient interface, but any front‑end can access the same permissionless contracts.

{% stepper %}
{% step %}
### Zapper (one‑step)

When to use: You want to use a single ERC‑20 or ETH and let the app handle the swaps.

* Click the **Mint** button on any DTF page → choose **Mint** or **Redeem**
* In the **You use** or **You receive** field, pick any supported token (ETH, USDC, wBTC, etc.)
* Enter an amount and review the quote
{% endstep %}

{% step %}
### Manual mint / redeem

When to use: You hold/want the exact collateral tokens or need precise slippage control.

* Click **Switch to manual mode** on the DTF’s Mint/Redeem page
* Review per‑token amounts to deposit/receive
* Approve any necessary tokens and submit
{% endstep %}

{% step %}
### Direct contract call

When to use: Integrations, scripts, advanced use cases.

* Call `issue(amount)` or `redeem(amount)` on the DTF proxy. Basket ratios are calculated and enforced onchain.

See the [smart contracts](/broken/pages/3aafb33ad9285a046a5b53448d8fdc3120c68ae4) section for contract details and addresses.
{% endstep %}
{% endstepper %}
