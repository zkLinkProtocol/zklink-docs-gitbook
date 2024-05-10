## type ChangePubKeyBuilder

### constructor

```javascript
/**
* @param {number} chain_id
* @param {number} account_id
* @param {number} sub_account_id
* @param {string} new_pubkey_hash
* @param {number} fee_token
* @param {string} fee
* @param {number} nonce
* @param {string | undefined} [eth_signature]
* @param {number | undefined} [ts]
*/
ChangePubKeyBuilder(chain_id, account_id, sub_account_id, new_pubkey_hash, fee_token, fee, nonce, eth_signature, ts)
```

## type ChangePubKey
[ChangePubkey](../../../api-and-sdk/data-types/transaction/change\_pubkey.md) transaction type.

### constructor

```javascript
/**
* @param {ChangePubKeyBuilder} builder
*/
newChangePubkey(builder)
```

### func get_change_pubkey_message

```javascript
/**
* @param {number} chainId
* @param {string} address
* @returns {string}
*/
get_change_pubkey_message(chainId, address)
```

Get the EIP-712 structured data of [ChangePubKey](#type-changepubkey)
