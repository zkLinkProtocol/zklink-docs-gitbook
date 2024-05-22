## type ContractPrice

### constructor

```javascript
/**
* @param {number} pair_id
* @param {string} market_price
*/
ContractPrice(pair_id, market_price)
```

**input:**
* pair_id: The contract pair id defined by zkLink.
* market_price: The market price of the contract pair

## type SpotPriceInfo

### constructor

```javascript
/**
* @param {number} token_id
* @param {string} price
*/
SpotPriceInfo(token_id, price)
```

**input:**
* token_id: The token id defined by zkLink.
* price: The spot price of the token.

## type LiquidationBuilder

### constructor

```javascript
/**
* @param {number} account_id
* @param {number} sub_account_id
* @param {number} sub_account_nonce
* @param {ContractPrice[]} contract_prices
* @param {SpotPriceInfo[]} margin_prices
* @param {number} liquidation_account_id
* @param {string} fee
* @param {number} fee_token
*/
LiquidationBuilder(account_id, sub_account_id, sub_account_nonce, contract_prices, margin_prices, liquidation_account_id, fee, fee_token) {
```

## type Liquidation
[Liquidation](../../../api-and-sdk/data-types/transaction/liquidation.md) transaction type.

### constructor

```javascript
/**
* @param {LiquidationBuilder} builder
*/
newLiquidation(builder)
```

### func sign

```javascript
/**
* @param {ZkLinkSigner} signer
* @returns {any}
*/
sign(signer)
```

Sign transaction with given [ZkLinkSigner](../signer.md#type-zklinksigner)