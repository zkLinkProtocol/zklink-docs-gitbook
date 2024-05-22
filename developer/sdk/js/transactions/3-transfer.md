## type TransferBuilder

### constructor

```javascript
/**
* @param {number} account_id
* @param {string} to_address
* @param {number} from_sub_account_id
* @param {number} to_sub_account_id
* @param {number} token
* @param {string} fee
* @param {string} amount
* @param {number} nonce
* @param {number | undefined} [ts]
*/
TransferBuilder(account_id, to_address, from_sub_account_id, to_sub_account_id, token, fee, amount, nonce, ts)
```

## type Transfer
[Transfer](../../../api-and-sdk/data-types/transaction/transfer.md) transaction type.

### constructor

```javascript
/**
* @param {TransferBuilder} builder
*/
newTransfer(builder)
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
