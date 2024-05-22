## type WithdrawBuilder

### constructor

```javascript
/**
* @param {number} account_id
* @param {number} sub_account_id
* @param {number} to_chain_id
* @param {string} to_address
* @param {number} l2_source_token
* @param {number} l1_target_token
* @param {string} amount
* @param {string | undefined} call_data
* @param {string} fee
* @param {number} nonce
* @param {boolean} withdraw_to_l1
* @param {number} withdraw_fee_ratio
* @param {number | undefined} [ts]
*/
WithdrawBuilder(account_id, sub_account_id, to_chain_id, to_address, l2_source_token, l1_target_token, amount, call_data, fee, nonce, withdraw_to_l1, withdraw_fee_ratio, ts)
```

## type Withdraw
[Withdraw](../../../api-and-sdk/data-types/transaction/withdraw.md) transaction type.

### constructor

```javascript
/**
* @param {WithdrawBuilder} builder
*/
newWithdraw(builder)
```

### func getEthSignMsg

```javascript
/**
* @param {string} token_symbol
* @returns {string}
*/
getEthSignMsg(token_symbol)
```

Get the Ethereum sign message

### func sign

```javascript
/**
* @param {ZkLinkSigner} signer
* @returns {any}
*/
sign(signer)
```

Sign transaction with given [ZkLinkSigner](../signer.md#type-zklinksigner)
