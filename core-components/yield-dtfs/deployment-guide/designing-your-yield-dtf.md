# Designing your Yield DTF

Before deploying, it’s important to consider what goes into creating a successful Yield DTF. Each DTF will have its own ecosystem use cases, community, and unique go-to-market strategies.

Just because anyone can deploy and/or mint a DTF does not mean adoption is guaranteed. Consider the following points to help ensure success.

## Key questions

{% stepper %}
{% step %}
### Collateral mix

Is the basket diverse and attractive?

* See our [basket design guide](https://blog.reserve.org/rtoken-basket-design-1d20ae03bf89) for key considerations
* Find collateral plugins in the [deployer UI](https://app.reserve.org/deploy/yield-dtf) or [write your own](yield-dtf-deployment-walkthrough.md#appendix)
* Specify emergency backup assets in case of default
{% endstep %}

{% step %}
### Target yield

What are the yield targets (if any) for your DTF?

* Consider risk/reward trade-offs and the DTF's target market
* Check out [DefiLlama](https://defillama.com/yields) for inspiration and market rates
* Fork [this spreadsheet](https://docs.google.com/spreadsheets/d/1nipH_G0lKQeaXItatDeAGju-4smkYeaIpUl3Pxnz3iU/edit) to model blended APY and revenue share
{% endstep %}

{% step %}
### Revenue‑split

What share goes to holders, RSR stakers, and other addresses?

* **DTF holders**: incentivize minting and holding as a savings vehicle
* **RSR stakers**: incentivize decentralized governance and overcollateralization
* **Other addresses**: fund marketing, liquidity, research, DAO management, etc.
{% endstep %}

{% step %}
### Branding

What is the name, ticker, and one-line marketing hook for your DTF?

* Use unique & memorable branding to convey the DTF's unique features
* Keep the ticker short & avoid overlap with other major assets
{% endstep %}

{% step %}
### Mandate

What will the DTF's mandate be?

* The mandate is a 256‑character compass for governance
* Specify the DTF's goals, constraints, and priorities
{% endstep %}
{% endstepper %}

If you’re unsure how to answer any or all of these questions, the Reserve community is welcoming and willing to help! [Jump in the Reserve Telegram channel here.](https://t.me/reservecurrency)

## Pre-launch checklist

Before deployment, make sure each of the following have been specified:

* ✅ Deployment network (Ethereum / Base / Arbitrum)
* ✅ Primary & backup collateral baskets
* ✅ Revenue split (DTF holders / RSR stakers / others)
* ✅ Name, ticker, & mandate
* ✅ DTF Parameters (use presets or tweak [advanced parameters](advanced-parameters.md))

Once each has been chosen, the Yield DTF is ready for launch.
