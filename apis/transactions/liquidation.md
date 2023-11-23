
| Name                 | Type            | Mandatory | Description                                                   |
|----------------------|-----------------|-----------|---------------------------------------------------------------|
| type                 | String          | yes       | The value is "Liquidation"                                    |
| accountId            | AccountId       | yes       | The account id of Liquidation                                 |
| subAccountId         | SubAccountId    | yes       | The subaccount id                                             |
| subAccountIdNonce    | Nonce           | yes       | The Nonce of subaccount id                                    |
| oraclePrices         | struct          | yes       | contains all infomation about contract price and margin price |
| > contractPrices     | array           | yes       | [ContractPrice]() array                                       |                                      |
| > marginPrices       | array           | yes       | [SpotPriceInfo]() array                                                 |                                      |
| liquidationAccountId | AccountId       | yes       | The account id of liquidation                                 |
| fee                  | BugUint         | yes       | The fee amount                                                |
| feeToken             | TokenId         | yes       | The token id of the fee                                       |
| signature            | ZkLinkSignature | yes       | The ZkLink L2 signature of Liquidation                        |

where the type `ContractPrices`  and `SpotPriceInfo` are as follows:

{% tabs %}
{% tab title="ContractPrice" %}

| Name        | Type              | Description                     |
|-------------|-------------------|---------------------------------|
| pairId      | [PairId](#PairId) | The pair id                     |
| marketPrice | BigUint           | The market price of the pair id |

{% endtab  %}

{% tab title="SpotPriceInfo" %}

| Name       | Type                | Description                   |
|------------|---------------------|-------------------------------|
| tokenId    | [TokenId](#tokenId) | The token id                  |
| marketPrice | BigUint             | The market price of the token |

{% endtab %}
{% endtabs %}

For example:

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

## Sign Liquidation

{% tabs %}
{% tab title="Golang" %}
```go

```

For more detail please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK
{% endtab %}

{% tab title="javascript" %}

```javascript

```

For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)
{% endtab %}
{% endtabs %}

