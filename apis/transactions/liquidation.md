<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>

<tr><td> type                 </td><td> String          </td><td> yes       </td><td> The value is "Liquidation"                                    </td></tr>
<tr><td> accountId            </td><td> <a href="../data_types.md#accountid">AccountId</a>       </td><td> yes       </td><td> The account id of Liquidation                                 </td></tr>
<tr><td> subAccountId         </td><td> <a href="../data_types.md#subaccountid">SubAccountId</a></td><td> yes       </td><td> The subaccount id                                             </td></tr>
<tr><td> subAccountIdNonce    </td><td> <a href="../data_types.md#nonce">Nonce</a></td><td> yes       </td><td> The Nonce of subaccount id                                    </td></tr>
<tr><td> oraclePrices         </td><td> struct          </td><td> yes       </td><td> contains all infomation about contract price and margin price </td></tr>
<tr><td> > contractPrices     </td><td> array           </td><td> yes       </td><td> <a name="a1" href="#">ContractPrice</a> array                                       </td></tr>                                  
<tr><td> > marginPrices       </td><td> array           </td><td> yes       </td><td> <a name="a2" href="#">SpotPriceInfo</a> array                                       </td></tr>                                 
<tr><td> liquidationAccountId </td><td> <a href="../data_types.md#accountid">AccountId</a></td><td> yes       </td><td> The account id of liquidation                                 </td></tr>
<tr><td> fee                  </td><td> BugUint         </td><td> yes       </td><td> The fee amount                                                </td></tr>
<tr><td> feeToken             </td><td> <a href="../data_types.md#tokenid">TokenId</a></td><td> yes       </td><td> The token id of the fee                                       </td></tr>
<tr><td> signature            </td><td> <a href="../data_types.md#zklinksignature">ZkLinkSignature</a></td><td> yes       </td><td> The ZkLink L2 signature of Liquidation                        </td></tr>

</tbody>
</table>

<p id="a1">where the type ContractPrice and SpotPriceInfo are as follows:</p>

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
import (
    fmt
    sdk "github.com/zkLinkProtocol/zklink_sdk/generated/uniffi/zklink_sdk"
)

func HighLevelLiquidation() {
    privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"

    contract_price1 := sdk.ContractPrice{
        sdk.PairId(1),
        *big.NewInt(656566),
    }

    contract_price2 := sdk.ContractPrice{
        sdk.PairId(3),
        *big.NewInt(52552131),
    }
    var contract_prices = make([]sdk.ContractPrice,2)
    contract_prices[0] = contract_price1
    contract_prices[1] = contract_price2

    margin_price1 := sdk.SpotPriceInfo {
       sdk.TokenId(17),
       *big.NewInt(3236653653635635),
    }
    margin_price2 := sdk.SpotPriceInfo {
      sdk.TokenId(18),
      *big.NewInt(549574875297),
    }

    var margin_prices = make([]sdk.SpotPriceInfo,2)
    margin_prices[0] = margin_price1
    margin_prices[1] = margin_price2

    builder := sdk.LiquidationBuilder {
        sdk.AccountId(1),
        sdk.SubAccountId(1),
        sdk.Nonce(9),
        contract_prices,
        margin_prices,
        sdk.AccountId(3),
       *big.NewInt(5545),
        sdk.TokenId(17),
    }

    tx := sdk.NewLiquidation(builder)
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignLiquidation(tx)
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

{% tab title="javascript" %}

```javascript

```

For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)
{% endtab %}
{% endtabs %}

