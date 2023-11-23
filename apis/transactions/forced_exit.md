
Forced withdraw from Layer2

| Name                  | Type            | Mandatory | Description                                                                                   |
|-----------------------|-----------------|-----------|-----------------------------------------------------------------------------------------------|
| type                  | String          | yes       | The value is "ForcedExit"                                                                     |
| toChainId             | ChainId         | yes       | The target chain of the withdrawal                                                            |
| initiatorAccountId    | AccountId       | yes       | Account ID of the transaction initiator                                                       |
| initiatorSubAccountId | SubAccountId    | yes       | Subaccount ID of the transaction initiator                                                    |
| initiatorNonce        | Nonce           | yes       | Nonce of the transaction initiator's subaccount                                               |
| target                | String          | yes       | The account address of the forced withdraw, the token on Layer 1 is also sent to this address |
| targetSubAccountId    | AccountId       | yes       | Subaccount ID of the account of the forced withdraw                                           |
| l2SourceToken         | TokenId         | yes       | The token deducted from the account of the forced withdraw                                    |
| l1TargetToken         | TokenId         | yes       | This token sent to the to_address on L1                                                       |
| exitAmount            | BigUint         | yes       | Withdrawal amount                                                                             |
| withdrawToL1          | u8              | yes       | 1: true, 0: false. withdraw to L1 or not                                                      |
| ts                    | u32             | yes       | Timestamp of the API call, used as front-end request id to generate transaction hash          |
| signature             | ZkLinkSignature | yes       | the public key hash corresponding to the signature must be aligned with the initiator account |

For example: 

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
  "withdrawToL1": 0,
  "signature": {
    "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
  }
}
```

## sign ForcedExit

{% tabs %}
{% tab title="Golang" %}
```go
func SignForcedExit() {
    privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
    address := sdk.ZkLinkAddress("0xAFAFf3aD1a0425D792432D9eCD1c3e26Ef2C42E9")
    builder := sdk.ForcedExitBuilder{
        ToChainId: sdk.ChainId(1),
        InitiatorAccountId: sdk.AccountId(1),
        TargetSubAccountId: sdk.SubAccountId(1),
        Target: address,
        InitiatorSubAccountId: sdk.SubAccountId(1),
        L2SourceToken: sdk.TokenId(18),
        L1TargetToken: sdk.TokenId(18),
        InitiatorNonce: sdk.Nonce(1),
        ExitAmount: *big.NewInt(100000),
        WithdrawToL1: false,
        Timestamp: sdk.TimeStamp(1693472232),
    }
    tx := sdk.NewForcedExit(builder)
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignForcedExit(tx)
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
import init, *  as wasm  from "/path/to/zklink-sdk-web.js";

async function main() {
    await init();
    const to_address = "0x5505a8cD4594Dbf79d8C59C0Df1414AB871CA896";
    const ts  = Math.floor(Date.now() / 1000);
    try {
        let tx_builder = new wasm.ForcedExitBuilder(1,10, 1, 1, to_address,18, 18,"100000000000000",  1,ts);
        let forced_exit = wasm.newForcedExit(tx_builder);
        let signer = new wasm.JsonRpcSigner();
        await signer.initZklinkSigner();
        let signature = signer.signForcedExit(forced_exit)
        console.log(signature);

        let submitter_signature = signer.submitterSignature(signature.tx);
        console.log(submitter_signature);
        let rpc_client = new wasm.RpcClient("testnet");
        let tx_hash = await rpc_client.sendTransaction(signature.tx,null,submitter_signature);
        console.log(tx_hash);

    } catch (error) {
        console.error(error);
    }

}
```

For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)

{% endtab %}

{% endtabs %}
