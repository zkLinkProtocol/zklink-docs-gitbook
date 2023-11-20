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
* [AutoDeleveraging](layer2-transaction.md#autodeleveraging)
* [ContractMatching](layer2-transaction.md#contractmatching)
* [Funding](layer2-transaction.md#funding)
* [Liquidation](layer2-transaction.md#liquidation)
* [UpdateGlobalVar](layer2-transaction.md#updateglobalvar)

## ChangePubKey

Modifies the public key hash of the Layer2 account.

### Payload

<table><thead><tr><th width="196">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td><td>ChangePubKey</td></tr>
<tr><td>chainId</td><td>ID defined by zkLink, for example, when the user performs ChangePubKey on ETH, the front-end needs to set this value to the Ethereum ID defined by zkLink on Layer2</td></tr>
<tr><td>accountId</td><td>Target account ID of ChangePubKey</td></tr>
<tr><td>subAccountId</td><td>Target subaccount ID of ChangePubKey, the fee will be deducted from this subaccount</td></tr>
<tr><td>newPkHash</td><td>New public key hash</td></tr>
<tr><td>nonce</td><td>Current nonce of the target account</td></tr>
<tr><td>feeToken</td><td>The token used as the fee token</td></tr>
<tr><td>fee</td><td>Fee obtained via <code>estimateTransactionFee</code> API, the value should be packable</td></tr>
<tr><td>ts</td><td>Timestamp of the API call, used as front-end request id to generate transaction hash</td></tr>
<tr><td>ethAuthData</td><td><code>ChangePubKeyAuthData</code> to set the public key</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the public key hash corresponding to the signature must be aligned with the newPkHash</td></tr>
</tbody></table>

#### **ChangePubKeyAuthData**

*   EthECDSA

    ```json
    {
      "type":"EthECDSA",
      "ethSignature":  "0x36a83c6358c55870f2da3a9a7abcbace3016debd6e6982e7fc9aace159592d2b6f42eb5a20b166e1b73193019c63c57cf90b7c2b531f6c5ec572f622e66b4a9e1c"
    }
    ```
*   EthCreate2

    ```json
    {
      "type":"EthCreate2",
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
  "accountId":8 // tx.accountId
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
    "pubKey": "0xed53a138751ed1e456f46e74eff3463d2420e488a4f608bde0f28d13c7104d29",
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

| Name         | Rule                                                                                         |
| ------------ |----------------------------------------------------------------------------------------------|
| type         | 1 byte with value `0x06`                                                                     |
| chainId      | 1 byte                                                                                       |
| accountId    | 4 bytes                                                                                      |
| subAccountId | 1 byte                                                                                       |
| newPkHash    | 20 bytes                                                                                     |
| feeToken     | 2 bytes                                                                                      |
| fee          | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| nonce        | 4 bytes                                                                                      |
| ts           | 4 bytes                                                                                      |

39 bytes in total.

## Transfer

Layer-2 transfer

### Payload

<table><thead><tr><th width="269">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td><td>Transfer</td></tr>
<tr><td>accountId</td><td>Account ID of the from_account</td></tr>
<tr><td>fromSubAccountId</td><td>Subaccount ID of the from_account</td></tr>
<tr><td>toSubAccountId</td><td>Sub-account ID of the to_account</td></tr>
<tr><td>to</td><td>Account address of the to_account, if the account does not exist, a new account will be automatically created on zkLink Layer2 for this address</td></tr>
<tr><td>token</td><td>Token ID</td></tr>
<tr><td>amount</td><td>Token amount, the value must be packable</td></tr>
<tr><td>fee</td><td>Fee returned by <code>estimateTransactionFee</code> API, the value should be packable</td></tr>
<tr><td>ts</td><td>Timestamp of the API call, used as front-end request id to generate transaction hash</td></tr>
<tr><td>nonce</td><td>Current nonce of the account</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the public key hash corresponding to the signature must be aligned with the from_account</td></tr></tbody></table>

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
    "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "892c622afac908201df54a3cfdecf8eba46d5411bdc29365f5536f024c195f2893d6313a6371fe1659830e2560c1eaedbafcc835837593d017cd557074f0bb03"
  }
}
```

### Encode

| Name             | Rule                                                                                            |
| ---------------- |-------------------------------------------------------------------------------------------------|
| type             | 1 byte with value `0x04`                                                                        |
| accountId        | 4 bytes                                                                                         |
| fromSubAccountId | 1 byte                                                                                          |
| to               | 32 bytes, extended to 32 bytes with prefix `0x00` prefix                                        |
| toSubAccountId   | 1 byte                                                                                          |
| token            | 2 bytes                                                                                         |
| amount           | 5 bytes, refer to the `amount` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| feeAmount        | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm)    |
| nonce            | 4 bytes                                                                                         |
| ts               | 4 bytes                                                                                         |

56 bytes in total.

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

<table><thead><tr><th width="263">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td><td>Withdraw</td></tr>
<tr><td>toChainId</td><td>The target chain of the withdrawal</td></tr>
<tr><td>accountId</td><td>ID of the withdraw account</td></tr>
<tr><td>subAccountId</td><td>ID of the withdraw subaccount</td></tr>
<tr><td>to</td><td>The target address of the withdrawal</td></tr>
<tr><td>l2SourceToken</td><td>The source token to be deducted from the Layer2 account and used as the fee token</td></tr>
<tr><td>l1TargetToken</td><td>The target token to be sent to the to_address on Layer1</td></tr>
<tr><td>amount</td><td>Withdrawal amount, the value does not have to be packable</td></tr>
<tr><td>fee</td><td>Fee requested via <code>estimateTransactionFee</code> API, the value should be packable</td></tr>
<tr><td>withdrawFeeRatio</td><td>Transaction fee for fast withdraw, 100 as 1%, 10000 as 100%</td></tr>
<tr><td>ts</td><td>Timestamp of the API call, used as front-end request id to generate transaction hash</td></tr>
<tr><td>nonce</td><td>Current nonce of the account</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the public key hash corresponding to the signature must be aligned with the withdraw account</td></tr>
</tbody></table>

### **Example**

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
  "ts": 1646102148,
  "nonce": 0,
  "signature": {
    "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
  }
}
```

### Encode

| Name             | Rule                                                                                             |
|------------------|--------------------------------------------------------------------------------------------------|
| type             | 1 byte with value `0x03`                                                                         |
| toChainId        | 1 byte                                                                                           |
| accountId        | 4 bytes                                                                                          |
| subAccountId     | 1 byte                                                                                           |
| to               | 32 bytes, extend to 32 bytes with `0x00` prefix                                                  |
| l2SourceToken    | 2 bytes                                                                                          |
| l1TargetToken    | 2 bytes                                                                                          |
| amount           | 16 bytes, refer to the `amount` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| fee              | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm)     |
| nonce            | 4 bytes                                                                                          |
| withdrawToL1     | 1 byte                                                                                           |
| withdrawFeeRatio | 2 bytes                                                                                          |
| ts               | 4 bytes                                                                                          |

72 bytes in total.

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

<table><thead><tr><th width="273">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td><td>ForcedExit</td></tr>
<tr><td>toChainId</td><td>The target chain of the withdrawal</td></tr>
<tr><td>initiatorAccountId</td><td>Account ID of the transaction initiator</td></tr>
<tr><td>initiatorSubAccountId</td><td>Subaccount ID of the transaction initiator</td></tr>
<tr><td>initiatorNonce</td><td>Nonce of the transaction initiator's subaccount</td></tr>
<tr><td>target</td><td>The account address of the forced withdraw, the token on Layer 1 is also sent to this address</td></tr>
<tr><td>targetSubAccountId</td><td>Subaccount ID of the account of the forced withdraw</td></tr>
<tr><td>l2SourceToken</td><td>The token deducted from the account of the forced withdraw</td></tr>
<tr><td>l1TargetToken</td><td>This token sent to the to_address on L1</td></tr>
<tr><td>exitAmount</td><td>Withdrawal amount</td></tr>
<tr><td>ts</td><td>Timestamp of the API call, used as front-end request id to generate transaction hash</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the public key hash corresponding to the signature must be aligned with the initiator account</td></tr>
</tbody></table>

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
    "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
  }
}
```

### Encode

<table><thead><tr><th width="248">Name</th><th>Rule</th></tr></thead><tbody>
<tr><td>type</td><td>1 byte with the value "7"</td></tr>
<tr><td>toChainId</td><td>1 byte</td></tr>
<tr><td>initiatorAccountId</td><td>4 bytes</td></tr>
<tr><td>initiatorSubAccountId</td><td>1 byte</td></tr>
<tr><td>target</td><td>20 bytes</td></tr>
<tr><td>targetSubAccountId</td><td>1 byte</td></tr>
<tr><td>l2SourceToken</td><td>2 bytes</td></tr>
<tr><td>l1TargetToken</td><td>2 bytes</td></tr>
<tr><td>initiatorNonce</td><td>4 bytes</td></tr>
<tr><td>exitAmount</td><td>Refer to the SDK <strong>serializeAmountFull</strong>, 16 bytes</td></tr>
<tr><td>ts</td><td>4 bytes</td></tr>
</tbody></table>

56 bytes in total.



## OrderMatching

Order Matching

### Payload

<table><thead><tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td><td>OrderMatching</td></tr>
<tr><td>accountId</td><td>Initiator's account id. Only specific accounts can initiate this type of transaction on Layer2</td></tr>
<tr><td>subAccountId</td><td>Initiator's subaccount id</td></tr>
<tr><td>taker</td><td><code>Order</code>, taker order</td></tr>
<tr><td>maker</td><td><code>Order</code>, maker order</td></tr>
<tr><td>feeToken</td><td>Fee token, deducted from the initiator's subaccount</td></tr>
<tr><td>fee</td><td>Fee returned via the <code>estimateTransactionFee</code> API. The value should be packable</td></tr>
<tr><td>expectBaseAmount</td><td>The maximum amount of base token that the initiator expects to be traded in this order matching, which cannot exceed the maximum amount that the maker and taker can actually trade. The value does not need to be packable</td></tr>
<tr><td>expectQuoteAmount</td><td>The maximum amount of quote token that the initiator expects to be traded in this order matching, which cannot exceed the maximum amount that the maker and taker can actually trade. The value does not need to be packable</td></tr>
<tr><td>signature</td><td><code>ZklinkSignature</code>, the pub key hash corresponding to the signature must be aligned with the initiator account</td></tr>
</tbody></table>

#### **Order**

<table><thead><tr><th width="190">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>accountId</td><td>Account id</td></tr>
<tr><td>subAccountId</td><td>Subaccount id</td></tr>
<tr><td>slotId</td><td>Slot id</td></tr>
<tr><td>nonce</td><td>Slot nonce</td></tr>
<tr><td>baseTokenId</td><td>The base token, for example, BTC in BTC/USDT is the base token</td></tr>
<tr><td>quoteTokenId</td><td>The quote token, for example, USDT in BTC/USDT is the quote token</td></tr>
<tr><td>amount</td><td>Order amount. The value must be packablee</td></tr>
<tr><td>price</td><td>Order price. The value does not need to be packable</td></tr>
<tr><td>isSell</td><td>1 for seller, that is, selling the base token. 0 for buyer, that is, buying the base token</td></tr>
<tr><td>feeRates</td><td>The list of [maker fee rate, taker fee rate]. 100 for 1%, maximum is 2.56%</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the pub key hash corresponding to the signature must be aligned with the accountId</td></tr>
</tbody></table>

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
    "feeRates": [5, 10],
    "signature": {
      "pubKey": "0x1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
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
    "feeRates": [5, 10],
    "signature": {
      "pubKey": "0x1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
      "signature": "9579e54f53aa709e72c7e4de9815d258cf92bb3e9c4b9d03c2f79a7a49b5bda062d7e81b278eb62f2452294d178728b460efdb80017c83748dd190e41e05b802"
    }
  },
  "fee": "405000000000000",
  "feeToken": 1,
  "expectBaseAmount": "373400000000000000000",
  "expectQuoteAmount": "452150060000000000000",
  "signature": {
    "pubKey": "0x84bf4edbe1f7056f079ba4c38359427f43d529fbab2e94e6d6b7a18efbf2fb87",
    "signature": "1242830780b17dd362e8d31952deab6d8b5d81cadd62779b2caab0821baa030a770afa9a2c249d8f45000c4b9c6f01ef6b002682760e5bc4e5d39ef7f511ce03"
  }
}
```

### Encode

| Name              | Rule                                                                                         |
| ----------------- |----------------------------------------------------------------------------------------------|
| type              | 1 byte with the value `0x08`                                                                 |
| accountId         | 4 bytes                                                                                      |
| subAccountId      | 1 byte                                                                                       |
| orderBytesHash    | 32 bytes, refer to Rust SDK `rescue_hash_orders`                                             |
| feeToken          | 2 bytes                                                                                      |
| fee               | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| expectBaseAmount  | 16 bytes                                                                                     |
| expectQuoteAmount | 16 bytes                                                                                     |

74 bytes in total.

### Order encode

| Name         | Rule                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------|
| type         | 1 byte with value `0xff`                                                                        |
| accountId    | 4 bytes                                                                                         |
| subAccountId | 1 byte                                                                                          |
| slotId       | 2 bytes                                                                                         |
| nonce        | 4 bytes                                                                                         |
| baseTokenId  | 2 bytes                                                                                         |
| quoteTokenId | 2 bytes                                                                                         |
| price        | 15 bytes                                                                                        |
| isSell       | 1 byte                                                                                          |
| feeRates     | 2 byte                                                                                          |
| amount       | 5 bytes, refer to the `amount` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |

39 bytes in total.

```js
var ordersBytes = [makerOrderBytes, takerOrderBytes, paddingBytes];
// Where makerOrderBytes and takerOrderBytes are the encoded values of taker and maker respectively
// paddingBytes is padding with zeros to make the length of orderBytes 178 bytes
var orderBytesHash = rescueHashOrders(ordersBytes);
```

## **autoDeleveraging**
Automatic deleveraging

### Payload

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td>   <td>AutoDeleveraging</td></tr>
<tr><td>accountId</td> <td>Account id</td></tr>
<tr><td>subAccountId</td><td>Subaccount id</td></tr>
<tr><td>subAccountNonce</td><td>The nonce of subaccount</td></tr>
<tr><td>oraclePrices</td><td>The <code>OraclePrices</code>, which contains a list of <code>ContractPrice</code> and list of <code>MarginPrice</code> </td></tr>
<tr><td>adlAccountId</td><td>The ADL account id</tr>
<tr><td>pairId</td><td>The pair id, for example the id of BTC-USDT pair</td></tr>
<tr><td>adlSize</td><td>The ADL size</td></tr>
<tr><td>adlPrice</td><td>The ADL price</td></tr>
<tr><td>fee</td><td>The fee</td></tr>
<tr><td>feeToken</td><td>The token id of the fee</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the pub key hash corresponding to the signature must be aligned with the initiator account</td></tr>
</tbody></table>

### ContractPrice
<table><thead>
<tr><th width="190">Field</th><th>Description</th></tr></thead>
<tbody>
<tr><td>pairId</td><td>The pair id of trade pair, for example the id of BTC-USDT pair</td></tr>
<tr><td>marketPrice</td><td>The market price of the associated pair</td></tr>
</tbody></table>

#### ContractPrice encode

| Name   | Rule                                                                              |
|--------|-----------------------------------------------------------------------------------|
| pariId | 1 byte                                                                            |
|marketPrice| 15 bytes, encode in big endian, then pass to the `pad_front` function in Rsut SDK |


### MarginPrice

<table><thead>
<tr><th width="190">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>tokenId</td><td>The token id of trade pair</td></tr>
<tr><td>marketPrice</td><td>The market price of the associated token</td></tr>
</tbody></table>

#### MarginPrice encode

| Name        | Rule                                                                                  |
|-------------|---------------------------------------------------------------------------------------|
| tokenId     | 2 byte, change to `u16`, then encode in big endian                                    |
| marketPrice | 15 bytes, encode in big endian, then pass to the`pad_front` function in Rust SDK |


### OraclePrices

<table><thead>
<tr><th width="190">Field</th><th>Description</th></tr></thead>
<tbody>
<tr><td>contractPrices</td><td>The list of <code>ContractPrice</code></td></tr>
<tr><td>spotPriceInfo</td><td>The list of <code>MarginPrice</code></td></tr>
</tbody></table>


### **Example**

```json
{
  "type": "AutoDeleveraging",
  "accountId": 0,
  "subAccountId": 0,
  "subAccountNonce": 0,
  "oraclePrices": {
    "contractPrices": [
      {
        "pairId": 1,
        "marketPrice": "100"
      }
    ],
    "marginPrices": [
      {
        "tokenId": 1,
        "price": "100"
      }
    ]
  },
  "adlAccountId": 0,
  "pairId": 0,
  "adlSize": "0",
  "adlPrice": "0",
  "fee": "100",
  "feeToken": 1,
  "signature": {
    "pubKey": "0x43cbec0bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
    "signature": "366e759d61a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
  }
}
```

### Encode
| Name  | Rule                                                                                        |
|-------|---------------------------------------------------------------------------------------------|
| type  | 1byte with the value `0x0b`                                                                 |
| accountId | 4 bytes                                                                                     |
| subAccountId    | 1 byte                                                                                      |
| subAccountNonce | 4 bytes                                                                                     |
| oraclePrices    | 31 bytes, refer to Rust SDK `rescue_hash` method                                          |
| adlAccountId    | 4 bytes                                                                                     |
| pairId          | 1 byte                                                                                      |
| adlSize         | 5 bytes, refer to `amount` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| adlPrice        | 15 bytes, encode to big endian bytes, then pass to the`pad_front` function in Rust SDK      |
| fee             | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm)    |
| feeToken        | 2 bytes                                                                                     |
 
70 bytes in total

### **ContractMatching**
Contract matching

### Payload

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td><td>ContractMatching</td></tr>
<tr><td>accountId</td><td>The account id</td></tr>
<tr><td>subAccountId</td><td>The subaccount id</td></tr>
<tr><td>maker</td>The maker list of <code>Contract</code></tr>
<tr><td>taker</td>The taker of <code>Contract</code></tr>
<tr><td>fee</td><td>The fee</td></tr>
<tr><td>feeToken</td><td>The token id of the fee</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the pub key hash corresponding to the signature must be aligned with the account</td></tr>
</tbody></table>

### **Example**

```json
{
  "type": "ContractMatching",
  "accountId": 0,
  "subAccountId": 0,
  "maker": [
    {
      "accountId": 0,
      "subAccountId": 0,
      "slotId": 0,
      "nonce": 0,
      "pairId": 0,
      "size": "0",
      "price": "0",
      "direction": 0,
      "feeRates": [
        0,
        0
      ],
      "hasSubsidy": 0,
      "signature": {
        "pubKey": "0x99575738f142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
        "signature": "957826468392052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
      }
    }
  ],
  "taker": {
    "accountId": 0,
    "subAccountId": 0,
    "slotId": 0,
    "nonce": 0,
    "pairId": 0,
    "size": "0",
    "price": "0",
    "direction": 0,
    "feeRates": [
      0,
      0
    ],
    "hasSubsidy": 0,
    "signature": {
      "pubKey": "0x43cbec0bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
      "signature": "366e759d61a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
    }
  },
  "fee": "0",
  "feeToken": 0,
  "signature": {
    "pubKey": "0x1234567bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
    "signature": "1234567861a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
  }
}
```

### Contract
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>accountId</td><td>The account id.</td></tr>
<tr><td>subAccountId</td><td>The subaccount id</td></tr>
<tr><td>slotId</td><td>Slot id</td></tr>
<tr><td>nonce</td><td>Slot nonce </td></tr>
<tr><td>pairId</td><td>The id of trade pair, for example the id of BTC-USDT pair </td></tr>
<tr><td>size</td><td>Position size</td></tr>
<tr><td>price</td><td>Price</td></tr>
<tr><td>direction</td><td>1: Long, 0: Short</td></tr>
<tr><td>feeRates</td><td>The fee list of [maker, taker], 100 means 1.0%</td></tr>
<tr><td>hasSubsidy</td><td>1: true, 0: false, the submitter needs to pay the transaction fee to the maker, if the maker's "hasSubsidy" is true.  </td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the pub key hash corresponding to the signature must be aligned with the initiator account</td></tr>
</tbody></table>

#### Contract encode
| Name  | Rule                                                             |
|-------|------------------------------------------------------------------|
|type | 1 byte, `0xfe`                                                   |
|accountId | 4 bytes                                                          |
|subAccountId| 1 byte                                                           |
|slotId | 2 bytes, use the lower 2 bytes of u32                            |
|nonce | 3 bytes, encode to 4 bytes in big endian, keep the `bytes[1..3]` |
|pairId | 1 byte, encode to 2 bytes in big endian, keep the lower byte     |
|direction | 1 byte                                                           |
|size | 5 bytes                                                          |
|price | 15 bytes                                                         |
|feeRates | 2 bytes                                                          |
| hasSubsidy | 1 byte                                                           |


### encode

| Name         | Rule                                    |
|--------------|-----------------------------------------|
| type         | 1 byte, `0x09`                        |
| accountId    | 4 bytes                                 |
| subAccountId | 1 byte                                  |
| maker,taker  | 31 bytes                                |
| fee          | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| feeToken     | 2 bytes                                 |

41 bytes in total, where the `maker` and `taker` encode as bellow:
* [Encode](#contract-encode) the items in `maker` into a bytes list in order;
* [Encode](#contract-encode) the taker and append to the bytes list;
* pass the bytes list to the `rescue_hash_orders` function in SDK, and get the 31 bytes result.

## **Funding**

### Payload
<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<td>accountId</td><td>The account ID of Funding</td></tr>
<tr><td>subAccountId</td><td>The subaccount ID of Funding</td></tr>
<tr><td>subAccountNonce</td><td>The subaccount nonce</td></tr>
<tr><td>fundingAccountIds</td><td>The account id list of funding</td></tr>
<tr><td>fee</td><td>The fee</td></tr>
<tr><td>feeToken</td><td>The token id of the fee</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the pub key hash corresponding to the signature must be aligned with the initiator account</td></tr>
</tbody></table>


### **example**

```json
{
  "accountId": 0,
  "subAccountId": 0,
  "subAccountNonce": 0,
  "fundingAccountIds": [
    1,
    2
  ],
  "fee": "0",
  "feeToken": 0,
  "signature": {
    "pubKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "signature": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
  }
}
```

### encode

| Name         | Rule                                                   |
|--------------|--------------------------------------------------------|
|type | 1 byte, `0x0d`                                       |
| accountId         | 4 bytes                                                |
| subAccountId      | 1 byte                                                 |
| accountIdNonce | 4 bytes                                                |
| fundingAccountIds | 4 bytes(when length is 1) or 31 bytes(when length > 1) |
| fee | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm)                |
| feeToken | 2 bytes                                                |

The encoding process of `fundingAccountIds` is as blew:
* If the length is 1, encode the item directly in big endian;
* If the length > 1, encode the item in big endian into a bytes list in order;
* Pass the bytes list to the `rescue_hash` function in Rust SDK, and get the 31 bytes result.

## **Liquidation**

### Payload
<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td><td>Liquidation</td></tr>
<tr><td>accountId</td><td>The account id of Liquidation</td></tr>
<tr><td>subAccountId</td><td>The subaccount id</td></tr>
<tr><td>subAccountIdNonce</td><td>The Nonce of subaccount id </td></tr>
<tr><td>oraclePrices</td><td><code>oraclePrices</code> which contains a list of <code>ContractPrice </code> and a list of <code>MarginPrice</code></td></tr>
<tr><td>liquidationAccountId</td><td>The account id of liquidation</td></tr>
<tr><td>fee</td><td>The fee</td></tr>
<tr><td>feeToken</td><td>The token id of the fee</td></tr>
<tr><td>signature</td><td><code>ZkLinkSignature</code>, the pub key hash corresponding to the signature must be aligned with the account id</td></tr>
</tbody></table>

### **Example**

```json
{
  "type": "Liquidation",
  "accountId": 0,
  "subAccountId": 0,
  "subAccountNonce": 0,
  "oraclePrices": {
    "contractPrices": [
      {
        "pairId": 1,
        "marketPrice": "100"
      }
    ],
    "marginPrices": [
      {
        "tokenId": 1,
        "price": "100"
      }
    ]
  },
  "liquidationAccountId": 0,
  "fee": "0",
  "feeToken": 0,
  "signature": {
    "pubKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "signature": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
  }
}
```

### Encode

| Name         | Rule                                    |
|--------------|-----------------------------------------|
|type | 1 byte, `0x0d`                        |
| accountId         | 4 bytes                                 |
| subAccountId      | 1 byte                                  |
| accountIdNonce | 4 bytes                                 |
| oraclePrices | 31 bytes                                |
| liquidationAccountId | 4 bytes                                 |
| fee | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| feeToken | 2 bytes                                 |

49 bytes in total, where the `oraclePrices` encode process is as blew:
* Encode the `oraclePrices` into a bytes list in order;
* Pass the bytes list to the SDK `rescue_hash` function then get the 31 bytes result.

## **UpdateGlobalVar**

This transaction is used to update the global variable settings.

### Payload

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>type</td>   <td>UpdateGlobalVar</td></tr>
<tr><td>fromChainId</td><td>The layer2 chain id</td></tr>
<tr><td>subAccountId</td><td>The subaccount id</td></tr>
<tr><td>parameter</td>The <code>Parameter</code> used to update the global variable</tr>
<tr><td>seriaId</td><td>The serial id</td></tr>
</tbody></table>

### Encode

| Name         | Rule                        |
|--------------|-----------------------------|
| type         | 1 byte, `0x0c`              |
| fromChainId  | 1 byte                      |
| subAccountId | 4 bytes                     |
| parameter    | refer to the Parameter encode |
| serialId | 8 bytes                     |

### Parameter

#### feeAccount

Modify the collect-fee account.

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>feeAccount</td><td>The fee Account</td></tr>
</tbody></table>

##### **Example**

```json
{
  "feeAccount": {
    "accountId": 1
  }
}
```

##### **Encode**

| Name         | Rule          |
|--------------|---------------|
| type         | 1 byte, `0x00` |
| feeAccount  | 4 byte        |

5 bytes in total

#### insuranceFundAccount

Modify the insurance fund account

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>accountId</td><td>The Account id of insuranceFundAccount</td></tr>
</tbody></table>

##### **Example**

```json
{
  "insuranceFundAccount": {
    "accountId": 1
  }
}
```

##### **Encode**

| Name         | Rule                        |
|--------------|-----------------------------|
| type | 1 byte, `0x01` |
| accountId | 4 bytes |

5 bytes in total

#### marginInfo

Modify the margin info in the specified index.

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>marginId</td><td>The margin id</td></tr>
<tr><td>tokenId</td><td>The token id</td></tr>
<tr><td>ratio</td><td>the ratio, 100 means 0.1%</td></tr>
</tbody></table>

##### Example

```json
{
  "marginInfo": {
    "marginId": 1,
    "tokenId": 2,
    "ratio": 100
  }
}
```

##### Encode

| Name     | Rule                                                |
|----------|-----------------------------------------------------|
| type     | 1 byte, `0x02`                                      |
| marginId | 1 bytes                                             |
| tokenId  | 2 bytes, downcast to u16, then encode in big endian |
| ratio    | 1 byte                                              |


#### contractInfo

Modify the info of every contract pair.

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>pairId</td><td>The id of trade Pair</td></tr>
<tr><td>symbol</td><td>The symbol of the contract</td></tr>
<tr><td>initialMarginRate</td><td>The initial margin rate of the contract, 100 means 0.1%</td></tr>
<tr><td>maintenanceMarginRate</td><td>The maintenance margin rate, 100 means 0.1%</td></tr>
</tbody></table>

##### Example

```json
{
  "contractInfo": {
    "pairId": 0,
    "symbol": "BTCUSDC",
    "initialMarginRate": 2,
    "maintenanceMarginRate": 2
  }
}
```

##### Encode

| Name                  | Rule                                                      |
|-----------------------|-----------------------------------------------------------|
| type                  | 1 byte, `0x03`                                            |
| pariId                | 1 bytes, downcast to u8 as 1 byte                         |
| symbol                | 15 bytes, expand to 15 bytes with `0x00` at the beginning |
| initialMarginRate     | 2 bytes                                                   |
| maintenanceMarginRate | 2 bytes                                                   |


#### fundingInfos

Update the funding rates to accumulated funding rates of the Global Vars for all position(contract pair) in this period

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>infos</td><td><code>FundingInfo</code> list</td></tr>
</tbody></table>

where `fundingInfo` is:

<table><thead>
<tr><th width="255">Field</th><th>Description</th></tr></thead><tbody>
<tr><td>pairId</td><td>The trade pair id, for example, the BTC/USDT pair id </td></tr>
<tr><td>price</td><td>the mark price of the trade pair</td></tr>
<tr><td>fundingRate</td><td>the fee funding rate, the actual result needs to be divided by 10^6</td></tr>
</tbody></table>

and the encode rule of `fundingInfo` is as blew:

| Name        | Rule                                               |
|-------------|----------------------------------------------------|
| pariId      | 1 bytes, downcast to u8 as 1 byte                  |
| price       | 15 bytes, refer to the Rust SDK `pad_front` method |
| fundingRate | 2 bytes                                            |
 
18 bytes in total, where `fundingRate` encode is as following:

* Get the absolute result as `u16` type，then encode as 2 bytes in big endian.
* If fundingRate < 0: `bytes[0] |= 0b1000_0000`


##### Example

```json
{
  "fundingInfos": {
      "infos": [
        {
          "pairId": 0,
          "price": "0",
          "fundingRate": 0
        },
        {
          "pairId": 0,
          "price": "0",
          "fundingRate": 0
        }
      ]
    }
}
```

##### Encode
| Name        | Rule                                 |
|-------------|--------------------------------------|
| type | 1 byte, `0x04`                        |
|infos | 18 bytes * the length of fundingRates, encode `fundingInfo` in order |

# BigUint pack algorithm
The integer is composed of `mantissa` and `exponent` part of encoding, and pack algorithm is:

> `integer = mantissa * (exponent_base^exponent)`  

where the `exponent_base` is `10`. This compression algorithm is mainly targeted at two types of data `fee` and `amount`:

* fee: 11 bits of mantissa and 5bits of exponent, total in 2 bytes
* amount: 35 bits of mantissa and 5bits of exponent, total in 5 bytes


## max value limitation of algorithm
For the `amount`,  the max mantissa is `2^35 - 1`, and the max exponent is `2^5 - 1` or `31`. According to the  pack algorithm,  when exponent larger than 30,
the result will exceed `u128`, so we use `30` as the max value of exponent, the max value of `amount` is

>  max_amount = (2^35 - 1) * 10^30 = 34359738367000000000000000000000000000
 
For the `fee`, the max mantissa is `2^11 - 1`, and the max exponent is `2^5 - 1` or `31`, which make the max value of `fee` is

> max_fee = (2^11 - 1) * 10^31 = 20470000000000000000000000000000000
 
## Numerical precision loss

According to the formula, it can be seen that the compression algorithm will be numerical precision loss. For the numbers smaller than `2^128` will be changed after compression and cannot be recovered.
For example, a `fee` integer is `(2^10+1)*10^2+1 = 102501` where the `mantissa` is `2^10+1=101` and the `exponent` is `2`, the pack result will be `(2^10+1)*10^2 = 102500` which results in a `1` numerical precision loss.
 
`version: 4457a91`
