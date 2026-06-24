# Roles

The [readme](https://github.com/reserve-protocol/reserve-index-dtf/blob/main/README.md) document in the Reserve Index Protocol GitHub [repository](https://github.com/reserve-protocol/reserve-index-dtf) provides an overview of the protocol's smart contract architecture and implementation details.

The material below provides a technical tour of the protocol's contracts, bridging the high-level system design with onchain implementations.

The [Protocol Operations](overview.md) chapter explains how these contracts work together to provide the core functionality of the protocol.

## System architecture

The table below groups the protocol's contracts into functional layers and highlights the role each set of contracts plays within the system.

| Layer                      | Core contracts                                                                 | Purpose                                                                                                                                                                      |
| -------------------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **DAO infrastructure**     | `FolioDAOFeeRegistry`, `FolioVersionRegistry`                                  | Collect protocol-wide fees and whitelist Folio implementations / deployers                                                                                                   |
| **Index (“Folio”) layer**  | `Folio` (ERC-20), `FolioProxy`                                                 | Represents the basket and embeds the rebalance-auction engine; proxy pattern allows controlled upgrades                                                                      |
| **Governance layer**       | <p><code>FolioGovernor</code></p><p><code>ReserveOptimisticGovernor</code></p> | Dual-path governance (standard & optimistic) that executes proposals and manages rebalances. The selector registry whitelists which actions can use the optimistic fast path |
| **Staking / voting layer** | `StakingVault`                                                                 | Escrows vote-locked governance tokens and streams rewards while providing voting weight for both standard and optimistic proposals                                           |

## Governance roles

Index DTFs employ a modular governance structure composed of scope-constrained onchain roles. Governance can be flexibly customized by assigning different actors or contracts (e.g., DAOs, multi-sigs, EOAs) to each role.

Each role is deliberately sandboxed to the smallest set of function calls and parameter ranges needed for its mandate, and every high-impact action must pass through a timelock or hard-coded ceiling. These guardrails guarantee that no single key can abuse the system—another role (or broader governance) always has time and authority to intervene.

{% stepper %}
{% step %}
### Admin

The Admin is the primary admin of the DTF. There should really only ever be 1 Admin.

#### Expected actor

A DAO that operates via the supplied `FolioGovernor.sol` / `TimelockController.sol` or `ReserveOptimisticGovernor.sol` / `TimelockControllerOptimistic.sol` contracts.

DAO power should be sourced from the `StakingVault.sol` contract. The main reason for this is that the `StakingVault` has the ability to collect multiple different reward tokens and distribute them to stakers pro-rata over time in an MEV-resistant manner. Any ERC20 can be configured as the deposit token for the `StakingVault` at deployment.

#### Abilities

* Add / remove assets to / from the basket
* Set fees & fee recipients
* Set auction length
* Set the auction delay (delay between when an auction is approved and when it can be permissionlessly launched)
* Assign all roles
* Manage the Optimistic Selector Registry (add or remove allowed optimistic actions)

#### Limitations

All actions are gated by a timelock (default 48 h). Cannot bypass hard-coded parameter ceilings (e.g., max 10% annual TVL fee) and cannot seize collateral directly. Changes to governance infrastructure (the Governor, Timelock, Selector Registry, and Staking Vault) must always go through the standard governance path.
{% endstep %}

{% step %}
### Rebalance Manager

The Rebalance Manager is responsible for configuring auctions for execution.

#### Expected actor

The same DAO that acts as the Admin should also take on the role of Rebalance Manager, and the default deployment configuration in Register is exactly this.

However, having the Rebalance Manager as a separate role gives the Admin the optionality to permission a smaller set of trusted actors to approve auctions. This would allow the DTF to move faster on basket changes, but comes with the added risk of the DTF being considered a security (due to the nature of the asset selection being managed by a small group of permissioned actors) and mint access being geo-blocked on Register.

#### Abilities

* Configure an auction for execution.

Each auction definition has the following parameters at approval:

* `Sell Token`: the token to be sold by the DTF
* `Buy Token`: the token to be bought by the DTF
* `Expected Volatility`: the amount of price movement that is expected in the `Sell Token`:`Buy Token` pairing. The Rebalance Manager can select between 3 options: Low, Medium, and High.
* `Time To Live (TTL)`: How long (in seconds) an auction can exist in an APPROVED state until it can no longer be opened. This value must be longer than the DTF-configured `Auction Delay` if the auction is intended to be permissionlessly available.

#### Limitations

May only define auctions within pre-set volatility and TTL ranges. Cannot alter fees, basket weights, or governance itself.
{% endstep %}

{% step %}
### Auction Launcher

The Auction Launcher is responsible for launching an approved auction and providing more accurate pricing information for it.

A DTF is NOT required to have any Auction Launchers, as auctions can be launched permissionlessly (as long as the DTF and auctions are configured correctly).

#### Expected actor

Because this role takes on a more ministerial job, it can be held by a trusted multisig or EOA.

While any Auction Launcher should be trusted, they can only modify the pricing information within the bounds set by the Rebalance Manager, thus limiting the amount of damage from mistakes or a rogue actor.

#### Abilities

* Launch auctions
* Kill auctions
* When opening an auction, optionally alter parameters of the auction within the approved ranges

#### Limitations

Can tweak start/end prices but only inside the bands approved by the Approver. Cannot extend an auction’s TTL or change the sell/buy tokens.
{% endstep %}

{% step %}
### Optimistic Proposer

The Optimistic Proposer is responsible for creating fast-path optimistic proposals for routine, pre-approved operations.

#### Expected actor

A trusted multisig or EOA that handles day-to-day operational proposals such as launching rebalance auctions or updating a DTF's display name.

#### Abilities

* Create optimistic proposals that skip affirmative voting and execute automatically after the veto period unless vetoed
* Only actions whitelisted in the Optimistic Selector Registry can be proposed optimistically

#### Limitations

Subject to a _proposal throttle_ limiting the number of optimistic proposals per account within a 24-hour window. Can only propose whitelisted (proposer, target, selector) combinations. Cannot propose changes to governance infrastructure (Governor, Timelock, Selector Registry, or Staking Vault). The role can be revoked at any time by a Guardian without a governance vote.
{% endstep %}

{% step %}
### Brand Manager

The Brand Manager is responsible for updating the Register UI with the correct social media links and token media (logo, banner).

#### Expected actor

A trusted multisig or EOA that is responsible for the branding and marketing of the DTF.

#### Abilities

* Update the DTF website, Twitter, Farcaster, and Telegram links that get displayed on Register
* Update the DTF logo and banner that get displayed on Register

#### Limitations

Limited to UI metadata (links, logos). Has zero permissions over assets, fees, or auctions.
{% endstep %}

{% step %}
### Guardian

The Guardian is responsible for vetoing malicious governance proposals and providing emergency safeguards against compromised optimistic proposers.

#### Expected actor

A trusted multisig or EOA that is ready and willing to act quickly to prevent malicious governance proposals from being executed or to revoke compromised optimistic proposers.

#### Abilities

* Veto governance proposals (both standard and optimistic)
* Revoke the Optimistic Proposer role from any address at any time without a governance vote

#### Limitations

Single-purpose veto (cancel) on queued governance proposals; cannot propose or execute anything itself. Power sunsets automatically if a quorum of stakers votes to revoke the role.
{% endstep %}
{% endstepper %}
