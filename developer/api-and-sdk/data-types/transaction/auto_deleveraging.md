
<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> type            </td><td>String          </td><td>yes       </td><td>The value is "AutoDeleveraging"                                                              </td></tr>
<tr><td> accountId       </td><td><a href="../basic-types.md#accountid">AccountId</a>       </td><td>yes       </td><td>Account id                                                                                 </td></tr>
<tr><td> subAccountId    </td><td><a href="../basic-types.md#subaccountid">SubAccountId</a>    </td><td>yes       </td><td>Subaccount id                                                                              </td></tr>
<tr><td> subAccountNonce </td><td><a href="../basic-types.md#nonce">Nonce</a>            </td><td>yes       </td><td>The nonce of subaccount                                                                    </td></tr>
<tr><td> oraclePrices         </td><td> struct          </td><td> yes       </td><td> contains all infomation about contract price and margin price </td></tr>
<tr><td> > contractPrices     </td><td> array           </td><td> yes       </td><td> <a name="a1" href="#">ContractPrice</a> array                                       </td></tr>                                  
<tr><td> > marginPrices       </td><td> array           </td><td> yes       </td><td> <a name="a2" href="#">SpotPriceInfo</a> array                                       </td></tr>  
<tr><td> adlAccountId    </td><td><a href="../basic-types.md#accountid">AccountId</a>       </td><td>yes       </td><td>The ADL account id                                                                         </td></tr>
<tr><td> pairId          </td><td><a href="../basic-types.md#pairid">PairId</a>          </td><td>yes       </td><td>The pair id, for example the id of BTC-USDT pair                                           </td></tr>
<tr><td> adlSize         </td><td>BigUint         </td><td>yes       </td><td>The ADL size, the value can't be zero                                                                       </td></tr>
<tr><td> adlPrice        </td><td><a href="../basic-types.md#price">BigUint</a>         </td><td>yes       </td><td>The ADL price, the value can't be zero                                                                  </td></tr>
<tr><td> fee             </td><td><a href="../../private-key-and-signature/encode/algorithm.md">BigUint</a>         </td><td>yes       </td><td>The fee                                                                                    </td></tr>
<tr><td> feeToken        </td><td><a href="../basic-types.md#tokenid">TokenId</a>         </td><td>yes       </td><td>The token id of the fee                                                                    </td></tr>
<tr><td> signature       </td><td><a href="../basic-types.md#zklinksignature">ZkLinkSignature</a> </td><td>yes       </td><td>The pub key hash corresponding to the signature must be aligned with the initiator account </td></tr>
</tbody>
</table>


where the `ContractPrice` and `SpotPriceInfo` defined as below:

{% tabs %}
{% tab title="ContractPrice" %}

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody><tr>
<td>pairId</td><td><a href="../basic-types.md#pairid">PairId</a></td><td>yes</td><td>The pair id of trade pair, for example the id of BTC-USDT pair</td></tr>
<td>marketPrice </td><td><a href="../basic-types.md#price">BigUint</a></td><td>yes</td><td> The market price of the associated pair</td></tr>
</tbody>
</table>

{% endtab %}

{% tab title="SpotPriceInfo" %}

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td>tokenId</td><td><a href="../basic-types.md#tokenid">TokenId</a></td><td>yes</td><td>The pair id of trade pair, for example the id of BTC-USDT pair</td></tr>
<tr><td>price</td><td><a href="../basic-types.md#price">BigUint</a></td><td>yes</td><td>The spot price</td></tr>
</tbody>
</table>


{% endtab %}
{% endtabs %}

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
import (
    fmt
    sdk "github.com/zkLinkProtocol/zklink_sdk/generated/uniffi/zklink_sdk"
)
func SignAutoDeleveraging() {
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

    builder := sdk.AutoDeleveragingBuilder{
        sdk.AccountId(3),
        sdk.SubAccountId(1),
        sdk.Nonce(9),
        contract_prices,
        margin_prices,
        sdk.AccountId(7),
        sdk.PairId(3),
        *big.NewInt(1000),
        *big.NewInt(100000000000),
         *big.NewInt(10000),
        sdk.TokenId(18),
    }
    tx := sdk.NewAutoDeleveraging(builder)
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignAutoDeleveraging(tx)
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
const {AutoDeleveragingBuilder,Signer,newAutoDeleveraging,ContractPrice,SpotPriceInfo,RpcClient } = require('./node-dist/zklink-sdk-node');

async function testAutoDeleveraging() {
    const private_key = "be725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4";
    try {
        const contract_price1 = new ContractPrice(1,"10000000000000");
        const contract_price2 = new ContractPrice(1,"2000000000000");
        let contract_prices = [];
        contract_prices.push(contract_price1.jsonValue());
        contract_prices.push(contract_price2.jsonValue());

        let margin_prices = [];
        const margin_price1 = new SpotPriceInfo(17,"3236653653635635");
        const margin_price2 = new SpotPriceInfo(18,"549574875297");
        margin_prices.push(margin_price1.jsonValue());
        margin_prices.push(margin_price2.jsonValue());
        let tx_builder = new AutoDeleveragingBuilder(5,1,10,contract_prices,margin_prices,3,2,"33535545","188888","199",17);
        let tx = newAutoDeleveraging(tx_builder);
        console.log(tx);
        const signer = new Signer(private_key);
        let tx_signature = signer.signAutoDeleveraging(tx);
        console.log(tx_signature);

        let submitter_signature = signer.submitterSignature(tx_signature.tx);
        console.log(submitter_signature);
        //send to zklink
        let rpc_client = new RpcClient("testnet");
        let tx_hash = await rpc_client.sendTransaction(tx_signature.tx,null,submitter_signature);
        console.log(tx_hash);

    } catch (error) {
        console.error(error);
    }

}
```

For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)
{% endtab %}
{% endtabs %}
