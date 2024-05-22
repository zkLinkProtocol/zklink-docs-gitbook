## type FundingBuilder

### constructor

```javascript
/**
* @param {number} account_id
* @param {number} sub_account_id
* @param {number} sub_account_nonce
* @param {Uint32Array} funding_account_ids
* @param {string} fee
* @param {number} fee_token
*/
FundingBuilder(account_id, sub_account_id, sub_account_nonce, funding_account_ids, fee, fee_token)
```

## type Funding
[Funding](../../../api-and-sdk/data-types/transaction/funding.md) transaction type.

### constructor

```javascript
/**
* @param {FundingBuilder} builder
*/
newFunding(builder)
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
