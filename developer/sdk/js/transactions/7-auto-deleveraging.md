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

## type AutoDeleveragingBuilder

### constructor

```javascript
/**
* @param {number} account_id
* @param {number} sub_account_id
* @param {number} sub_account_nonce
* @param {ContractPrice[]} contract_prices
* @param {SpotPriceInfo[]} margin_prices
* @param {number} adl_account_id
* @param {number} pair_id
* @param {string} adl_size
* @param {string} adl_price
* @param {string} fee
* @param {number} fee_token
*/
AutoDeleveragingBuilder(account_id, sub_account_id, sub_account_nonce, contract_prices, margin_prices, adl_account_id, pair_id, adl_size, adl_price, fee, fee_token)
```

## type AutoDeleveraging
[AutoDeleveraging](../../../api-and-sdk/data-types/transaction/auto\_deleveraging.md) transaction type.

```javascript
/**
* @param {AutoDeleveragingBuilder} builder
*/
newAutoDeleveraging(builder)
```
