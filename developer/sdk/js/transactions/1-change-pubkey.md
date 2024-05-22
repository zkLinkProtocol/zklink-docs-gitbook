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

### func getChangePubkeyMessage

```javascript
/**
* @param {number} chainId
* @param {string} address
* @returns {string}
*/
getChangePubkeyMessage(chainId, address)
```

Get the EIP-712 structured data of [ChangePubKey](#type-changepubkey)

### func getEthSignMsg

```javascript
/**
* @param {number} nonce
* @param {number} account_id
* @returns {string}
*/
getEthSignMsg(nonce, account_id)
```

Get the Ethereum sign message

### func setEthAuthData

```javascript
/**
* @param {string} sig
* @returns {any}
*/
setEthAuthData(String sig)
```

Set Ethereum authentication data with given EthECDSA signature

### func sign

```javascript
/**
* @param {ZkLinkSigner} signer
* @returns {any}
*/
sign(signer)
```

Sign transaction with given [ZkLinkSigner](../signer.md#type-zklinksigner)

### Example

```javascript
const new_pubkey_hash = "0x8255f5a6d0d2b34a19f381e448ed151cc3a59b9e";
const ts  = Math.floor(Date.now() / 1000);
let zklinkSigner = ZkLinkSigner.ethSig("0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001");
let tx_builder = new ChangePubKeyBuilder(
    16,21,0,new_pubkey_hash,140,"10",
    0,"0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001b",
    ts);
let tx = newChangePubkey(tx_builder);
tx.sign(zklinkSigner);
console.log("ETH Signed Message:\n" + tx.getEthSignMsg(100, 1));
tx.setEthAuthData("0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001b");
console.log(tx.jsValue())
```
