# State Update

Updates are generated after zkLinkTx execution, details can be found in [StateUpdateResp](./json-rpc/json-rpc-api.md#txresp). Parsing the updates can get some data that is only known after the transaction is executed, such as the actual withdraw amount of FullExit. There are 4 types of Update:

1. `AccountCreate`: A new account created on the state tree;
2. `AccountChangePubkeyUpdate`: The `pubkey` and `nonce` changed;
3. `BalanceUpdate`: The assets of account and `nonce` changed;
4. `OrderUpdate`: The order slot of the account has changed.

{% tabs %}
{% tab title="AccountCreate" %}

| Name      | Type                                 | Description         |
|-----------|--------------------------------------|---------------------|
| type      | String                               | Update Name         |
| updateId  | i32                                  | Update id           |
| accountId | [AccountId](data_types.md#AccountId) | The new accout id   |
| address   | String                               | The account address |

The transactions that create `AccountCreate`:

* [Deposit](./transaction/deposit.md)
* [Transfer](./transaction/transfer.md)

For Example:

```json
{
  "type": "AccountCreate",
   "updateId": 40,
   "accountId": 42,
   "address": "0xe4efc3d7b69a19d3ae574cbc2915ddf598efe43f"
}
```

{% endtab %}

{% tab title="AccountChangePubkeyUpdate" %}

| Name      | Type                                   | Description        |
|-----------|----------------------------------------|--------------------|
| type          | String                                 | Update name        |
| updateId      | i32                                    | Update Id          |
| accountId     | [AccountId](data_types.md#AccountId)   | Account id         |
| oldPubkeyHash | [PubkeyHash](data_types.md#PubkeyHash) | The old pubkeyHash |
| newPubkeyHash | [PubkeyHash](data_types.md#PubkeyHash) | The new pubkeyHash |
| oldNonce      | [Nonce](data_types.md#Nonce)           | The old nonce      |
| newNonce      | [Nonce](data_types.md#Nonce)           | The new nonce      |

The transactions that create `AccountChangePubkeyUpdate`:

* [ChangePubKey](transaction#changepubkey)

For example:

```json
{
  "type": "AccountChangePubkeyUpdate",
  "updateId": 10,
  "accountId": 39,
  "oldPubkeyHash": "0x0000000000000000000000000000000000000000",
  "newPubkeyHash": "0xbfb4f4a68dc9e49f7785082a8c12354ed663b6e0",
  "oldNonce": 0,
  "newNonce": 1
}
```
  
{% endtab %}

{% tab title="BalanceUpdate" %}

| Name         | Type                                       | Description                   |
|--------------|--------------------------------------------|-------------------------------|
| type         | String                                     | The update name               |
| updateId     | i32                                        | The update id                 |
| accountId    | [AccountId](data_tpes.md#AccountId)        | The account id                |
| subAccountId | [SubAccountId](data_types.md#SubAccountId) | The subaccount id             |
| coinId       | [TokenId](data_types.md#TokenId)           | The token id                  |
| oldBalance   | String                                     | The old balance of subaccount |
| newBalance   | String                                     | The new balance of subaccount |
| oldNonce     | [Nonce](data_types.md#Nonce)               | The old nonce of subaccount   |
| newNonce     | [Nonce](data_types.md#Nonce)               | The new nonce of subaccount   |

Transaction that generate `BalanceUpdate`:

* [ChangePubKey](./transaction/change_pubkey.md)
* [Deposit](./transaction/deposit.md)
* [ForcedExit](./transaction/forced_exit.md)
* [FullExit](./transaction/full_exit.md)
* [Transfer](./transaction/transfer.md)
* [Withdraw](./transaction/withdraw.md)
* [OrderMatching](./transaction/order_matching.md)

For example

```json
{
  "type": "BalanceUpdate",
  "updateId": 1,
  "accountId": 2,
  "subAccountId": 1,
  "coinId": 18,
  "oldBalance": "66517000000000000",
  "newBalance": "66518000000000000",
  "oldNonce": 66518,
  "newNonce": 66519
}
```

{% endtab %}

{% tab title="OrderUpdate" %}

| Name         | Type                                       | Description     |
|--------------|--------------------------------------------|-----------------|
| type         | String                                     | Update name     |
| updateId     | i32                                        | Update id       |
| accountId    | [AccountId](basic_type.md#AccountId)       | Account id      |
| subAccountId | [SubAccountId](data_types.md#SubAccountId) | Subaccount id   |
| slotId       | [SlotId](data_types.md#SlotId)             | Slot id         |
| oldTidyOrder | [ResponseTidyOrder](#ResponseTidyOrder)    | Old`TidyOrder`  |
| newTidyOrder | [ResponseTidyOrder](#ResponseTidyOrder)    | New `TidyOrder` |

where ResponseTidyOrder is

| Name         | Type                                       | Description                                                  |
|--------------|--------------------------------------------|--------------------------------------------------------------|
| nonce   | [Nonce](#Nonce) | The slot nonce of order                                      |
| residue | String          | The string format of BigDecimal, the residue balance of slot |

Transaction that generate `OrderUpdate`:

* [OrderMatching](./transaction/order_matching.md)

For Example:

```json
{
  "type": "OrderUpdate",
  "updateId": 30,
  "accountId": 11,
  "subAccountId": 1,
  "slotId": 27,
  "oldTidyOrder": {
    "nonce": 14,
    "residue": "4607200000000000000000"
  },
  "newTidyOrder": {
    "nonce": 14,
    "residue": "4233800000000000000000"
  }
}
```

{% endtab %}

{% endtabs %}

## Deposit

Updates generated by Deposit:

```
[AccountCreate(to)], BalanceUpdate(to), BalanceUpdate(global_asset)
```

> \[`update`]: this update may occur, `update(account)`: the update related to a certain account. The account id of `global_asset` is 1, and the account id of `fee` is 0.

When the `to` account does not exist, a new account will be automatically created by `AccountCreate`.

### Example

```json
"updates": [
  // As the to_account (accountId is 4) does not exist, an AccountCreate is involved
  {
      "type": "AccountCreate",
      "updateId": 15,
      "accountId": 4,
      "address": "0xb92a8ba62ff1d141798c7133cccefb33d9073323"
  },
  // The balance of the to_account increased, depositAmount=newBalance-oldBalance
  {
      "type": "BalanceUpdate",
      "updateId": 16,
      "accountId": 4,
      "subAccountId": 0,
      "coinId": 18,
      "oldBalance": "0",
      "newBalance": "10000000000000000000",
      "oldNonce": 0,
      "newNonce": 0
  },
  // The 'global asset' account records the increase of on-chain asset reserves
  {
      "type": "BalanceUpdate",
      "updateId": 17,
      "accountId": 1,
      "subAccountId": 2,
      "coinId": 18,
      "oldBalance": "3030000000000000000000",
      "newBalance": "3040000000000000000000",
      "oldNonce": 0,
      "newNonce": 0
  }
]
```

## FullExit

Updates generated by FullExit:

```
BalanceUpdate(exit), BalanceUpdate(global_asset)
```

### Example

```json
"updates": [
  // The balance of the exit_account decreased, exitAmount=oldBalance-newBalance
  {
      "type": "BalanceUpdate",
      "updateId": 0,
      "accountId": 28,
      "subAccountId": 1,
      "coinId": 44,
      "oldBalance": "1080000000000000000",
      "newBalance": "0",
      "oldNonce": 0,
      "newNonce": 0
  },
  // The 'global asset' account records the decrease of on-chain asset reserves
  {
      "type": "BalanceUpdate",
      "updateId": 1,
      "accountId": 1,
      "subAccountId": 1,
      "coinId": 44,
      "oldBalance": "1497300000000000000000",
      "newBalance": "1496220000000000000000",
      "oldNonce": 0,
      "newNonce": 0
  }
]
```

## ChangePubKey

Updates generated by ChangePubKey:

```
AccountChangePubkeyUpdate(target), BalanceUpdate(target), BalanceUpdate(fee)
```

### Example

```json
"updates": [
  // The pubkey of target account was successfully set
 
{
    "type": "AccountChangePubkeyUpdate",
    "updateId": 8,
    "accountId": 2,
    "oldPubkeyHash": "0x0000000000000000000000000000000000000000",
    "newPubkeyHash": "0x870b67b523f93dad7a313f6b64b9608dedab3874",
    "oldNonce": 0,
    "newNonce": 1
  },
  // The balance of the target account decreased
  {
    "type": "BalanceUpdate",
    "updateId": 9,
    "accountId": 2,
    "subAccountId": 0,
    "coinId": 18,
    "oldBalance": "10000000000000000000",
    "newBalance": "9000000000000000000",
    "oldNonce": 1,
    "newNonce": 1
  },
  // The balance of the fee account increased
  {
    "type": "BalanceUpdate",
    "updateId": 10,
    "accountId": 0,
    "subAccountId": 0,
    "coinId": 18,
    "oldBalance": "0",
    "newBalance": "1000000000000000000",
    "oldNonce": 0,
    "newNonce": 0
  }
]
```

## Transfer

Updates generated by Transfer:

```
[AccountCreate(to)], BalanceUpdate(from), BalanceUpdate(to), BalanceUpdate(fee)
```

Similar to [Deposit](state-update.md#deposit), when the `to` account does not exist, a new account will be automatically created by `AccountCreate`.

### Example

```json
"updates": [
  // The balance of the from_account decreased
  {
    "type": "BalanceUpdate",
    "updateId": 12,
    "accountId": 2,
    "subAccountId": 0,
    "coinId": 18,
    "oldBalance": "3018976060000000000000",
    "newBalance": "3018975850000000000000",
    "oldNonce": 115,
    "newNonce": 116
  },
  // The balance of the to_account increased. Noted that the to_accountId is the same as the from_accountId, but the subAccountId is different
  // It means that this transaction is a token transfer between separate sub-accounts of the same account
  {
    "type": "BalanceUpdate",
    "updateId": 13,
    "accountId": 2,
    "subAccountId": 1,
    "coinId": 18,
    "oldBalance": "114000000000000",
    "newBalance": "115000000000000",
    "oldNonce": 115,
    "newNonce": 116
  },
  // The balance of the fee account increased
  {
    "type": "BalanceUpdate",
    "updateId": 14,
    "accountId": 0,
    "subAccountId": 0,
    "coinId": 18,
    "oldBalance": "1023826000000000000",
    "newBalance": "1024035000000000000",
    "oldNonce": 0,
    "newNonce": 0
  }
]
```

## Withdraw

Updates generated by Withdraw:

```
BalanceUpdate(from), BalanceUpdate(global_asset), BalanceUpdate(fee)
```

### Example

```json
"updates": [
  // The from_account balance decreased due to the withdrawal
  {
    "type": "BalanceUpdate",
   

"updateId": 3,
    "accountId": 3,
    "subAccountId": 0,
    "coinId": 44,
    "oldBalance": "2993839561700000000000",
    "newBalance": "2992538030700000000000",
    "oldNonce": 81,
    "newNonce": 82
  },
  // The global asset account records the decrease in on-chain asset reserves
  {
    "type": "BalanceUpdate",
    "updateId": 4,
    "accountId": 1,
    "subAccountId": 1,
    "coinId": 44,
    "oldBalance": "1500000000000000000000",
    "newBalance": "1498700000000000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  // The balance of the fee account increased
  {
    "type": "BalanceUpdate",
    "updateId": 5,
    "accountId": 0,
    "subAccountId": 0,
    "coinId": 44,
    "oldBalance": "438300000000000",
    "newBalance": "1969300000000000",
    "oldNonce": 0,
    "newNonce": 0
  }
]
```

## ForcedExit

Updates generated by ForcedExit:

```
BalanceUpdate(init), BalanceUpdate(target), BalanceUpdate(global_asset), BalanceUpdate(fee)
```

### Example

```json
"updates": [
  // The balance of init account decreased due to the payment of transaction fees
  {
    "type": "BalanceUpdate",
    "updateId": 20,
    "accountId": 3,
    "subAccountId": 0,
    "coinId": 18,
    "oldBalance": "2790050120799999999999",
    "newBalance": "2790031090799999999999",
    "oldNonce": 178,
    "newNonce": 179
  },
  // The amount of the withdrawal to the target account on Layer1: exitAmount=oldBalance-newBalance
  {
    "type": "BalanceUpdate",
    "updateId": 21,
    "accountId": 15,
    "subAccountId": 1,
    "coinId": 40,
    "oldBalance": "2100000000000000000",
    "newBalance": "0",
    "oldNonce": 0,
    "newNonce": 0
  },
  // The global asset account records the decrease in on-chain asset reserves
  {
    "type": "BalanceUpdate",
    "updateId": 22,
    "accountId": 1,
    "subAccountId": 2,
    "coinId": 40,
    "oldBalance": "4000001486590000000000000000",
    "newBalance": "4000001484490000000000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  //The balance of the fee account increased
  {
    "type": "BalanceUpdate",
    "updateId": 23,
    "accountId": 0,
    "subAccountId": 0,
    "coinId": 18,
    "oldBalance": "7272586000000000000",
    "newBalance": "7291616000000000000",
    "oldNonce": 0,
    "newNonce": 0
  }
```

### OrderMatching

Updates generated by OrderMatching:

```
OrderUpdate(maker), BalanceUpdate(maker), BalanceUpdate(maker),
OrderUpdate(taker), BalanceUpdate(taker), BalanceUpdate(taker),
BalanceUpdate(submitter), BalanceUpdate(submitter), BalanceUpdate(submitter),
BalanceUpdate(fee)
```

### Example

```json
"updates": [
  // Orderslot of the maker
  {
    "type": "OrderUpdate",
    "updateId": 0,
    "accountId": 13,
    "subAccountId": 1,
    "slotId": 898,
    "oldTidyOrder": {
      "nonce": 1,
      "residue": "54800000000000000"
    },
    "newTidyOrder": {
      "nonce": 2,
      "residue": "0"
    }
  },
  // The balance of token0 in the maker account decreased
  {
    "type": "BalanceUpdate",
    "updateId": 1,
    "accountId": 13,
    "subAccountId": 1,
    "coinId": 1,
    "oldBalance": "200304228482442156994500000",
    "newBalance": "200304142493022156994500000",
    "oldNonce": 1,
    "newNonce": 1
  },
  // The balance of token1 in the maker account increased
  {
    "type": "BalanceUpdate",
    "updateId": 2,
    "accountId": 13,
    "subAccountId": 1,
    "coinId": 41,
    "oldBalance": "199999971788889400000000000",
    "newBalance": "199999971843662000000000000",
    "oldNonce": 1,
    "newNonce": 1
  },
  // Orderslot of the taker
  {
    "type": "OrderUpdate",
    "updateId": 3,
    "accountId": 13,
    "subAccountId": 1,
    "slotId": 163,
    "oldTidyOrder": {
      "nonce": 0,
      "residue": "1467400000000000000"
    },
    "newTidyOrder": {
      "nonce": 0,
      "residue": "1412600000000000000"
    }
  },
  // The balance of token1 in the taker account decreased
  {
    "type": "BalanceUpdate",
    "updateId": 4,
    "accountId": 13,
    "subAccountId": 1,
    "coinId": 41,
    "oldBalance": "199999971843662000000000000",
    "newBalance": "199999971788862000000000000",
    "oldNonce": 1,
    "newNonce": 1
  },
  // The balance of token0 in the taker account increased
  {
    "type": "BalanceUpdate",
    "updateId": 5,
    "accountId": 13,
    "subAccountId": 1,
    "coinId": 1,
    "oldBalance": "200304142493022156994500000",
    "newBalance": "200304228396452736994500000",
    "oldNonce": 1,
    "newNonce": 1
  },
  // The submitter balance of token0 decreased due to transaction fees
  {
    "type": "BalanceUpdate",
    "updateId": 6,
    "accountId": 6,
    "subAccountId": 1,
    "coinId": 1,
    "oldBalance": "9999680162000000000000",
    "newBalance": "9999679771000000000000",
    "oldNonce": 224,
    "newNonce": 224
  },
  // The submitter collect token0 as transaction fee
  {
    "type": "BalanceUpdate",
    "updateId": 7,
    "accountId": 6,
    "subAccountId": 0,
    "coinId": 1,
    "oldBalance": "3529001244641983488661500000",
    "newBalance": "3529001244727972908661500000",
    "oldNonce": 224,
    "newNonce": 224
  },
  // The submitter collect token1 as transaction fee
  {
    "type": "BalanceUpdate",
    "updateId": 8,
    "accountId": 6,
    "subAccountId": 0,
    "coinId": 41,
    "oldBalance": "1594000000063937300000000000",
    "newBalance": "1594000000063964700000000000",
    "oldNonce": 224,
    "newNonce": 224
  },
  // The balance of the fee account increased
  {
    "type": "BalanceUpdate",
    "updateId": 9,
    "accountId": 0,
    "subAccountId": 0,
    "coinId": 1,
    "oldBalance": "483683000000000000",
    "newBalance": "484074000000000000",
    "oldNonce": 0,
    "newNonce": 0
  }
]
```



`version: 4457a91`

