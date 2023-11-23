Automatic deleveraging

| Name            | Type            | Mandatory | Description                                                                                |
|-----------------|-----------------|-----------|--------------------------------------------------------------------------------------------|
| type            | String          | yes       | The value is "AutoDeleveraging"                                                              |
| accountId       | AccountId       | yes       | Account id                                                                                 |
| subAccountId    | SubAccountId    | yes       | Subaccount id                                                                              |
| subAccountNonce | Nonce           | yes       | The nonce of subaccount                                                                    |
| oraclePrices    | OraclePrices    | yes       | it contains a list of [ContractPrice]() and list of [SpotPriceInfo]()                      |
| adlAccountId    | AccountId       | yes       | The ADL account id                                                                         |
| pairId          | PairId          | yes       | The pair id, for example the id of BTC-USDT pair                                           |
| adlSize         | BigUint         | yes       | The ADL size                                                                               |
| adlPrice        | BigUint         | yes       | The ADL price                                                                              |
| fee             | BigUint         | yes       | The fee                                                                                    |
| feeToken        | TokenId         | yes       | The token id of the fee                                                                    |
| signature       | ZkLinkSignature | yes       | The pub key hash corresponding to the signature must be aligned with the initiator account |

where the `ContractPrice` and `SpotPriceInfo` defined as below:

{% tabs %}
{% tab title="ContractPrice" %}

| Name        | Type    | Mandatory | Description                                                    |
|-------------|---------|-----------|----------------------------------------------------------------|
| pairId      | PairId  | yes       | The pair id of trade pair, for example the id of BTC-USDT pair |
| marketPrice | BigUint | yes       |  The market price of the associated pair                       |

{% endtab %}

{% tab title="SpotPriceInfo" %}

| Name        | Type    | Mandatory | Description                                                    |
|-------------|---------|-----------|----------------------------------------------------------------|
| tokenId     | TokenId | yes       | The pair id of trade pair, for example the id of BTC-USDT pair |
| price | BigUint | yes       | The spot price                                              |
{% endtab %}

{% end tabs %}

For example:

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

## Sign autoDeleveraging

{% tabs %}
{% tab title="Golang" %}
```go

```

For more detail please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK
{% endtab %}

{%tab title="javascript" %}

```javascript

```

For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)
{% endtab %}
{% endtabs %}
