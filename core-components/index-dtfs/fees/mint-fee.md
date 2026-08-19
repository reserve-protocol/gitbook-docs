# Mint fee

### Mint fee explained <a href="#mint-fee-explained" id="mint-fee-explained"></a>

The mint fee is a straightforward percentage fee applied whenever a user mints new DTF tokens.

#### Simplified calculation <a href="#simplified-calculation" id="simplified-calculation"></a>

The calculation is a simple percentage of the mint amount:

```
fee_amount = mint_amount * mint_fee_percentage
user_receives = mint_amount - fee_amount
```

This is the user-facing economic model. Contract integrations must also match the Folio's upward integer rounding. See the [native mint integration guide](../native-mint-and-redeem-integration.md#2-calculate-net-shares-and-minsharesout) for the calculation.

#### Example <a href="#example" id="example"></a>

* User mints 100 $COIN50
* Mint fee = 1%
* User receives 99 $COIN50
* 1 $COIN50 is set aside for fee recipients
