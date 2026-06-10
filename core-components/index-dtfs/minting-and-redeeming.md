# Minting & redeeming

Minting/redeeming directly converts between Index DTFs and their underlying tokens—no authorized participants required. Anyone can mint or redeem permissionlessly.

![](<../../.gitbook/assets/issuance and redemption.svg>)

For convenience, the protocol offers zapper helpers that let users enter or exit using almost any ERC‑20 token or native ETH in one transaction. Manual redemptions and direct contract calls enable more flexibility and provide an escape hatch if other methods are down, ensuring users are not reliant on offchain tools to exit positions.

## Protocol level vs. app level

Market makers and integrators often ask what a mint or redeem actually does under the hood. The mechanics differ depending on whether you interact with the contracts directly or go through the app’s zapper—but in every case, the operation is atomic.

### At the protocol level

Onchain, minting and redeeming are atomic operations in a single transaction: the user exchanges exact amounts of each basket token for an exact number of DTF tokens (or vice versa for a redeem). The exchange ratios are defined by—and readable from—the [DTF contracts](smart-contracts.md), so the precise basket amounts for any mint or redeem can be computed in advance.

### In the app (via the zapper)

When using the zapper, the user only specifies the input/output token and how much they want to spend. The zapper abstracts the rest: it trades the input token into the required basket tokens and executes the `mint` action—or executes the `redeem` action and then trades the basket tokens into the desired output token. The zapper transaction is **also** atomic, just with the added trading step packaged in series with the `mint`/`redeem` step.

### DTFs with offchain liquidity (RFQ / intents)

{% hint style="info" %}
Some DTFs utilize tokens whose main source of liquidity is offchain (e.g., Ondo or Universal assets). For these, the in‑app zapper might not function as well, because there is not sufficient liquidity onchain to route the trading step. Instead, we rely on RFQ / intent systems: approved minters source the underlying basket tokens as needed—effectively acting as the trading layer of the zapper—and execute the mint/redeem functionality to provide the end user with a seamless result.
{% endhint %}

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

See the [smart contracts](smart-contracts.md) section for contract details and addresses.
{% endstep %}
{% endstepper %}
