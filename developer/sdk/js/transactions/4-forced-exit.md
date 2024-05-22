## type ForcedExitBuilder

### constructor

```javascript
/**
* @param {number} to_chain_id
* @param {number} initiator_account_id
* @param {number} initiator_sub_account_id
* @param {number} target_sub_account_id
* @param {string} target
* @param {number} l2_source_token
* @param {number} l1_target_token
* @param {string} exit_amount
* @param {number} initiator_nonce
* @param {boolean} withdraw_to_l1
* @param {number | undefined} [ts]
*/
ForcedExitBuilder(to_chain_id, initiator_account_id, initiator_sub_account_id, target_sub_account_id, target, l2_source_token, l1_target_token, exit_amount, initiator_nonce, withdraw_to_l1, ts)
```

## type ForcedExit
[ForcedExit](../../../api-and-sdk/data-types/transaction/forced\_exit.md) transaction type.

### constructor

```javascript
/**
* @param {ForcedExitBuilder} builder
*/
newForcedExit(builder)
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
