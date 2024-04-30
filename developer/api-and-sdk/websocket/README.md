# Websocket

## Connect

In the public channel, when the client successfully connects to the ZkLink node, the server will return a message with ID 0, which contains the `listen_key` automatically created by the server for the connection:

```json
{ "result": 
    {
      "listen_key": "9752db94ea664f1ea1e58074f547a9fe"
    }, 
  "error_code": 0,
  "error_msg": "",
  "id": 0
}
```

The server will send `ping` frame to the client every two minutes, and the client should reply the `pong` frame within 10 minutes, otherwise the connection will be automatically disconnected.

## Public Topics

* Topic of transaction execution results: `txExecuteResult@{sub_account_id}`, for example when `sub_account_id` is `1`, the topic will be `txExecuteResult@1`;
* full exit event： `fullExitEvent@{sub_account_id}`, for example when `sub_account_id` is `1`， the topic will be `fullExitEvent@1`.

## Subscribe & Unsubscribe Topics

The client can subscribe and unsubscribe to topics at any time after connecting, and only needs to send a request to the service:

```json
{
    "method": "subscribe",
    "topics": ["fullExitEvent@01", "txExecuteResult@1"],
    "id": 1
}
```

where the value of `method` can be `subscribe` or `unSubscribe`. The id in the response content is an unsigned integer, which serves as the unique identifier of the communication. It also contains the value of the currently subscribed topic list.

```json
{
  "result": {
    "topics":["fullExitEvent@01", "txExecuteResult@1"]
  },
  "error_code":0,
  "error_msg":"",
  "id":1
}
```

## Query Topic

`GET /api/topics/{listen_key}`

The client can use the `listen_key`(returned when the Websocket first connection) to query the `topic` list of its corresponding Websocket connection. The return value is:

```json
["txExecuteResult@1", "fullExitEvent@1"]
```

When the Websocket connection corresponding to `listen_key` is disconnected, `null` is returned.

## Data Push

### Transaction Execution Results

topic: `txExecuteResult@{sub_account_id}`

| Name                | Type                                          | Describe                                                                                               |
| ------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| type                | String                                        | Event type                                                                                             |
| tx\_hash            | [TxHash](../data-types/basic-types.md#txhash) | The tx hash on L1                                                                                      |
| tx                  | [ZklinkTx](../data-types/transaction/)        | [Transaction](../data-types/transaction/) detail                                                       |
| receipt             | struct                                        | The transaction status after received                                                                  |
| > executed          | bool                                          | The transaction finished executing or not                                                              |
| > executedTimestamp | Option                                        | The Unix microsecond timestamp when transaction excute, when executed is false, the value will be null |
| > success           | bool                                          | The transaction executed successfull or not                                                            |
| > failReason        | Option                                        | The reason that transaction execute fail, the value will be null if transaction execute success        |
| > block             | Option                                        | Block height that contains the transaction, it will be null if success is false                        |
| > index             | Option                                        | The transaction index in the block, if value will be null if success is false                          |
| updates             | [StateUpdateResp](broken-reference) array     | Update event                                                                                           |

```json
{
  "type": "TxExecuteResult",
  "tx_hash": "0xde9b5a916309f5097825ed1fb34eb5cc3a52faa1f5ef901aab54ef546d8e86b7",
  "tx": {
      "type": "Transfer",
      "accountId": 8,
      "fromSubAccountId": 3,
      "toSubAccountId": 3,
      "to": "0xbfDa941Bd2a0eddB57b10f8E8d3486A738B92cCC",
      "token": 3,
      "amount": "998000000000000000",
      "fee": "3000000000000000",
      "ts": 1646101085,
      "nonce": 1,
      "signature": {
        "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
        "signature": "892c622afac908201df54a3cfdecf8eba46d5411bdc29365f5536f024c195f2893d6313a6371fe1659830e2560c1eaedbafcc835837593d017cd557074f0bb03"
      }
   },
  "receipt": {
      "executed": true,
      "executedTimestamp": 1689731233000000,
      "success": true,
      "failReason": null,
      "block": 3947,
      "index": 1
    },
  "updates": [
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
}
```

### Full Exit Event

topic: fullExitEvent@{sub\_account\_id}

| Name     | Type                                          | Description          |
| -------- | --------------------------------------------- | -------------------- |
| type     | String                                        | Event type           |
| tx\_hash | [TxHash](../data-types/basic-types.md#txhash) | The tx hash on L1    |
| tx       | [Transaction](../data-types/transaction/)     | FullExit transaction |

For example:

```json
{
  "type": "FullExitEvent",
  "tx_hash": "0xde9b5a916309f5097825ed1fb34eb5cc3a52faa1f5ef901aab54ef546d8e86b7",
  "tx": {
      "type": "FullExit",
      "toChainId": 1,
      "accountId": 25,
      "subAccountId": 1,
      "exitAddress": "0xae08c2e27765faef5cb05908dbac12242caf91af",
      "l2SourceToken": 47,
      "l1TargetToken": 47,
      "serialId": 43,
      "ethHash": "0x748d32538f71d937d9e2c47adc26c499d0451b87e4fd337c2d6190c3271dafd7"
   }
}
```
