# Platform fee

### Platform fee <a href="#platform-fee" id="platform-fee"></a>

A platform fee is applied to both the TVL and Mint fees before they are distributed to recipients.

This platform fee is defaulted to 33%, but can be set per-DTF.

#### Technical implementation <a href="#technical-implementation" id="technical-implementation"></a>

The platform fee is supplied to DTFs via a Platform Fee Registry contract, which is controlled by the platform owner multisig. This registry also provides DTFs with the current platform fee recipient address.

### Fee parameters and constraints <a href="#fee-parameters-and-constraints" id="fee-parameters-and-constraints"></a>

The following parameters apply to all Index DTFs:

| Parameter                     | Value |
| ----------------------------- | ----- |
| Net platform TVL fee minimum  | 0.15% |
| Net platform mint fee minimum | 0.15% |
| Max admin-defined TVL fee     | 10%   |
| Max admin-defined mint fee    | 5%    |
