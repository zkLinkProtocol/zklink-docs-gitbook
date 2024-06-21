L3 transfer

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> type                                       </td><td> String                             </td><td> yes                              </td><td> The value is "Transfer"                                                                                                                         </td></tr>
<tr><td> accountId                                  </td><td> <a href="../basic-types.md#accountid">AccountId</a>                          </td><td> yes                              </td><td> Account ID of the from_account                                                                                                                  </td></tr>
<tr><td> fromSubAccountId                           </td><td> <a href="../basic-types.md#subaccountid">SubAccountId</a>                       </td><td> yes                              </td><td> Subaccount ID of the from_account                                                                                                               </td></tr>
<tr><td> toSubAccountId                             </td><td> <a href="../basic-types.md#subaccountid">SubAccountId</a>                       </td><td> yes                              </td><td> Sub-account ID of the to_account                                                                                                                </td></tr>
<tr><td> to                                         </td><td> String                             </td><td> yes                              </td><td> Account address of the to_account, if the account does not exist, a new account will be automatically created on zkLink Layer3 for this address </td></tr>
<tr><td> token                                      </td><td> <a href="../basic-types.md#tokenid">TokenId</a>                            </td><td> yes                              </td><td> Token ID                                                                                                                                        </td></tr>
<tr><td> amount                                     </td><td> <a href="../../private-key-and-signature/encode/algorithm.md">BigUint</a> </td><td> yes                              </td><td> Token amount, the value must be packable                                                                                                        </td></tr>
<tr><td> fee                                        </td><td> <a href="../../private-key-and-signature/encode/algorithm.md">BigUint</a> </td><td> yes                              </td><td> Fee returned by <code>estimateTransactionFee</code> API, the value should be packable                                                           </td></tr>
<tr><td> nonce </td><td> <a href="../basic-types.md#nonce">Nonce</a> </td><td> yes </td><td> Current nonce of the account                                                                                                                    |
<tr><td>signature </td><td> <a href="../basic-types.md#zklinksignature">ZkLinkSignature</a> </td><td> yes </td><td> the public key hash corresponding to the signature must be aligned with the from_account </td></tr>
<tr><td> ts                                         </td><td> u32                                </td><td> yes                              </td><td> Timestamp of the API call, used as front-end request id to generate transaction hash                                                            </td></tr>
</tbody>
</table>

For example:

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

## sign Transfer
{% tabs %}
{% tab title="Golang" %}
```go
import (
	"math/big"
	"fmt"
	sdk "github.com/zkLinkProtocol/zklink_sdk/go_example/generated/uniffi/zklink_sdk"
)
func SignTransfer() {
    privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
    address := sdk.ZkLinkAddress("0xAFAFf3aD1a0425D792432D9eCD1c3e26Ef2C42E9")
    builder := sdk.TransferBuilder {
        sdk.AccountId(1),
        address,
        sdk.SubAccountId(1),
        sdk.SubAccountId(1),
        sdk.TokenId(18),
        *big.NewInt(100000),
        *big.NewInt(100),
        sdk.Nonce(1),
        sdk.TimeStamp(1693472232),
    }
    tokenSymbol := "DAI"
    tx := sdk.NewTransfer(builder)
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignTransfer(tx, tokenSymbol)
    if err != nil {
        return
    }
    fmt.Println("L1 signature: %s", txSignature.Layer1Signature)
    fmt.Println("signed Tx: %s", txSignature.Tx)
    submitterSignature, err := signer.SubmitterSignature(txSignature.Tx)
    fmt.Println("submitter signature: %s, %s", submitterSignature.PubKey, submitterSignature.Signature)
}
```
For more details please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK
{% endtab %}

{% tab title="Javascript" %}

```javascript
import init, *  as wasm  from "./web-dist/zklink-sdk-web.js";

async function main() {
    await init();
    const to_address = "0x5505a8cD4594Dbf79d8C59C0Df1414AB871CA896";
    const ts  = Math.floor(Date.now() / 1000);
    try {
        let amount = wasm.closestPackableTransactionAmount("1234567899808787");
        let fee = wasm.closestPackableTransactionFee("10000567777")
        let tx_builder = new wasm.TransferBuilder(10, to_address, 1,
            1, 18, fee, amount, 1,ts);
        let transfer = wasm.newTransfer(tx_builder);
        let signer = new wasm.JsonRpcSigner();
        await signer.initZklinkSigner();
        let signature = await signer.signTransfer(transfer,"USDC")
        console.log(signature);

        let submitter_signature = signer.submitterSignature(signature.tx);
        console.log(submitter_signature);
        let rpc_client = new wasm.RpcClient("testnet");
        let l1_signature = new wasm.TxLayer1Signature(wasm.L1SignatureType.Eth,signature.eth_signature);
        let tx_hash = await rpc_client.sendTransaction(signature.tx,l1_signature,submitter_signature);
        console.log(tx_hash);

    } catch (error) {
        console.error(error);
    }

}
```
{% endtab %}
{% endtabs %}


