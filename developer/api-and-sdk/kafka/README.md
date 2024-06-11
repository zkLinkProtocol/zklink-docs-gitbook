# kafka

## Connect

When using Kafka, the client establishes a connection by specifying the Kafka server's address and port. In the configuration file (such as the application's configuration file or the Kafka client configuration), the bootstrap.servers property must be set to specify the connection information for the Kafka cluster.

Below is a JSON format example demonstrating how to configure the Kafka server's address and port in the client:

```json
{
  "servers": "localhost:9092"
}
```

This configuration ensures that the client can locate and connect to the Kafka server running on localhost with the port number 9092.



## Consumer Topics

### Common consumer topics
The event type [`TxEventMsg`](#txeventmsg) and its associated data structures consumed in the Kafka `SUBMIT_TX_TOPIC` topic. This topic is specifically used for receiving l2 submit transactions, as well as FullExit ready message.

### Event type

The event type consumed is `TxEventMsg`, which is an enumeration type. Currently, there is only `Submit`.

#### Submit event

The `Submit` event carries a `BatchSubmitMessages` structure, which represents a batch of submitted transaction messages.

### data structure

#### TxEventMsg

| eventType | Type                                        | Description                             |
|-----------|---------------------------------------------|-----------------------------------------|
| Submit    | [BatchSubmitMessages](#batchsubmitmessages) | Submitted transaction batch information |

#### BatchSubmitMessages

| Field     | Type                       | Description                |
|-----------|----------------------------|----------------------------|
| messages  | Vec<[TxParams](#txparams)> | Transaction parameter list |
| messageId | `i64`                      | message id                 |
| id        | `i64`                      | batch id                   |
| createdAt | `i64`                      | Creation timestamp         |

#### TxParams

| Field              | Type                                                                | Description         |
|--------------------|---------------------------------------------------------------------|---------------------|
| tx                 | [ZkLinkTx](../data-types/transaction)                               | Transaction details |
| ethSignature       | [TxLayer1Signature](../data-types/basic-types.md#txlayer1signature) | layer1 signature    |
| submitterSignature | [ZkLinkSignature](../data-types/basic-types.md#zklinksignature)     | Submitter signature |

### JSON example

```json
{
 "refId": 0,
 "createdAt": 1712495689413,
 "eventType": "submit",
 "messageId": 94795,
 "id": 510694,
 "messages": [
   {
     "tx": {
       "type": "Funding",
       "accountId": 4,
       "subAccountId": 0,
       "fee": "0",
       "feeToken": 140,
       "signature": {
         "pubKey": "b939c75660ac6ec9dc7c4233c53647ba69bb8db7dc0485134f14fc8bc8b23e15",
         "signature": "4af9bc51a6118b1226a1887bd657faedeeb5422f87b04c23af94d2c5193a041e1855c45dc576e5966e4ca47b9f28200d4600a8ad78d8f9159cd0a0af6625bf01"
       },
       "subAccountNonce": 755,
       "fundingAccountIds": [93]
     },
     "ethSignature": null,
     "submitterSignature": {
       "pubKey": "b939c75660ac6ec9dc7c4233c53647ba69bb8db7dc0485134f14fc8bc8b23e15",
       "signature": "7f709ca1cff725ef0bec2a5147bd2405cf2cf843b16e94e0af2c4559eae31f122f685da8fc52c6d0056b19e29ecd87532467e8642a6203d46307d617b9237403"
     }
   }
 ]
}
```

### Special consumer topics
The `SYSTEM` topic, be used for stopping consuming messages.

## Producer Topics

### Topic Description

This document provides details about the event types and data structures associated with the `TX_RESULT_TOPIC` topic in Kafka. It is designed to guide producers on how to format messages that consumers will process.

### Consumer Usage Guide

To ensure that consumers can effectively process messages received from the `TX_RESULT_TOPIC` topic, here are the data and structures that consumers need to be familiar with:

### data structure

#### TxMsgResp(stage)

| stage    | Type                              | Description                                                                                                      |
|----------|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| ready    | [TxReadyResp](#txreadyresp)       | This is for FullExit, Deposit, means layer2 is ready, not yet executed(be executed after [submit](#event-type)). |
| submit   | [TxExecutedResp](#txexecutedresp) | This is the result for a failed tx(all tx type).                                                                 |
| executed | [TxExecutedResp](#txexecutedresp) | This is the result for a successfully executed tx(all tx type).                                                  |

#### TxReadyResp

| Field   | Type                                          | Description       |
|---------|-----------------------------------------------|-------------------|
| txInput | Vec<[ZkLinkTx](../data-types/transaction)>    | Transaction input |
| txHash  | [TxHash](../data-types/basic-types.md#txhash) | Transaction Hash  |

#### TxExecutedResp

| Field     | Type                                       | Description         |
|-----------|--------------------------------------------|---------------------|
| txInput   | Vec<[ZkLinkTx](../data-types/transaction)> | Transaction input   |
| messageId | `Option<i64>`                              | message ID          |
| id        | `Option<i64>`                              | Transaction ID      |
| txResults | Vec<[TxResult](#txresult)>                 | Transaction results |

#### TxResult

`TxResult` is an enumeration type that contains three cases: `Succeed`, `Failed` and `UnExecuted`.

##### Succeed

| Field   | Type                                                       | Description      |
|---------|------------------------------------------------------------|------------------|
| txHash  | [TxHash](../data-types/basic-types.md#txhash)              | Transaction Hash |
| updates | Vec<[StateUpdateResp](../json-rpc/json-rpc-api.md#txresp)> | Status updates   |

##### Failed

| Field   | Type     | Description   |
|---------|----------|---------------|
| code    | `i32`    | error code    |
| message | `String` | error message |

##### UnExecuted

No field means the transaction was not executed.

### JSON example

#### ready
```json
{
  "stage": "ready",
  "txInput": [
    {
      "type": "Deposit",
      "fromChainId": 1,
      "from": "0x76920dfacad4f28f97d6209977c1057b9e3e5cad",
      "subAccountId": 1,
      "l1SourceToken": 18,
      "l2TargetToken": 1,
      "amount": "4000000000000000000000",
      "to": "0x76920dfacad4f28f97d6209977c1057b9e3e5cad",
      "serialId": 53,
      "l2Hash": "0xaaa1e7a5bc48e7cfaa562a4d1a5abc1d6dc5e7f7683e89eb00e895d438f0acab"
    }
  ],
  "txHash": "0xbb4d4852f06e143aa1451666085180a517e83f2c0a5f42fdd40e8df180b54c91"
}
```

#### submit
```json
{
  "stage": "submit",
  "txInput": [
    {
      "type": "OrderMatching",
      "accountId": 2,
      "subAccountId": 0,
      "taker": {
        "accountId": 28,
        "subAccountId": 0,
        "slotId": 9378,
        "nonce": 6,
        "baseTokenId": 17,
        "quoteTokenId": 140,
        "amount": "33000000000000000000",
        "price": "1000000000000000000",
        "isSell": 0,
        "hasSubsidy": 0,
        "feeRates": [
          10,
          20
        ],
        "signature": {
          "pubKey": "0x06cca8fbfa06c39801f8cb0d8032539cba8491f4fdee275abf390c49ec9e8dae",
          "signature": "a7702be3d72b3f9ea333c3da90746845a21bd9b00273f7f8363d8542d60ef22085737ea8eba0fc383edf29ebe4e0fa83b3ddc2f96f9b7f44cd29e17783efe400"
        }
      },
      "maker": {
        "accountId": 100,
        "subAccountId": 0,
        "slotId": 14381,
        "nonce": 4,
        "baseTokenId": 17,
        "quoteTokenId": 140,
        "amount": "31000000000000000000",
        "price": "1000000000000000000",
        "isSell": 1,
        "hasSubsidy": 0,
        "feeRates": [
          10,
          20
        ],
        "signature": {
          "pubKey": "0x9953827fc901dd57718d464ddc5db23445474e9f87f6f554c81176c5d93a760b",
          "signature": "f43675557eb4f581a97bf1662ce2b0058f56f718569fbcafb6b2c53fe7efc2971ebe906f7185daab8f2b3e363f8281b91027189eb51bc65491a54e1a7944c004"
        }
      },
      "oraclePrices": {
        "contractPrices": [
          {
            "pairId": 0,
            "marketPrice": "69407038011000000000000"
          },
          {
            "pairId": 1,
            "marketPrice": "0"
          },
          {
            "pairId": 2,
            "marketPrice": "0"
          },
          {
            "pairId": 3,
            "marketPrice": "0"
          },
          {
            "pairId": 4,
            "marketPrice": "0"
          },
          {
            "pairId": 5,
            "marketPrice": "0"
          },
          {
            "pairId": 6,
            "marketPrice": "0"
          },
          {
            "pairId": 7,
            "marketPrice": "1000085417000000000000"
          }
        ],
        "marginPrices": [
          {
            "tokenId": 140,
            "price": "1000000000000000000"
          },
          {
            "tokenId": 17,
            "price": "999500000000000000"
          },
          {
            "tokenId": 142,
            "price": "1000000000000000000000"
          },
          {
            "tokenId": 0,
            "price": "0"
          }
        ]
      },
      "fee": "0",
      "feeToken": 140,
      "expectBaseAmount": "0",
      "expectQuoteAmount": "0",
      "signature": {
        "pubKey": "0x51ab37221738f1b013233e07b4d502abed0562060689b3ccc7b7766999dfc681",
        "signature": "7b41379292f91b77cd2ce1b750c9e18fdaf569cbbe6f33b4f89ce3cca3b8d7a9ccbf6d364bdd61bf7d590d0e30997b325e7dda4091b7d225529a7a8fe67c9001"
      }
    }
  ],
  "messageId": null,
  "id": null,
  "txResults": [
    {
      "code": 213,
      "message": "Duplicate tx"
    }
  ]
}
```

#### executed
```json
{
  "stage": "executed",
  "txInput": [
    {
      "type": "OrderMatching",
      "accountId": 2,
      "subAccountId": 0,
      "taker": {
        "accountId": 28,
        "subAccountId": 0,
        "slotId": 9378,
        "nonce": 6,
        "baseTokenId": 17,
        "quoteTokenId": 140,
        "amount": "33000000000000000000",
        "price": "1000000000000000000",
        "isSell": 0,
        "hasSubsidy": 0,
        "feeRates": [
          10,
          20
        ],
        "signature": {
          "pubKey": "0x06cca8fbfa06c39801f8cb0d8032539cba8491f4fdee275abf390c49ec9e8dae",
          "signature": "a7702be3d72b3f9ea333c3da90746845a21bd9b00273f7f8363d8542d60ef22085737ea8eba0fc383edf29ebe4e0fa83b3ddc2f96f9b7f44cd29e17783efe400"
        }
      },
      "maker": {
        "accountId": 100,
        "subAccountId": 0,
        "slotId": 14381,
        "nonce": 4,
        "baseTokenId": 17,
        "quoteTokenId": 140,
        "amount": "31000000000000000000",
        "price": "1000000000000000000",
        "isSell": 1,
        "hasSubsidy": 0,
        "feeRates": [
          10,
          20
        ],
        "signature": {
          "pubKey": "0x9953827fc901dd57718d464ddc5db23445474e9f87f6f554c81176c5d93a760b",
          "signature": "f43675557eb4f581a97bf1662ce2b0058f56f718569fbcafb6b2c53fe7efc2971ebe906f7185daab8f2b3e363f8281b91027189eb51bc65491a54e1a7944c004"
        }
      },
      "oraclePrices": {
        "contractPrices": [
          {
            "pairId": 0,
            "marketPrice": "69407038011000000000000"
          },
          {
            "pairId": 1,
            "marketPrice": "0"
          },
          {
            "pairId": 2,
            "marketPrice": "0"
          },
          {
            "pairId": 3,
            "marketPrice": "0"
          },
          {
            "pairId": 4,
            "marketPrice": "0"
          },
          {
            "pairId": 5,
            "marketPrice": "0"
          },
          {
            "pairId": 6,
            "marketPrice": "0"
          },
          {
            "pairId": 7,
            "marketPrice": "1000085417000000000000"
          }
        ],
        "marginPrices": [
          {
            "tokenId": 140,
            "price": "1000000000000000000"
          },
          {
            "tokenId": 17,
            "price": "999500000000000000"
          },
          {
            "tokenId": 142,
            "price": "1000000000000000000000"
          },
          {
            "tokenId": 0,
            "price": "0"
          }
        ]
      },
      "fee": "0",
      "feeToken": 140,
      "expectBaseAmount": "0",
      "expectQuoteAmount": "0",
      "signature": {
        "pubKey": "0x51ab37221738f1b013233e07b4d502abed0562060689b3ccc7b7766999dfc681",
        "signature": "7b41379292f91b77cd2ce1b750c9e18fdaf569cbbe6f33b4f89ce3cca3b8d7a9ccbf6d364bdd61bf7d590d0e30997b325e7dda4091b7d225529a7a8fe67c9001"
      }
    }
  ],
  "messageId": null,
  "id": null,
  "txResults": [
    {
      "txHash": "0xbb4d4852f06e143aa1451666085180a517e83f2c0a5f42fdd40e8df180b54c91",
      "updates": [
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "OrderUpdate",
          "updateId": 0,
          "accountId": 100,
          "subAccountId": 0,
          "slotId": 14381,
          "oldTidyOrder": {
            "nonce": 4,
            "residue": "0"
          },
          "newTidyOrder": {
            "nonce": 4,
            "residue": "25000000000000000000"
          }
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "BalanceUpdate",
          "updateId": 1,
          "accountId": 100,
          "subAccountId": 0,
          "coinId": 17,
          "oldBalance": "68498000000000000000",
          "newBalance": "62498000000000000000",
          "oldNonce": 0,
          "newNonce": 0
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "BalanceUpdate",
          "updateId": 2,
          "accountId": 100,
          "subAccountId": 0,
          "coinId": 140,
          "oldBalance": "39306373000000000000",
          "newBalance": "45300373000000000000",
          "oldNonce": 0,
          "newNonce": 0
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "OrderUpdate",
          "updateId": 3,
          "accountId": 28,
          "subAccountId": 0,
          "slotId": 9378,
          "oldTidyOrder": {
            "nonce": 6,
            "residue": "6000000000000000000"
          },
          "newTidyOrder": {
            "nonce": 7,
            "residue": "0"
          }
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "BalanceUpdate",
          "updateId": 4,
          "accountId": 28,
          "subAccountId": 0,
          "coinId": 140,
          "oldBalance": "67593004000000000000",
          "newBalance": "61593004000000000000",
          "oldNonce": 0,
          "newNonce": 0
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "BalanceUpdate",
          "updateId": 5,
          "accountId": 28,
          "subAccountId": 0,
          "coinId": 17,
          "oldBalance": "35333000000000000000",
          "newBalance": "41321000000000000000",
          "oldNonce": 0,
          "newNonce": 0
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "BalanceUpdate",
          "updateId": 6,
          "accountId": 2,
          "subAccountId": 0,
          "coinId": 140,
          "oldBalance": "0",
          "newBalance": "0",
          "oldNonce": 6,
          "newNonce": 6
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "BalanceUpdate",
          "updateId": 7,
          "accountId": 3,
          "subAccountId": 0,
          "coinId": 17,
          "oldBalance": "6761781393480000000000",
          "newBalance": "6761793393480000000000",
          "oldNonce": 0,
          "newNonce": 0
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "BalanceUpdate",
          "updateId": 8,
          "accountId": 3,
          "subAccountId": 0,
          "coinId": 140,
          "oldBalance": "1307544014198290000000000",
          "newBalance": "1307544020198290000000000",
          "oldNonce": 0,
          "newNonce": 0
        },
        {
          "stateUpdateType": "AccountUpdate",
          "accountUpdateType": "BalanceUpdate",
          "updateId": 9,
          "accountId": 3,
          "subAccountId": 0,
          "coinId": 140,
          "oldBalance": "1307544020198290000000000",
          "newBalance": "1307544020198290000000000",
          "oldNonce": 0,
          "newNonce": 0
        }
      ]
    }
  ]
}
```