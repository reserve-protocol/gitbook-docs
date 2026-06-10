# Mint fee

### Mint fee explained <a href="#mint-fee-explained" id="mint-fee-explained"></a>

The mint fee is a straightforward percentage fee applied whenever a user mints new DTF tokens.

#### Technical implementation <a href="#technical-implementation" id="technical-implementation"></a>

The calculation is a simple percentage of the mint amount:

```
fee_amount = mint_amount * mint_fee_percentage
user_receives = mint_amount - fee_amount
```

#### Example <a href="#example" id="example"></a>

* User mints 100 $COIN50
* Mint fee = 1%
* User receives 99 $COIN50
* 1 $COIN50 is set aside for fee recipients
