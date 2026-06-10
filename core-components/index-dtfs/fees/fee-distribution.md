# Fee distribution

### Fee distribution <a href="#fee-distribution" id="fee-distribution"></a>

After platform fees are deducted, the remaining fees are distributed among the fee recipients according to their Admin-defined weights.

### Practical implications for users <a href="#practical-implications-for-users" id="practical-implications-for-users"></a>

#### For governance stakers <a href="#for-governance-stakers" id="for-governance-stakers"></a>

* Fee distribution to governance token holders follows an exponential distribution model over time to mitigate MEV (Miner Extractable Value) attacks
* Governance token holders can claim their pro-rata share of fees based on their staked tokens

#### For traders and holders <a href="#for-traders-and-holders" id="for-traders-and-holders"></a>

* When holding DTF tokens, be aware that the TVL fee continuously accrues, effectively acting as a management fee
* When minting new DTF tokens, account for the one-time mint fee in your calculations

#### For integrators <a href="#for-integrators" id="for-integrators"></a>

* Implement accurate fee calculations in your interfaces to provide users with precise token valuations
* Account for the continuous nature of the TVL fee when displaying token values
* Ensure your integration accounts for both fees when estimating transaction outcomes

### Governance implications <a href="#governance-implications" id="governance-implications"></a>

Fee parameters can be adjusted through governance, allowing the protocol to adapt to changing market conditions. This includes:

* TVL fee rate adjustments
* Mint fee rate adjustments
* Fee recipient configurations
