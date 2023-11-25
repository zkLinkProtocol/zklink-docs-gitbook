
| Name              | Type            | Required | Description                    |
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
{% tab title="Golang" %}
```go
import (
    fmt
    sdk "github.com/zkLinkProtocol/zklink_sdk/generated/uniffi/zklink_sdk"
)

func SignFunidng() {
    privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"

    var funding_account_ids = make([]sdk.AccountId,3)
    funding_account_ids[0] = sdk.AccountId(55)
    funding_account_ids[1] = sdk.AccountId(56)
    funding_account_ids[2] = sdk.AccountId(57)

    builder := sdk.FundingBuilder{
        sdk.AccountId(1),
        sdk.SubAccountId(99),
        sdk.Nonce(23),
        funding_account_ids,
        *big.NewInt(100000000000),
        sdk.TokenId(17),
    }
    tx := sdk.NewFunding(builder)
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignFunding(tx)
    if err != nil {
        return
    }
    fmt.Println("L1 signature: %s", txSignature.Layer1Signature)
    fmt.Println("signed Tx: %s", txSignature.Tx)
    submitterSignature, err := signer.SubmitterSignature(txSignature.Tx)
    fmt.Println("submitter signature: %s, %s", submitterSignature.PubKey, submitterSignature.Signature)
}
```

For more detail please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK
{% endtab %}

{%tab title="javascript" %}

```javascript

```

For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)
{% endtab %}
{% endtabs %}
