Modifies the public key hash of the Layer3 account.

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> type         </td><td> String                                        </td><td> yes       </td><td> The value is "ChangePubKey"                                                                                                                                         </td></tr>
<tr><td> chainId      </td><td> <a href="../data_types.md#chainid">ChainId</a>                                       </td><td> yes       </td><td> ID defined by zkLink, for example, when the user performs ChangePubKey on ETH, the front-end needs to set this value to the Ethereum ID defined by zkLink on Layer3 </td></tr>
<tr><td> accountId    </td><td> <a href="../data_types.md#accountid">AccountId</a>                                     </td><td> yes       </td><td> Target account ID of ChangePubKey                                                                                                                                   </td></tr>
<tr><td> subAccountId </td><td> <a href="../data_types.md#subaccount">SubAccountId</a>                                  </td><td> yes       </td><td> Target subaccount ID of ChangePubKey, the fee will be deducted from this subaccount                                                                                 </td></tr>
<tr><td> newPkHash    </td><td> <a href="../data_types.md#pubkeyhash">PubkeyHash</a>                                    </td><td> yes       </td><td> New public key hash                                                                                                                                                 </td></tr>
<tr><td> nonce        </td><td> <a href="../data_types.md#nonce">Nonce</a>                                         </td><td> yes       </td><td> Current nonce of the target account </td></tr>
<tr><td> feeToken     </td><td> <a href="../data_types.md#tokenid">TokenId</a>                                       </td><td> yes       </td><td> The token used as the fee token</td></tr>
<tr><td> fee          </td><td> BigUint                                       </td><td> yes       </td><td> Fee obtained via <code>estimateTransactionFee</code> API, the value should be packable </td></tr>
<tr><td> ethAuthData  </td><td> <a name="changepubkeyauthdata" href="#">ChangePubKeyAuthData</a>  </td><td> yes       </td><td> ChangePubKeyAuthData to set the public key </td></tr>
<tr><td> signature    </td><td> <a href="../data_types.md#zklinksignature">ZkLinkSignatur</a>                                </td><td> yes       </td><td>  the public key hash corresponding to the signature must be aligned with the newPkHash </td></tr>
<tr><td> ts           </td><td> u32                                           </td><td> yes       </td><td> Timestamp of the API call, used as front-end request id to generate transaction hash </td></tr>
</tbody>
</table>

where the <a id="changepubkeyauthdata">ChangePubKeyAuthData</a> is an Enum which contains 3 types: `EthECDSA`, `EthCreate2`, `Onchain`

{% tabs %}
{% tab title="EthECDSA" %}

| Name         | Type   | Required | Description                   |
|--------------|--------|-----------|-------------------------------|
| type         | String | yes       | The value is "EthECDSA"        |
| ethSignature | String | yes       | eth signature with `0x` prefix |

Refer to [EIP712](https://eips.ethereum.org/EIPS/eip-712) to create the signature content in this way, where the domain is:

```json
{
  "name":"ZkLink", // constaint
  "version":"1", // constaint
  "chainId":13, // this this the L1 chain id, not the one defined by zkLink
  "verifyingContract":"0x388c818ca8b9251b393131c08a736a67ccb19297" // this is the zkLink contract address on L1
}
```

> Note: Different chain has different `chainId` and `verifyingContract`, you can get the chain information from rpc interface `getSupportChains`.

For Example:

```json
{
  "type": "ChangePubKey",
  "chainId": 1,
  "accountId": 39,
  "subAccountId": 1,
  "newPkHash": "0xbfb4f4a68dc9e49f7785082a8c12354ed663b6e0",
  "feeToken": 1,
  "fee": "1285000000000000",
  "nonce": 0,
  "signature": {
    "pubKey": "0xed53a138751ed1e456f46e74eff3463d2420e488a4f608bde0f28d13c7104d29",
    "signature": "3b91c0421df4295281596746722ae20ccf270c5fc0561f93a0219db1faea6518f033e778dd552f90a9a6afd06427428b2ac4ea6f6893a3f162b32683d1108a02"
  },
  "ethAuthData": {
    "type": "EthECDSA",
    "ethSignature": "0x8e548e3727a94533b3963877b87966e308e6eef7762f78de567ff14b4e0e87780d37a845501ffd2cdbc7d6f0d620c14589212761f1637ea8214b0b6bac10aa9b1b"
  },
  "ts": 1675650037
}
```

{% endtab %}

{% tab title="EthCreate2" %}

| Name           | Type   | Required | Description                                            |
|----------------|--------|-----------|--------------------------------------------------------|
| type           | String | yes       | The value is "EthCreate2"                              |
| creatorAddress | String | yes       | creator address                                        |
| saltArg        | String | yes       | The salt argument when create address, H256 hex string |
| codeHash       | String | yes       | code hash, H256 hex string                             |

For Example:

```json
{
  "type": "ChangePubKey",
  "chainId": 1,
  "accountId": 39,
  "subAccountId": 1,
  "newPkHash": "0xbfb4f4a68dc9e49f7785082a8c12354ed663b6e0",
  "feeToken": 1,
  "fee": "1285000000000000",
  "nonce": 0,
  "signature": {
    "pubKey": "0xed53a138751ed1e456f46e74eff3463d2420e488a4f608bde0f28d13c7104d29",
    "signature": "3b91c0421df4295281596746722ae20ccf270c5fc0561f93a0219db1faea6518f033e778dd552f90a9a6afd06427428b2ac4ea6f6893a3f162b32683d1108a02"
  },
  "ethAuthData": {
    "type":"EthCreate2",
    "creatorAddress":"0x388c818ca8b9251b393131c08a736a67ccb19297",
    "saltArg":"0x66b8e2fa879542a0c32c77e137ab830c3345788a2696999afbd07acabab8ad81",
    "codeHash":"0xfb57f5a066444ff426f4433344fd2e179843fd9069c06eb3b4ecc74e8b599410"
  },
  "ts": 1675650037
}
```

{% endtab %}

{% tab title="Onchain" %}

| Name | Type   | Required | Description            |
|------|--------|-----------|------------------------|
| type | String | yes       | The value is "Onchain" |

For Example:

```json
{
  "type": "ChangePubKey",
  "chainId": 1,
  "accountId": 39,
  "subAccountId": 1,
  "newPkHash": "0xbfb4f4a68dc9e49f7785082a8c12354ed663b6e0",
  "feeToken": 1,
  "fee": "1285000000000000",
  "nonce": 0,
  "signature": {
    "pubKey": "0xed53a138751ed1e456f46e74eff3463d2420e488a4f608bde0f28d13c7104d29",
    "signature": "3b91c0421df4295281596746722ae20ccf270c5fc0561f93a0219db1faea6518f033e778dd552f90a9a6afd06427428b2ac4ea6f6893a3f162b32683d1108a02"
  },
  "ethAuthData": {
      "type": "Onchain"
  },
  "ts": 1675650037
}
```

{% endtab %}

{% endtabs %}


## sign

{% tabs %}
{% tab title="Golang" %}

```go
import (
    time
    fmt
    sdk "github.com/zkLinkProtocol/zklink_sdk/generated/uniffi/zklink_sdk"
)

func SignChangePubkey {
    ethSignature := sdk.PackedEthSignature("0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001b")
    // get current timestamp
    now := time.Now()
    timeStamp := sdk.TimeStamp(now.Unix())
    mainContract := sdk.ZkLinkAddress("0x5505a8cD4594Dbf79d8C59C0Df1414AB871CA896")

    // create ChangePubKey transaction type without signed
    builder := sdk.ChangePubKeyBuilder{
		ChainId: sdk.ChainId(1),
		AccountId: sdk.AccountId(1),
		SubAccountId: sdk.SubAccountId(4),
		NewPubkeyHash: sdk.PubKeyHash("0xd8d5fb6a6caef06aa3dc2abdcdc240987e5330fe"),
		FeeToken: sdk.TokenId(1),
		Fee: *big.NewInt(100),
		Nonce: sdk.Nonce(100),
		EthSignature: &ethSignature,
		Timestamp: timeStamp,
    }
    tx := sdk.NewChangePubKey(builder)
    l1ClientId := uint32(1)
    privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignChangePubkeyWithEthEcdsaAuth(tx, l1ClientId, mainContract)
    fmt.Println("L1 signature: %s", txSignature.Layer1Signature)
    fmt.Println("signed Tx: %s", txSignature.Tx)
    submitterSignature, err := signer.SubmitterSignature(txSignature.Tx)
    fmt.Println("submitter signature: %s, %s", submitterSignature.PubKey, submitterSignature.Signature)
}
```

For more detail please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK

{% endtab %}

{% tab title="Javascript" %}
```js
import init, *  as wasm  from "/path/to/zklink-sdk-web.js";

async function testEcdsaAuth() {
    await init();
    const main_contract = "0x5505a8cD4594Dbf79d8C59C0Df1414AB871CA896";
    const l1_client_id = 80001;
    const new_pubkey_hash = "0xd8d5fb6a6caef06aa3dc2abdcdc240987e5330fe";
    const ts  = Math.floor(Date.now() / 1000);
    try {
        let tx_builder = new wasm.ChangePubKeyBuilder(
            1,5,1,new_pubkey_hash,18,"100000000000000",
            1,"0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001b",
            ts);
        let tx = wasm.newChangePubkey(tx_builder);
        const signer = new wasm.JsonRpcSigner();
        await signer.initZklinkSigner();
        let tx_signature = await signer.signChangePubkeyWithEthEcdsaAuth(tx,l1_client_id,main_contract);
        console.log(tx_signature);
        let submitter_signature = signer.submitterSignature(tx_signature.tx);
        console.log(submitter_signature);
        //send to zklink
        let rpc_client = new wasm.RpcClient("testnet");
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
