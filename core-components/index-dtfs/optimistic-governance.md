# Optimistic Governance

Reserve's optimistic governance lets pre-approved, routine actions on a DTF execute quickly — without holding a full multi-day vote — while giving token holders a direct way to block anything they disagree with.

If enough holders object during a short veto window, the action does not just get cancelled. It automatically escalates into a regular community vote.&#x20;

### Two paths through one system

A DTF using optimistic governance has two ways for proposals to move forward:

| Path                  | What it's for                                                    | What holders do                                          |
| --------------------- | ---------------------------------------------------------------- | -------------------------------------------------------- |
| **Fast (optimistic)** | Routine, pre-approved actions                                    | Watch for proposals; vote against any they disagree with |
| **Slow (standard)**   | Anything else — including changes to how governance itself works | Vote For, Against, or Abstain through a normal proposal  |

Both paths share the same token, the same delays, and the same execution pipeline. The only difference is whether the action was pre-approved as routine enough to run on the fast path.

{% hint style="info" %}
Anything that changes the governance system itself — the rules of what's allowed on the fast path, the contracts that run governance, or the token — can only happen through the slow path. Fast governance can never modify itself.
{% endhint %}

### Why have a fast path at all?

Most governance actions on a live DTF are small and predictable: adjusting a single parameter, rotating an oracle, scheduling a rebalance window. Running each of these through a full multi-day vote with quorum adds friction without adding meaningful security — the action is already pre-scoped and the operators are already known.

The fast path removes that friction for routine work, and the veto window keeps holders in control. If something unexpected ever appears on the fast path, holders can stop it.

### How the fast path works

{% stepper %}
{% step %}
#### A proposer submits a fast proposal

Only addresses with permission to use the fast path can submit. Each submission is checked against an allowlist of pre-approved actions — if the proposal tries to do anything not on that list, it's rejected immediately.
{% endstep %}

{% step %}
#### A short delay, then the veto window opens

After a brief delay, the proposal enters its veto window. This is the time during which token holders can object.
{% endstep %}

{% step %}
#### Holders can vote against

During the veto window, any holder can vote against the proposal. There are no For votes on the fast path — holders only weigh in if they want to stop the action. Silence is consent.
{% endstep %}

{% step %}
#### One of two things happens:

**If the veto window closes an too few Against votes came in**, the proposal passes and executes right away.

**If Against votes cross the veto threshold at any point during the veto window**, the proposal is blocked and a full community vote is automatically opened in its place.
{% endstep %}

{% step %}
#### Escalation: the confirmation vote

The escalated vote runs as a normal slow proposal: For, Against, or Abstain. Votes from the veto window do not carry over — voting starts fresh.

If the community vote passes, the action still happens, just with full approval and the normal timelock delay. If it fails, the action is permanently blocked.
{% endstep %}
{% endstepper %}

### What holders need to do

#### Stake to gain voting power

Voting power on both paths comes from staking the DTF's governance token. Staked tokens are subject to an unstaking delay when you withdraw — this prevents flash-loaning votes in or out around a proposal.

#### Delegate (twice, if you want)

Staked tokens carry two separate delegations:

* **Standard delegation** chooses who votes your weight on slow proposals.
* **Veto delegation** chooses who can use your weight to veto fast proposals.

You can point these at the same address or at two different ones. A common pattern: delegate standard governance to a long-term steward, and delegate veto authority to someone focused on monitoring fast proposals as they happen.

If you stake and never delegate, your tokens don't vote on either path. Most users delegate to themselves when they stake.

#### Watch the veto window

When a fast proposal is active, the only thing you (or your veto delegate) needs to decide is whether to vote against it. There is no quorum to hit and no For vote to cast — your only job is to flag anything that looks wrong.

If something does look wrong, voting against it costs nothing and contributes to the veto threshold. If enough holders feel the same way, the proposal escalates to a full vote.

#### Participate in slow proposals normally

Slow proposals — including any confirmation vote that comes out of a vetoed fast proposal — work like standard governance. You vote For, Against, or Abstain, and the proposal needs both quorum and majority support to pass.

### What protects the fast path

The fast path is fast because it's narrow. Four independent safeguards keep it that way:

**A pre-approved action list.** Fast proposals can only call actions that have been explicitly added to an allowlist by slow governance. New actions cannot be added through the fast path itself.

**Untouchable targets.** The governance contracts themselves — the token, the timelock, the governor, the allowlist — can never be called by a fast proposal, even if someone tries to add them. Any change to the system has to go through slow governance.

**A rate limit on proposers.** Each fast-path proposer can only submit a limited number of proposals per 12 hour period. This caps the rate at which a compromised proposer key could flood the system, giving holders time to respond and giving the broader community time to revoke the key.

**A shared guardian.** A guardian role can cancel any non-final fast proposal mid-flight. This is typically held by a monitoring service that watches for clearly malicious or broken proposals, so holders don't have to be the first line of defense in an emergency.

**The veto itself.** Even if every other safeguard fails, the veto window is the final backstop. The threshold scales with the total supply of staked tokens, so it cannot be diluted by issuing more tokens.

### Possible outcomes for a fast proposal

| What happens                      | Result                                                       |
| --------------------------------- | ------------------------------------------------------------ |
| Veto threshold never reached      | Proposal executes right after the veto window                |
| Veto threshold reached            | Proposal is blocked; a confirmation vote opens automatically |
| Confirmation vote passes          | Action executes through the normal slow path                 |
| Confirmation vote fails           | Action is permanently blocked                                |
| Cancelled by proposer or guardian | Proposal is dropped before completing                        |

### Tracking what kind of proposal you're looking at

Every proposal — fast or slow — has a unique ID. Whether a given proposal is on the fast or slow path is fixed and does not change.

When a fast proposal is vetoed and escalates, the resulting confirmation vote is a **new** proposal with its own ID. The original fast proposal stays on record as defeated; the confirmation vote runs as a standard slow proposal from that point on.

### When does the fast path actually get used?

The fast path is intended for actions that are routine, reversible, or time-sensitive — for example:

* Routine parameter tuning
* Scheduling or pausing a rebalance window
* Rotating an oracle to a vetted alternative

It is **not** intended for, and cannot be used for, structural changes:

* Changing what's on the pre-approved action list
* Upgrading the governance contracts
* Changing the token or the timelock
* Adding or removing voting power directly

Anything in the second category requires a full community vote.
