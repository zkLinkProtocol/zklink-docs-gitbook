Withdraw from zkLink L2 to connected networks.

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> type             </td><td> String          </td><td> yes       </td><td> The value is "Withdraw"                                                                               </td></tr>
<tr><td> toChainId        </td><td> <a href="../basic-types.md#chainid">ChainId</a>           </td><td> yes       </td><td> The target chain of the withdrawal                                                                    </td></tr>
<tr><td> accountId        </td><td> <a href="../basic-types.md#accountid">AccountId</a>       </td><td> yes       </td><td> TheID of the withdraw account                                                                         </td></tr>
<tr><td> subAccountId     </td><td> <a href="../basic-types.md#subaccountid">SubAccountId</a>    </td><td> yes       </td><td> The ID of the withdraw subaccount                                                                     </td></tr>
<tr><td> to               </td><td> String          </td><td> yes       </td><td> The target address of the withdrawal                                                                  </td></tr>
<tr><td> l2SourceToken    </td><td> <a href="../basic-types.md#TokenId">TokenId</a>         </td><td> yes       </td><td> The source token to be deducted from the Layer3 account and used as the fee token                     </td></tr>
<tr><td> l1TargetToken    </td><td> <a href="../basic-types.md#TokenId">TokenId</a>         </td><td> yes       </td><td> The target token to be sent to the to_address on Layer1                                               </td></tr>
<tr><td> amount           </td><td> <a href="../../private-key-and-signature/encode/algorithm.md">BigUint</a> </td><td> yes       </td><td> Withdrawal amount, the value does not have to be packable                                             </td></tr>
<tr><td> fee              </td><td> <a href="../../private-key-and-signature/encode/algorithm.md">BigUint</a> </td><td> yes       </td><td> Fee requested via <code>estimateTransactionFee</code> API, the value should be packable               </td></tr>
<tr><td> withdrawToL1     </td><td> u8              </td><td> yes       </td><td> 1: true, 0: false. Withdraw to L1 or not                                                              </td></tr>
<tr><td> withdrawFeeRatio </td><td> u16             </td><td> yes       </td><td> Transaction fee for fast withdraw, 100 as 1%, 10000 as 100%, If ratio is not zero means fast withdraw, Value range: [0, 10000] </td></tr>
<tr><td> ts               </td><td> u32             </td><td> yes       </td><td> Timestamp of the API call, used as front-end request id to generate transaction hash                  </td></tr>
<tr><td> nonce            </td><td> <a href="../basic-types.md#nonce">Nonce</a>           </td><td> yes       </td><td> Current nonce of the account                                                                          </td></tr>
<tr><td> signature        </td><td> <a href="../basic-types.md#zklinksignature">ZkLinkSignature</a> </td><td> yes       </td><td> the public key hash corresponding to the signature must be aligned with the withdraw account          </td></tr>
</tbody>
</table>

For Example:
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
  "withdrawToL1": 0,
  "withdrawFeeRatio": 50,
  "ts": 1646102148,
  "nonce": 0,
  "signature": {
    "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
  }
}
```

## sign Withdraw

{% tabs %}
{% tab title="Golang" %}
```go
import (
	"math/big"
	"fmt"
	"time"
	sdk "github.com/zkLinkProtocol/zklink_sdk/go_example/generated/uniffi/zklink_sdk"
)

func SignWithdraw() {
    privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
	accountId := sdk.AccountId(8300)
	subAccountId := sdk.SubAccountId(4)
	toChainId := sdk.ChainId(1)
    toAddress := sdk.ZkLinkAddress("0xAFAFf3aD1a0425D792432D9eCD1c3e26Ef2C42E9")
    l3SourceToken := sdk.TokenId(6)
    l1TargetToken := sdk.TokenId(5)
	amount := *big.NewInt(1000000)
	fee := *big.NewInt(1000)
	nonce := sdk.Nonce(1)
	withdrawFeeRatio := uint16(50)
    // get current timestamp
    now := time.Now()
    timestamp := sdk.TimeStamp(now.Unix())
    builder := sdk.WithdrawBuilder{
        AccountId: accountId,
        ToChainId: toChainId,
        SubAccountId: subAccountId,
        ToAddress: toAddress,
        L2SourceToken: l3SourceToken,
        L1TargetToken: l1TargetToken,
        Amount: amount,
        Fee: fee,
        Nonce: nonce,
        WithdrawToL1: true,
        WithdrawFeeRatio: withdrawFeeRatio,
        Timestamp: timestamp,
    }
    tx := sdk.NewWithdraw(builder)
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignWithdraw(tx, "USDT")
    fmt.Println("L1 signature: %s", txSignature.Layer1Signature)
    fmt.Println("signed Tx: %s", txSignature.Tx)
    submitterSignature, err := signer.SubmitterSignature(txSignature.Tx)
    fmt.Println("submitter signature: %s, %s", submitterSignature.PubKey, submitterSignature.Signature)
}
```
For more details please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK

{% endtab %}
{% tab title="javascript" %}
```javascript
import init, *  as wasm  from "/path/to/zklink-sdk-web.js";

async function sign_withdraw() {
    await init();
    const to_address = "0x5505a8cD4594Dbf79d8C59C0Df1414AB871CA896";
    const ts  = Math.floor(Date.now() / 1000);
    try {

        let tx_builder = new wasm.WithdrawBuilder(10, 1, 1, to_address,18, "100000000000000", false,10,18,"10000000000000000", 1,ts);
        let withdraw = wasm.newWithdraw(tx_builder);
        let signer = new wasm.JsonRpcSigner();
        await signer.initZklinkSigner();
        let signature = await signer.signWithdraw(withdraw,"USDC")
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
For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)
{% endtab %}
{% endtabs %}
