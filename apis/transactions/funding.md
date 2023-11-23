# Funding

| Name              | Type            | Mandatory | Description                    |
|-------------------|-----------------|-----------|--------------------------------|
| type              | String          | yes       | The value is "Funding"         |
| accountId         | AccountId       | yes       | The account ID of Funding      |
| subAccountId      | SubAccountId    | yes       | The subaccount ID of Funding   |
| subAccountNonce   | Nonce           | yes       | The subaccount nonce           |
| fundingAccountIds | AccountId list  | yes       | The account id list of funding |
| fee               | BigUint         | yes       | The fee                        |
| feeToken          | TokenId         | yes       | The token id of the fee        |
| signature         | ZkLinkSignature | yes       | The Zklink L2 signature        |

For example:

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

## Sign Funding

{% tabs %}
{% tab title = "Golang" %}
```golang

```

For more detail please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK
{% endtab %}

{%tab title = "javascript" %}

```javascript

```

For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)
{% endtab %}
{% endtabs %}
