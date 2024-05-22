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

## type ContractBuilder

### constructor

```javascript
/**
* @param {number} account_id
* @param {number} sub_account_id
* @param {number} slot_id
* @param {number} nonce
* @param {number} pair_id
* @param {string} size
* @param {string} price
* @param {boolean} direction
* @param {number} maker_fee_rate
* @param {number} taker_fee_rate
* @param {boolean} has_subsidy
*/
ContractBuilder(account_id, sub_account_id, slot_id, nonce, pair_id, size, price, direction, maker_fee_rate, taker_fee_rate, has_subsidy)
```

## type Contract
The [Contract](../../../api-and-sdk/data-types/transaction/contract\_matching.md) struct of taker and maker in perpetual contract.

### constructor

```javascript
/**
* @param {ContractBuilder} builder
*/
newContract(builder)
```

## type ContractMatchingBuilder

### constructor

```javascript
/**
* @param {number} account_id
* @param {number} sub_account_id
* @param {Contract} taker
* @param {Contract[]} maker
* @param {string} fee
* @param {number} fee_token
* @param {ContractPrice[]} contract_prices
* @param {SpotPriceInfo[]} margin_prices
*/
ContractMatchingBuilder(account_id, sub_account_id, taker, maker, fee, fee_token, contract_prices, margin_prices)
```

## type ContractMatching
[ContractMatching](../../../api-and-sdk/data-types/transaction/contract\_matching.md) transaction type.

### constructor

```javascript
/**
* @param {ContractMatchingBuilder} builder
*/
newContractMatching(builder)
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
