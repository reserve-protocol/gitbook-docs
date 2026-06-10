# Platform Fee

### Platform Fee <a href="#platform-fee" id="platform-fee"></a>

A platform fee is applied to both the TVL and Mint fees before they are distributed to recipients.

This platform fee is defaulted to 50%, but can be set per-DTF, and is recalculated and manually adjusted on a monthly basis, based on the following progressive fee schedule table.

| **TVL**                | **Platform Fee** |
| ---------------------- | ---------------- |
| TVL < $100m            | 50%              |
| 100m\<TVL<100m\<TVL<1b | 40%              |
| 1b\<TVL<1b\<TVL<10b    | 30%              |
| 10b\<TVL<10b\<TVL<100b | 20%              |
| 100b\<TVL<100b\<TVL<1T | 10%              |
| $1T < TVL              | 5%               |

#### Example Calculation <a href="#example-calculation" id="example-calculation"></a>

A DTF with $3.1 billion in TVL and a TVL fee of 1% would have its platform fee calculated as:

```
(0.5 * 0.01 * $100 million + 0.4 * 0.01 * $900 million + 0.3 * 0.01 * $2.1 billion) / (0.01 * $3.1 billion) ≈ 33.5%
```

This means 33.5% of the collected fees would go to the platform, and the remaining 66.5% would be distributed among fee recipients.

#### Technical Implementation <a href="#technical-implementation" id="technical-implementation"></a>

The platform fee is supplied to DTFs via a Platform Fee Registry contract, which is controlled by the platform owner multisig. This registry also provides DTFs with the current platform fee recipient address.

### Fee Parameters and Constraints <a href="#fee-parameters-and-constraints" id="fee-parameters-and-constraints"></a>

The following parameters apply to all Index DTFs:

| Parameter                     | Value |
| ----------------------------- | ----- |
| Net platform TVL fee minimum  | 0.15% |
| Net platform mint fee minimum | 0.15% |
| Max admin-defined TVL fee     | 10%   |
| Max admin-defined mint fee    | 5%    |
