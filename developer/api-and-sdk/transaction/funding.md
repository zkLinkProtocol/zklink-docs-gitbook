
<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> type              </td><td> String          </td><td> yes       </td><td> The value is "Funding"         </td></tr>
<tr><td> accountId         </td><td> <a href="../data_types.md#accountid">AccountId</a> </td><td> yes       </td><td> The account ID of Funding      </td></tr>
<tr><td> subAccountId      </td><td> <a href="../data_types.md#subaccountid">SubAccountId    </a></td><td> yes       </td><td> The subaccount ID of Funding   </td></tr>
<tr><td> subAccountNonce   </td><td> <a href="../data_types.md#nonce">Nonce</a></td><td> yes       </td><td> The subaccount nonce           </td></tr>
<tr><td> fundingAccountIds </td><td> <a href="../data_types.md#accountid">AccountId</a> array  </td><td> yes       </td><td> The account id list of funding </td></tr>
<tr><td> fee               </td><td> BigUint         </td><td> yes       </td><td> The fee                        </td></tr>
<tr><td> feeToken          </td><td> <a href="../data_types.md#tokenid">TokenId         </a></td><td> yes       </td><td> The token id of the fee        </td></tr>
<tr><td> signature         </td><td> <a href="../data_types.md#zklinksignature">ZkLinkSignature</a></td><td> yes       </td><td> The Zklink L2 signature        </td></tr>
</tbody>
</table>


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
const {FundingBuilder,Signer,newFunding,RpcClient } = require('./node-dist/zklink-sdk-node');
async function testFunding() {
    const private_key = "be725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4";
    try {
        const signer = new Signer(private_key);
        let tx_builder = new FundingBuilder(5,1,2,[3,4,5],"34343",17);
        let tx = newFunding(tx_builder);
        console.log(tx);
        let tx_signature = signer.signFunding(tx);
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
