# TVL fee

### TVL fee explained <a href="#tvl-fee-explained" id="tvl-fee-explained"></a>

The TVL fee is expressed as a percentage of the DTF's Total Value Locked and is applied continuously through a compound interest calculation.

#### Technical implementation <a href="#technical-implementation" id="technical-implementation"></a>

The TVL fee is implemented using a compound interest formula:

```
fee = (1 / (1 - feePerSecond))^secondsPassed - 1
```

Where:

* `feePerSecond` is set by the DTF Owner (governance)
* `secondsPassed` represents the time since the last fee calculation

This mechanism ensures that:

* The fee is calculated and accounted for on every user interaction
* Any view functions account for the TVL fee since the last contract update
* The fee accumulates continuously rather than at discrete intervals

#### Practical implications <a href="#practical-implications" id="practical-implications"></a>

For users and integrators, this means:

* The TVL fee effectively acts as a continuous management fee
* The longer assets remain in the DTF, the more fees accrue
* The displayed value of Index DTF tokens will gradually decrease relative to the underlying assets, reflecting the accrued fees
