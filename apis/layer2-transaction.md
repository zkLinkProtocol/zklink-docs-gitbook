# Layer2 Transaction

There are two categories of zkLink transactions: L1Transaction and L2Transaction.

L1 Transactions are initiated by calling Layer1 contracts, check [Layer1 Transaction](layer1-transaction.md) for details.

* [Deposit](../streamline/deposit.md)
* [FullExit](layer1-transaction.md#fullexit)

L2Transaction are initiated by calling zkLink API [`sendTransaction`](json-rpc-api.md#sendtransaction):

* [ChangePubKey](layer2-transaction.md#changepubkey)
* [Transfer](layer2-transaction.md#transfer)
* [Withdraw](layer2-transaction.md#withdraw)
* [ForcedExit](layer2-transaction.md#forcedexit)
* [OrderMatching](layer2-transaction.md#ordermatching)

## ChangePubKey

Modifies the public key hash of the Layer2 account.

### Payload

<table><thead><tr><th width="196">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>ChangePubKey</td></tr><tr><td>chainId</td><td>ID defined by zkLink, for example, when the user performs ChangePubKey on ETH, the front-end needs to set this value to the Ethereum ID defined by zkLink on Layer2</td></tr><tr><td>accountId</td><td>Target account ID of ChangePubKey</td></tr><tr><td>subAccountId</td><td>Target subaccount ID of ChangePubKey, the fee will be deducted from this subaccount</td></tr><tr><td>newPkHash</td><td>New public key hash</td></tr><tr><td>nonce</td><td>Current nonce of the target account</td></tr><tr><td>feeToken</td><td>The token used as the fee token</td></tr><tr><td>fee</td><td>Fee obtained via <code>estimateTransactionFee</code> API, the value should be packable</td></tr><tr><td>ts</td><td>Timestamp of the API call, used as front-end request id to generate transaction hash</td></tr><tr><td>ethAuthData</td><td><code>ChangePubKeyAuthData</code> to set the public key</td></tr><tr><td>signature</td><td><code>TxSignature</code>, the public key hash corresponding to the signature must be aligned with the newPkHash</td></tr></tbody></table>

#### **ChangePubKeyAuthData**

*   EthECDSA

    ```json
    {
      "type":"EthECDSA",
      "ethSignature":  "0x36a83c6358c55870f2da3a9a7abcbace3016debd6e6982e7fc9aace159592d2b6f42eb5a20b166e1b73193019c63c57cf90b7c2b531f6c5ec572f622e66b4a9e1c"
    }
    ```
*   EthCREATE2

    ```json
    {
      "type":"EthCREATE2",
      "creatorAddress":"0x388c818ca8b9251b393131c08a736a67ccb19297",
      "saltArg":"0x66b8e2fa879542a0c32c77e137ab830c3345788a2696999afbd07acabab8ad81",
      "codeHash":"0xfb57f5a066444ff426f4433344fd2e179843fd9069c06eb3b4ecc74e8b599410"
    }
    ```
*   Onchain

    ```json
    {
      "type":"Onchain"
    }
    ```

The `EthECDSA` signature is assembled according to EIP712 standard, where the value of the domain is:

```json
{
  "name":"ZkLink", // Constant
  "version":"1", // Constant
  "chainId":13, // This is the Layer1 blockchain Id, not the ID defined by zkLink
  "verifyingContract": "0x388c818ca8b9251b393131c08a736a67ccb19297" // This is the address of the zkLink contract on Layer1 blockchain
}
```

{% hint style="info" %}
**Note:** `chainId` and `verifyingContract` are different for each chain, all related information can be obtained through the `getSupportChains` API.
{% endhint %}

EIP712 message includes:

```json
{
  "pubKeyHash":"0x79a239efe5cedff14e3e0c9bf9cd4c6c4cdec507", // tx.newPkHash
  "nonce":0, // tx.nonce
  "accountId":8

// tx.accountId
}
```

This message is the content of the signature displayed on Metamask.

#### Example

```json
{
  "type": "ChangePubKey",
  "chainId": 1,
  "accountId": 39,
  "subAccountId": 1,
  "newPkHash": "0xbfb4f4a68dc9e49f7785082a8c12354ed663b6e0",
  "feeToken": 1,
  "fee": "1285000000000000",
  "nonce": 0,
  "signature": {
    "pubKey": "ed53a138751ed1e456f46e74eff3463d2420e488a4f608bde0f28d13c7104d29",
    "signature": "3b91c0421df4295281596746722ae20ccf270c5fc0561f93a0219db1faea6518f033e778dd552f90a9a6afd06427428b2ac4ea6f6893a3f162b32683d1108a02"
  },
  "ethAuthData": {
    "type": "EthECDSA",
    "ethSignature": "0x8e548e3727a94533b3963877b87966e308e6eef7762f78de567ff14b4e0e87780d37a845501ffd2cdbc7d6f0d620c14589212761f1637ea8214b0b6bac10aa9b1b"
  },
  "ts": 1675650037
}
```

### Encode

| Name         | Rule                                         |
| ------------ | -------------------------------------------- |
| type         | 1 byte with value "6"                        |
| chainId      | 1 byte                                       |
| accountId    | 4 bytes                                      |
| subAccountId | 1 byte                                       |
| newPkHash    | 20 bytes                                     |
| feeToken     | 2 bytes                                      |
| fee          | Refer to SDK **serializeFeePacked**, 2 bytes |
| nonce        | 4 bytes                                      |
| ts           | 4 bytes                                      |

39 bytes in total.

## Transfer

Layer-2 transfer

### Payload

<table><thead><tr><th width="269">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>Transfer</td></tr><tr><td>accountId</td><td>Account ID of the from_account</td></tr><tr><td>fromSubAccountId</td><td>Subaccount ID of the from_account</td></tr><tr><td>toSubAccountId</td><td>Sub-account ID of the to_account</td></tr><tr><td>to</td><td>Account address of the to_account, if the account does not exist, a new account will be automatically created on zkLink Layer2 for this address</td></tr><tr><td>token</td><td>Token ID</td></tr><tr><td>amount</td><td>Token amount, the value must be packable</td></tr><tr><td>fee</td><td>Fee returned by <code>estimateTransactionFee</code> API, the value should be packable</td></tr><tr><td>ts</td><td>Timestamp of the API call, used as front-end request id to generate transaction hash</td></tr><tr><td>nonce</td><td>Current nonce of the account</td></tr><tr><td>signature</td><td><code>TxSignature</code>, the public key hash corresponding to the signature must be aligned with the from_account</td></tr></tbody></table>

#### **Example**

```json
{
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
    "pubKey": "0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "892c622afac908201df54a3cfdecf8eba46d5411bdc29365f5536f024c195f2893d6313a6371fe1659830e2560c1eaedbafcc835837593d017cd557074f0bb03"
  }
}
```

### Encode

| Name             | Rule                                            |
| ---------------- | ----------------------------------------------- |
| type             | 1 byte with value "4"                           |
| accountId        | 4 bytes                                         |
| fromSubAccountId | 1 byte                                          |
| to               | 20 bytes                                        |
| toSubAccountId   | 1 byte                                          |
| token            | 2 bytes                                         |
| amount           | Refer to SDK **serializeAmountPacked**, 5 bytes |
| feeAmount        | Refer to SDK **serializeFeePacked**, 2 bytes    |
| nonce            | 4 bytes                                         |
| ts               | 4 bytes                                         |

44 bytes in total.&#x20;

### Eth sig message

```
Transfer {amount} {token} to: {to}
Fee: {feeAmount} {token}
Nonce: {nonce}
```

* The unit of `amount` is ether, for example, if the user transfers `2.3` ZKL, then the amount would be `2.3`.
* `token`: the symbol of the token.
* `to`: the encoded address, such as `0xbfda941bbd2a0eddb57b10f8e8d3486a738b92ccc`(all in lowercase).
* `fee`: the amount of transaction fee being paid, the unit is the same as `amount` (ether). If `feeAmount` is 0, the message does not need to include `Fee: {fee} {token}`.

## Withdraw

Withdraw from zkLink L2 to connected networks.

### Payload

<table><thead><tr><th width="263">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>Withdraw</td></tr><tr><td>toChainId</td><td>The target chain of the withdrawal</td></tr><tr><td>accountId</td><td>ID of the withdraw account</td></tr><tr><td>subAccountId</td><td>ID of the withdraw subaccount</td></tr><tr><td>to</td><td>The target address of the withdrawal</td></tr><tr><td>l2SourceToken</td><td>The source token to be deducted from the Layer2 account and used as the fee token</td></tr><tr><td>l1TargetToken</td><td>The target token to be sent to the to_address on Layer1</td></tr><tr><td>amount</td><td>Withdrawal amount, the value does not have to be packable</td></tr><tr><td>fee</td><td>Fee requested via <code>estimateTransactionFee</code> API, the value should be packable</td></tr><tr><td>withdrawFeeRatio</td><td>Transaction fee for fast withdraw, 100 as 1%, 10000 as 100%</td></tr><tr><td>fastWithdraw</td><td>0 for normal withdraw, 1 for fast withdraw</td></tr><tr><td>ts</td><td>Timestamp of the API call, used as front-end request id to generate transaction hash</td></tr><tr><td>nonce</td><td>Current nonce of the account</td></tr><tr><td>signature</td><td><code>TxSignature</code>, the public key hash corresponding to the signature must be aligned with the withdraw account</td></tr></tbody></table>

**Example**

```json
{
  "type": "Withdraw",
  "toChainId": 1,
  "accountId": 7,
  "subAccountId": 2,
  "to": "0x3498f456645270ee003441df82c718b56c0e6666",
  "l2SourceToken": 1,
  "l1TargetToken": 17,
  "amount": "995900000000000000",
  "fee": "4100000000000000",
  "withdrawFeeRatio": 50,
  "fastWithdraw": 1,
  "ts": 1646102148,
  "nonce": 0,
  "signature": {
    "pubKey": "0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
  }
}
```

### Encode

| Name             | Rule                                           |
| ---------------- | ---------------------------------------------- |
| type             | 1 byte with value "3"                          |
| toChainId        | 1 byte                                         |
| accountId        | 4 bytes                                        |
| subAccountId     | 1 byte                                         |
| to               | 20 bytes                                       |
| l2SourceToken    | 2 bytes                                        |
| l1TargetToken    | 2 bytes                                        |
| amount           | Refer to SDK **serializeAmountFull**, 16 bytes |
| fee              | Refer to SDK **serializeFeePacked**, 2 bytes   |
| nonce            | 4 bytes                                        |
| fastWithdraw     | 1 byte                                         |
| withdrawFeeRatio | 2 bytes                                        |
| ts               | 4 bytes                                        |

60 bytes in total.

### Eth sig message

```
Withdraw {amount} {l2SourceToken} to: {to}
Fee: {feeAmount} {l2SourceToken}
Nonce: {nonce}
```

* The unit of `amount` is ether, for example, if the user transfers `2.3` ZKL, then the amount would be `2.3`.
* `token`: the symbol of the token.
* `to`: the encoded address, such as `0xbfda941bbd2a0eddb57b10f8e8d3486a738b92ccc`(all in lowercase).
* `fee`: the amount of transaction fee being paid, the unit is the same as `amount` (ether). If `feeAmount` is 0, the message does not need to include `Fee: {fee} {token}`.

## ForcedExit

Forced withdraw from Layer2

### Payload

<table><thead><tr><th width="273">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>ForcedExit</td></tr><tr><td>toChainId</td><td>The target chain of the withdrawal</td></tr><tr><td>initiatorAccountId</td><td>Account ID of the transaction initiator</td></tr><tr><td>initiatorSubAccountId</td><td>Subaccount ID of the transaction initiator</td></tr><tr><td>initiatorNonce</td><td>Nonce of the transaction initiator's subaccount</td></tr><tr><td>target</td><td>The account address of the forced withdraw, the token on Layer 1 is also sent to this address</td></tr><tr><td>targetSubAccountId</td><td>Subaccount ID of the account of the forced withdraw</td></tr><tr><td>l2SourceToken</td><td>The token deducted from the account of the forced withdraw</td></tr><tr><td>l1TargetToken</td><td>This token sent to the to_address on L1</td></tr><tr><td>exitAmount</td><td>Withdrawal amount</td></tr><tr><td>ts</td><td>Timestamp of the API call, used as front-end request id to generate transaction hash</td></tr><tr><td>signature</td><td><code>TxSignature</code>, the public key hash corresponding to the signature must be aligned with the initiator account</td></tr></tbody></table>

#### **Example**

```json
{
  "type": "ForcedExit",
  "toChainId": 1,
  "initiatorAccountId": 7,
  "initiatorSubAccountId": 3,
  "initiatorNonce":4,
  "target": "0x3498f456645270ee003441df82c718b56c0e6666",
  "targetSubAccountId": 2,
  "l2SourceToken": 1,
  "l1TargetToken": 17,
  "exitAmount": "4100000000000000",
  "ts": 1646102148,
  "nonce": 0,
  "signature": {
    "pubKey": "0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
  }
}
```

### Encode

<table><thead><tr><th width="248">Name</th><th>Rule</th></tr></thead><tbody><tr><td>type</td><td>1 byte with the value "7"</td></tr><tr><td>toChainId</td><td>1 byte</td></tr><tr><td>initiatorAccountId</td><td>4 bytes</td></tr><tr><td>initiatorSubAccountId</td><td>1 byte</td></tr><tr><td>target</td><td>20 bytes</td></tr><tr><td>targetSubAccountId</td><td>1 byte</td></tr><tr><td>l2SourceToken</td><td>2 bytes</td></tr><tr><td>l1TargetToken</td><td>2 bytes</td></tr><tr><td>initiatorNonce</td><td>4 bytes</td></tr><tr><td>exitAmount</td><td>Refer to the SDK <strong>serializeAmountFull</strong>, 16 bytes</td></tr><tr><td>ts</td><td>4 bytes</td></tr></tbody></table>

56 bytes in total.



## OrderMatching

Order Matching

### Payload

<table><thead><tr><th width="255">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>OrderMatching</td></tr><tr><td>accountId</td><td>Initiator's account id. Only specific accounts can initiate this type of transaction on Layer2</td></tr><tr><td>subAccountId</td><td>Initiator's subaccount id</td></tr><tr><td>taker</td><td><code>Order</code>, taker order</td></tr><tr><td>maker</td><td><code>Order</code>, maker order</td></tr><tr><td>feeToken</td><td>Fee token, deducted from the initiator's subaccount</td></tr><tr><td>fee</td><td>Fee returned via the <code>estimateTransactionFee</code> API. The value should be packable</td></tr><tr><td>expectBaseAmount</td><td>The maximum amount of base token that the initiator expects to be traded in this order matching, which cannot exceed the maximum amount that the maker and taker can actually trade. The value does not need to be packable</td></tr><tr><td>expectQuoteAmount</td><td>The maximum amount of quote token that the initiator expects to be traded in this order matching, which cannot exceed the maximum amount that the maker and taker can actually trade. The value does not need to be packable</td></tr><tr><td>signature</td><td><code>TxSignature</code>, the pub key hash corresponding to the signature must be aligned with the initiator account</td></tr></tbody></table>

#### **Order**

<table><thead><tr><th width="190">Field</th><th>Description</th></tr></thead><tbody><tr><td>accountId</td><td>Account id</td></tr><tr><td>subAccountId</td><td>Subaccount id</td></tr><tr><td>slotId</td><td>Slot id</td></tr><tr><td>nonce</td><td>Slot nonce</td></tr><tr><td>baseTokenId</td><td>The base token, for example, BTC in BTC/USDT is the base token</td></tr><tr><td>quoteTokenId</td><td>The quote token, for example, USDT in BTC/USDT is the quote token</td></tr><tr><td>amount</td><td>Order amount. The value must be packablee</td></tr><tr><td>price</td><td>Order price. The value does not need to be packable</td></tr><tr><td>isSell</td><td>1 for seller, that is, selling the base token. 0 for buyer, that is, buying the base token</td></tr><tr><td>feeRatio1</td><td>Fee rate for takers. 100 for 1%, for is 2.56%</td></tr><tr><td>feeRatio2</td><td>Fee rate for makers</td></tr><tr><td>signature</td><td><code>TxSignature</code>, the pub key hash corresponding to the signature must be aligned with the accountId</td></tr></tbody></table>

The `subAccountId`, `baseTokenId`, and `quoteTokenId` of the maker and taker must be the same, and `isSell` must be the opposite. If the taker is a sell order, the price of the taker must not be higher than the price of the maker. If the taker is a buy order, the price of the taker must not be lower than the price of the maker.

#### **Example**

```json
{
  "type": "OrderMatching",
  "accountId": 4,
  "subAccountId": 1,
  "taker": {
    "accountId": 11,
    "subAccountId": 1,
    "slotId": 5844,
    "nonce": 24,
    "baseTokenId": 42,
    "quoteTokenId": 1,
    "amount": "373400000000000000000",
    "price": "1210900000000000000",
    "isSell": 1,
    "feeRatio1": 5,
    "feeRatio2": 10,
    "signature": {
      "pubKey": "1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
      "signature": "3a5c5c23a74cc04d256f03eb1671acd3959d707252ed52b16cb7b2ffe332a804986e09e0d62bcf49fc9231b38f07b71199769a7343eddc8b43ed9dd2ef8a4405"
    }
  },
  "maker": {
    "accountId": 11,
    "subAccountId": 1,
    "slotId": 5915,
    "nonce": 14,
    "baseTokenId": 42,
    "quoteTokenId": 1,
    "amount": "5165400000000000000000",
    "price": "1210900000000000000",
    "isSell": 0,
    "feeRatio1": 5,
    "feeRatio2": 10,
    "signature": {
      "pubKey": "1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
      "signature": "9579e54f53aa709e72c7e4de9815d258cf92bb3e9c4b9d03c2f79a7a49b5bda062d7e81b278eb62f2452294d178728b460efdb80017c83748dd190e41e05b802"
    }
  },
  "fee": "405000000000000",
  "feeToken": 1,
  "expectBaseAmount": "373400000000000000000",
  "expectQuoteAmount": "452150060000000000000",
  "signature": {
    "pubKey": "84bf4edbe1f7056f079ba4c38359427f43d529fbab2e94e6d6b7a18efbf2fb87",
    "signature": "1242830780b17dd362e8d31952deab6d8b5d81cadd62779b2caab0821baa030a770afa9a2c249d8f45000c4b9c6f01ef6b002682760e5bc4e5d39ef7f511ce03"
  }
}
```

### Encode

| Name              | Rule                                         |
| ----------------- | -------------------------------------------- |
| type              | 1 byte with the value "8=="                  |
| accountId         | 4 bytes                                      |
| subAccountId      | 1 byte                                       |
| orderBytesHash    | 32 bytes, refer to SDK **rescueHashOrders**  |
| feeToken          | 2 bytes                                      |
| fee               | Refer to SDK **serializeFeePacked**, 2 bytes |
| expectBaseAmount  | 16 bytes                                     |
| expectQuoteAmount | 16 bytes                                     |

74 bytes in total.

### Order encode

| Name         | Rule                                        |
| ------------ | ------------------------------------------- |
| type         | 0xff, 1 byte                                |
| accountId    | 4 bytes                                     |
| subAccountId | 1 byte                                      |
| slotId       | 2 bytes                                     |
| nonce        | 4 bytes                                     |
| baseTokenId  | 2 bytes                                     |
| quoteTokenId | 2 bytes                                     |
| price        | 15 bytes                                    |
| isSell       | 1 byte                                      |
| feeRatio1    | 1 byte                                      |
| feeRatio2    | 1 byte                                      |
| amount       | Refer to SDK serializeAmountPacked, 5 bytes |

39 bytes in total.

```js
var ordersBytes = [makerOrderBytes, takerOrderBytes, paddingBytes];
// Where makerOrderBytes and takerOrderBytes are the encoded values of taker and maker respectively
// paddingBytes is padding with zeros to make the length of orderBytes 178 bytes
var orderBytesHash = rescueHashOrders(ordersBytes);
```

`version: 4457a91`
