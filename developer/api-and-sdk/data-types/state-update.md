# State Update

Updates are generated after zkLinkTx execution, details can be found in [StateUpdateResp](../json-rpc/json-rpc-api.md#txresp).  
Parsing the updates can get some data that is only known after the transaction is executed, such as the actual withdraw amount of FullExit

## AccountUpdate. 
There are 4 types of AccountUpdate:

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
| accountId | [AccountId](basic-types.md#AccountId) | The new accout id   |
| address   | String                               | The account address |

Transaction that generate `AccountCreate`:

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
| accountId     | [AccountId](basic-types.md#AccountId)   | Account id         |
| oldPubkeyHash | [PubkeyHash](basic-types.md#PubkeyHash) | The old pubkeyHash |
| newPubkeyHash | [PubkeyHash](basic-types.md#PubkeyHash) | The new pubkeyHash |
| oldNonce      | [Nonce](basic-types.md#Nonce)           | The old nonce      |
| newNonce      | [Nonce](basic-types.md#Nonce)           | The new nonce      |

Transaction that generate `AccountChangePubkeyUpdate`:

* [ChangePubKey](./transaction/change\_pubkey.md)

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
| subAccountId | [SubAccountId](basic-types.md#SubAccountId) | The subaccount id             |
| coinId       | [TokenId](basic-types.md#TokenId)           | The token id                  |
| oldBalance   | String                                     | The old balance of subaccount |
| newBalance   | String                                     | The new balance of subaccount |
| oldNonce     | [Nonce](basic-types.md#Nonce)               | The old nonce of subaccount   |
| newNonce     | [Nonce](basic-types.md#Nonce)               | The new nonce of subaccount   |

Transaction that generate `BalanceUpdate`:

* [ChangePubKey](./transaction/change\_pubkey.md)
* [Deposit](./transaction/deposit.md)
* [ForcedExit](./transaction/forced\_exit.md)
* [FullExit](./transaction/full\_exit.md)
* [Transfer](./transaction/transfer.md)
* [Withdraw](./transaction/withdraw.md)
* [OrderMatching](./transaction/order\_matching.md)
* [ContractMatching](./transaction/contract\_matching.md)
* [Liquidation](./transaction/liquidation.md)
* [AutoDeleveraging](./transaction/auto\_deleveraging.md)
* [Funding](./transaction/funding.md)
* [UpdateGlobalVar](./transaction/update\_global\_var.md)

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
| subAccountId | [SubAccountId](basic-types.md#SubAccountId) | Subaccount id   |
| slotId       | [SlotId](basic-types.md#SlotId)             | Slot id         |
| oldTidyOrder | [ResponseTidyOrder](#ResponseTidyOrder)    | Old`TidyOrder`  |
| newTidyOrder | [ResponseTidyOrder](#ResponseTidyOrder)    | New `TidyOrder` |

where ResponseTidyOrder is

| Name         | Type                                       | Description                                                  |
|--------------|--------------------------------------------|--------------------------------------------------------------|
| nonce   | [Nonce](#Nonce) | The slot nonce of order                                      |
| residue | String          | The string format of BigDecimal, the residue balance of slot |

Transaction that generate `OrderUpdate`:

* [OrderMatching](./transaction/order\_matching.md)

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

## GlobalVarUpdate

`GlobalVarsUpdate` is an enumeration type used to represent the update status of global variables. It includes the following 4 types:

1. `FeeAccountUpdate`: The fee account of global variables changed
2. `InsuranceFundAccountUpdate`: The Insurance fund account of global variables changed
3. `MarginParamsUpdate`: The margin parameter of global variables changed
4. `ContractParamsUpdate`: The contract parameter of global variables changed

{% tabs %}
{% tab title="FeeAccountUpdate" %}

| Field              | Type                                        | Description        |
|--------------------|---------------------------------------------|--------------------|
| type               | String                                      | The update name    |
| update_id          | i32                                         | update index       |
| sub_account_id     | [SubAccountId](basic-types.md#SubAccountId) | sub-account ID     |
| old_fee_account_id | [AccountId](basic_type.md#AccountId)        | Old fee account ID |
| new_fee_account_id | [AccountId](basic_type.md#AccountId)        | New fee account ID |

Transaction that generate `FeeAccountUpdate`:

* [UpdateGlobalVar](./transaction/update\_global\_var.md)

For Example:

```json
{
  "stateUpdateType": "GlobalVarsUpdate",
  "globalVarUpdate": "FeeAccountUpdate",
  "updateId": 0,
  "subAccountId": 0,
  "oldFeeAccountId": 0,
  "newFeeAccountId": 3
}
```

{% endtab %}

{% tab title="InsuranceFundAccountUpdate" %}

| Field                         | Type                                        | Description                   |
|-------------------------------|---------------------------------------------|-------------------------------|
| type                          | String                                      | The update name               |
| update_id                     | i32                                         | update index                  |
| sub_account_id                | [SubAccountId](basic-types.md#SubAccountId) | sub-account id                |
| old_insurance_fund_account_id | [AccountId](basic_type.md#AccountId)        | old insurance fund account id |
| new_insurance_fund_account_id | [AccountId](basic_type.md#AccountId)        | new insurance fund account id |

Transaction that generate `InsuranceFundAccountUpdate`:

* [UpdateGlobalVar](./transaction/update\_global\_var.md)

For example:

```json
{
  "stateUpdateType": "GlobalVarsUpdate",
  "globalVarUpdate": "InsuranceFundAccountUpdate",
  "updateId": 0,
  "subAccountId": 0,
  "oldInsuranceFundAccountId": 4,
  "newInsuranceFundAccountId": 6
}
```
  
{% endtab %}

{% tab title="MarginParamsUpdate" %}

| Field           | Type                                        | Description                          |
|-----------------|---------------------------------------------|--------------------------------------|
| type            | String                                      | The update name                      |
| update_id       | i32                                         | update index                         |
| sub_account_id  | [SubAccountId](basic-types.md#SubAccountId) | sub-account id                       |
| margin_id       | [MarginId](basic-types.md#MarginId)         | margin index                         |
| old_symbol      | String                                      | Old symbol of the margin token       |
| new_symbol      | String                                      | New symbol of the margin token       |
| old_index_price | String                                      | Old index price of the margin token  |
| new_index_price | String                                      | New index price of the margin token  |
| old_token_id    | [TokenId](basic-types.md#TokenId)           | Old margin token id of the margin_id |
| new_token_id    | [TokenId](basic-types.md#TokenId)           | New margin token id of the margin_id |
| old_ratio       | u8                                          | Old margin ratio of the margin token |
| new_ratio       | u8                                          | New margin ratio of the margin token |

Transaction that generate `MarginParamsUpdate`:

* [UpdateGlobalVar](./transaction/update\_global\_var.md)

For example

```json
{
  "stateUpdateType": "GlobalVarsUpdate",
  "globalVarUpdate": "MarginParamsUpdate",
  "updateId": 0,
  "subAccountId": 0,
  "marginId": 2,
  "oldSymbol": "",
  "newSymbol": "BTC",
  "oldIndexPrice": "0",
  "newIndexPrice": "0",
  "oldTokenId": 0,
  "newTokenId": 142,
  "oldRatio": 0,
  "newRatio": 90
}
```

{% endtab %}

{% tab title="ContractParamsUpdate" %}

| Field                       | Type                                        | Description                                      |
|-----------------------------|---------------------------------------------|--------------------------------------------------|
| update_id                   | i32                                         | update id                                        |
| sub_account_id              | [SubAccountId](basic-types.md#SubAccountId) | sub-account id                                   |
| pair_id                     | [PairId](basic-types.md#PairId)             | contract pair id                                 |
| old_symbol                  | String                                      | old symbol of the contract pair                  |
| new_symbol                  | String                                      | new symbol of the contract pair                  |
| old_maintenance_margin_rate | u16                                         | old maintenance margin rate of the contract pair |
| new_maintenance_margin_rate | u16                                         | new maintenance margin rate of the contract pair |
| old_initial_margin_rate     | u16                                         | old initial margin rate of the contract pair     |
| new_initial_margin_rate     | u16                                         | new initial margin rate of the contract pair     |
| old_acc_funding_price       | String                                      | old accumulated funding price the contract pair  |
| new_acc_funding_price       | String                                      | new accumulative funding price the contract pair |
| old_mark_price              | String                                      | old mark price of the contract pair              |
| new_mark_price              | String                                      | new mark price of the contract pair              |

Transaction that generate `ContractParamsUpdate`:

* [UpdateGlobalVar](./transaction/update\_global\_var.md)

For Example:

```json
{
  "stateUpdateType": "GlobalVarsUpdate",
  "globalVarUpdate": "ContractParamsUpdate",
  "updateId": 0,
  "subAccountId": 0,
  "pairId": 0,
  "oldSymbol": "",
  "newSymbol": "BTC/USDT",
  "oldMaintenanceMarginRate": 0,
  "newMaintenanceMarginRate": 5,
  "oldInitialMarginRate": 0,
  "newInitialMarginRate": 10,
  "oldAccFundingPrice": "0",
  "newAccFundingPrice": "0",
  "oldMarkPrice": "0",
  "newMarkPrice": "0"
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

### ContractMatching

Updates generated by ContractMatching:

```
OrderUpdate(maker), PositionUpdate(maker), BalanceUpdate(maker),
OrderUpdate(taker), PositionUpdate(taker), BalanceUpdate(taker),
BalanceUpdate(submitter), BalanceUpdate(fee), BalanceUpdate(submitter),
```

### Example

```json
"updates": [
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "OrderUpdate",
    "updateId": 9,
    "accountId": 14,
    "subAccountId": 0,
    "slotId": 61089,
    "oldTidyOrder": {
      "nonce": 0,
      "residue": "0"
    },
    "newTidyOrder": {
      "nonce": 1,
      "residue": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 10,
    "accountId": 14,
    "subAccountId": 0,
    "pairId": 0,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": true,
      "price": "70000000000000000000000",
      "value": "70000000000000000000000",
      "size": "1000000000000000000",
      "accFundingPrice": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 11,
    "accountId": 14,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "9981900000000000000000",
    "newBalance": "9911900000000000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "OrderUpdate",
    "updateId": 12,
    "accountId": 14,
    "subAccountId": 0,
    "slotId": 62895,
    "oldTidyOrder": {
      "nonce": 0,
      "residue": "0"
    },
    "newTidyOrder": {
      "nonce": 1,
      "residue": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 13,
    "accountId": 14,
    "subAccountId": 0,
    "pairId": 0,
    "oldPosition": {
      "direction": true,
      "price": "70000000000000000000000",
      "value": "70000000000000000000000",
      "size": "1000000000000000000",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 14,
    "accountId": 14,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "9911900000000000000000",
    "newBalance": "9771900000000000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 15,
    "accountId": 3,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "18100000000000000000",
    "newBalance": "228100000000000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 16,
    "accountId": 2,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "0",
    "newBalance": "0",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 17,
    "accountId": 3,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "228100000000000000000",
    "newBalance": "228100000000000000000",
    "oldNonce": 0,
    "newNonce": 0
  }
]
```

### Liquidation

Updates generated by Liquidation:

```
PositionUpdate(liquidator), ......,
PositionUpdate(insurance fund account), ......,
BalanceUpdate(liquidator), 
BalanceUpdate(insurance fund account),
BalanceUpdate(submitter), BalanceUpdate(fee account), 
```
#### Example

```json
"updates": [
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 0,
    "accountId": 21,
    "subAccountId": 0,
    "pairId": 0,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "-113682873499983950000"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "-113682873499983950000"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 1,
    "accountId": 21,
    "subAccountId": 0,
    "pairId": 1,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 2,
    "accountId": 21,
    "subAccountId": 0,
    "pairId": 2,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 3,
    "accountId": 21,
    "subAccountId": 0,
    "pairId": 3,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 4,
    "accountId": 21,
    "subAccountId": 0,
    "pairId": 4,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 5,
    "accountId": 21,
    "subAccountId": 0,
    "pairId": 5,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 6,
    "accountId": 21,
    "subAccountId": 0,
    "pairId": 6,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "0"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 7,
    "accountId": 21,
    "subAccountId": 0,
    "pairId": 7,
    "oldPosition": {
      "direction": true,
      "price": "3560007803000000000000",
      "value": "35600078030000000000000",
      "size": "10000000000000000000",
      "accFundingPrice": "400710000000000000000"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "400710000000000000000"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 8,
    "accountId": 21,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "570283843940000000000",
    "newBalance": "0",
    "oldNonce": 1,
    "newNonce": 1
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 9,
    "accountId": 21,
    "subAccountId": 0,
    "coinId": 17,
    "oldBalance": "1000000000000000000000",
    "newBalance": "0",
    "oldNonce": 1,
    "newNonce": 1
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 10,
    "accountId": 21,
    "subAccountId": 0,
    "coinId": 142,
    "oldBalance": "10099800000000000000",
    "newBalance": "0",
    "oldNonce": 1,
    "newNonce": 1
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 11,
    "accountId": 6,
    "subAccountId": 0,
    "pairId": 7,
    "oldPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "400710000000000000000"
    },
    "newPosition": {
      "direction": true,
      "price": "3560007803000000000000",
      "value": "35600078030000000000000",
      "size": "10000000000000000000",
      "accFundingPrice": "400710000000000000000"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 12,
    "accountId": 6,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "0",
    "newBalance": "570283843940000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 13,
    "accountId": 6,
    "subAccountId": 0,
    "coinId": 17,
    "oldBalance": "1000000000000000000000",
    "newBalance": "2000000000000000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 14,
    "accountId": 6,
    "subAccountId": 0,
    "coinId": 142,
    "oldBalance": "0",
    "newBalance": "10099800000000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 15,
    "accountId": 2,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "0",
    "newBalance": "0",
    "oldNonce": 0,
    "newNonce": 1
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 16,
    "accountId": 3,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "1298848471059090000000000",
    "newBalance": "1298848471059090000000000",
    "oldNonce": 0,
    "newNonce": 0
  }
]
```

### AutoDeleveraging

Updates generated by AutoDeleveraging:

```
PositionUpdate(insurance fund account), BalanceUpdate(insurance fund account),
PositionUpdate(ADL account), BalanceUpdate(ADL account),
BalanceUpdate(submitter), BalanceUpdate(fee),
```
#### Example

```json
"updates": [
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 17,
    "accountId": 6,
    "subAccountId": 0,
    "pairId": 7,
    "oldPosition": {
      "direction": true,
      "price": "3560007803000000000000",
      "value": "35600078030000000000000",
      "size": "10000000000000000000",
      "accFundingPrice": "400710000000000000000"
    },
    "newPosition": {
      "direction": false,
      "price": "0",
      "value": "0",
      "size": "0",
      "accFundingPrice": "400710000000000000000"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 18,
    "accountId": 6,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "570283843940000000000",
    "newBalance": "-24986787496060000000000",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 19,
    "accountId": 126,
    "subAccountId": 0,
    "pairId": 7,
    "oldPosition": {
      "direction": false,
      "price": "3559280996274044968406",
      "value": "238450471064383368613418",
      "size": "66994000000000000000",
      "accFundingPrice": "400710000000000000000"
    },
    "newPosition": {
      "direction": false,
      "price": "3559280996274044968406",
      "value": "202857661101642918929355",
      "size": "56994000000000000000",
      "accFundingPrice": "400710000000000000000"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 20,
    "accountId": 126,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "1005451189769616631386582",
    "newBalance": "1031000993042357081070645",
    "oldNonce": 0,
    "newNonce": 0
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 21,
    "accountId": 2,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "0",
    "newBalance": "0",
    "oldNonce": 1,
    "newNonce": 2
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 22,
    "accountId": 3,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "1298848471059090000000000",
    "newBalance": "1298848471059090000000000",
    "oldNonce": 0,
    "newNonce": 0
  }
]
```

### funding

Updates generated by funding:

```
PositionUpdate(funding account), PositionUpdate(funding account), 
BalanceUpdate(funding account), BalanceUpdate(fee account),
```
#### Example

```json
[
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 2,
    "accountId": 20,
    "subAccountId": 0,
    "pairId": 0,
    "oldPosition": {
      "direction": true,
      "price": "70726400000000000000000",
      "value": "240469760000000000000000",
      "size": "3400000000000000000",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": true,
      "price": "70726400000000000000000",
      "value": "240469760000000000000000",
      "size": "3400000000000000000",
      "accFundingPrice": "14149851999998000000"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "PositionUpdate",
    "updateId": 7,
    "accountId": 20,
    "subAccountId": 0,
    "pairId": 7,
    "oldPosition": {
      "direction": false,
      "price": "3581685331905781584582",
      "value": "5017941150000000000000",
      "size": "1401000000000000000",
      "accFundingPrice": "0"
    },
    "newPosition": {
      "direction": false,
      "price": "3581685331905781584582",
      "value": "5017941150000000000000",
      "size": "1401000000000000000",
      "accFundingPrice": "5374000000000000000"
    }
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 8,
    "accountId": 20,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "294694153026870000000000",
    "newBalance": "294653572504070006800000",
    "oldNonce": 2,
    "newNonce": 2
  },
  {
    "stateUpdateType": "AccountUpdate",
    "accountUpdateType": "BalanceUpdate",
    "updateId": 9,
    "accountId": 4,
    "subAccountId": 0,
    "coinId": 140,
    "oldBalance": "0",
    "newBalance": "0",
    "oldNonce": 4,
    "newNonce": 5
  }
]
```

### update_global_var

All updates that may be generated by update_global_var:

#### Example
```
FeeAccountUpdate(global)
```
```json
{
  "stateUpdateType": "GlobalVarsUpdate",
  "globalVarUpdate": "FeeAccountUpdate",
  "updateId": 0,
  "subAccountId": 0,
  "oldFeeAccountId": 0,
  "newFeeAccountId": 3
}
```

```
InsuranceFundAccountUpdate(global)
```
```json
{
  "stateUpdateType": "GlobalVarsUpdate",
  "globalVarUpdate": "InsuranceFundAccountUpdate",
  "updateId": 0,
  "subAccountId": 0,
  "oldInsuranceFundAccountId": 4,
  "newInsuranceFundAccountId": 6
}
```

```
MarginParamsUpdate(global)
```
```json
{
  "stateUpdateType": "GlobalVarsUpdate",
  "globalVarUpdate": "MarginParamsUpdate",
  "updateId": 0,
  "subAccountId": 0,
  "marginId": 2,
  "oldSymbol": "",
  "newSymbol": "BTC",
  "oldIndexPrice": "0",
  "newIndexPrice": "0",
  "oldTokenId": 0,
  "newTokenId": 142,
  "oldRatio": 0,
  "newRatio": 90
}
```

```
ContractParamsUpdate(global)
```
```json
{
  "stateUpdateType": "GlobalVarsUpdate",
  "globalVarUpdate": "ContractParamsUpdate",
  "updateId": 0,
  "subAccountId": 0,
  "pairId": 0,
  "oldSymbol": "",
  "newSymbol": "BTC/USDT",
  "oldMaintenanceMarginRate": 0,
  "newMaintenanceMarginRate": 5,
  "oldInitialMarginRate": 0,
  "newInitialMarginRate": 10,
  "oldAccFundingPrice": "0",
  "newAccFundingPrice": "0",
  "oldMarkPrice": "0",
  "newMarkPrice": "0"
}
```

`version: fa4618`
