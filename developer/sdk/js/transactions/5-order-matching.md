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

## type Order
The [Order](../../../api-and-sdk/data-types/transaction/order\_matching.md) struct of taker and maker.

### constructor

```javascript
/**
* @param {number} account_id
* @param {number} sub_account_id
* @param {number} slot_id
* @param {number} nonce
* @param {number} base_token_id
* @param {number} quote_token_id
* @param {string} amount
* @param {string} price
* @param {boolean} is_sell
* @param {number} maker_fee_rate
* @param {number} taker_fee_rate
* @param {boolean} has_subsidy
*/
Order(account_id, sub_account_id, slot_id, nonce, base_token_id, quote_token_id, amount, price, is_sell, maker_fee_rate, taker_fee_rate, has_subsidy)
```

## type OrderMatchingBuilder

### constructor

```javascript
/**
* @param {number} account_id
* @param {number} sub_account_id
* @param {Order} taker
* @param {Order} maker
* @param {string} fee
* @param {number} fee_token
* @param {ContractPrice[]} contract_prices
* @param {SpotPriceInfo[]} margin_prices
* @param {string} expect_base_amount
* @param {string} expect_quote_amount
*/
OrderMatchingBuilder(account_id, sub_account_id, taker, maker, fee, fee_token, contract_prices, margin_prices, expect_base_amount, expect_quote_amount)
```

## type OrderMatching
[OrderMatching](../../../api-and-sdk/data-types/transaction/order\_matching.md) transaction type.

### constructor

```javascript
/**
* @param {OrderMatchingBuilder} builder
*/
newOrderMatching(builder)
```
