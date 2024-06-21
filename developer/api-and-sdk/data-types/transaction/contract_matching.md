<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> type         </td><td> String          </td><td> yes       </td><td> The value is "ContractMatching"                                                  </td></tr>
<tr><td> accountId    </td><td> <a href="../basic-types.md#accountid">AccountId</a>       </td><td> yes       </td><td> The account id                                                                   </td></tr>
<tr><td> subAccountId </td><td> <a href="../basic-types.md#subaccountid">SubAccountId</a>    </td><td> yes       </td><td> The subaccount id                                                                </td></tr>
<tr><td> maker        </td><td> Array of Contracts    </td><td> yes       </td><td> The maker list                                                                   </td></tr>
<tr><td> taker        </td><td> Contract    </td><td> yes       </td><td> The taker                                                                        </td></tr>
<tr><td> fee          </td><td> <a href="../../private-key-and-signature/encode/algorithm.md">BigUint</a>      </td><td> yes       </td><td> The fee amount of ContractMatching                                               </td></tr>
<tr><td> feeToken     </td><td> <a href="../basic-types.md#tokenid">TokenId</a>         </td><td> yes       </td><td> The token id of the fee                                                          </td></tr>
<tr><td> signature    </td><td> <a href="../basic-types.md#zklinksignature">ZkLinkSignature</a> </td><td> yes       </td><td> the pub key hash corresponding to the signature must be aligned with the account </td></tr>
</tbody>
</table>


where the `Contract` is the order in perpetual contract


<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> accountId    </td><td> <a href="../basic-types.md#accountid">AccountId</a>       </td><td> account id                                                                         </td></tr>
<tr><td> subAccountId </td><td> <a href="../basic-types.md#subaccountid">SubAccountId</a>  </td><td> sub account id                                                                     </td></tr>
<tr><td> slotId       </td><td> <a href="../basic-types.md#slotid">SlotId</a>            </td><td> slot id                                                                            </td></tr>
<tr><td> nonce        </td><td> Nonce</td><td> slot nonce, Value range: [0, 2^24 - 1]                                                              </td></tr>
<tr><td> pairId       </td><td> <a href="../basic-types.md#pairid">PairId</a>             </td><td> the pair id                                                                        </td></tr>
<tr><td> size         </td><td> <a href="../../private-key-and-signature/encode/algorithm.md">BigUint</a></td><td> position size                                                                      </td></tr>
<tr><td> price        </td><td> <a href="../basic-types.md#price">BigUint</a>                                    </td><td> price, the value can't be zero                             </td></tr>
<tr><td> direction    </td><td> u8                                            </td><td> 1: long, 0: short                                                                  </td></tr>
<tr><td> feeRates     </td><td> [u8, u8]                                      </td><td> The fee rates of [maker, taker], 100 means 1.00%, max is 2.56%                     </td></tr>
<tr><td> hasSubsidy   </td><td> u8                                            </td><td> 1: true, 0: false, if the maker has subsidy, the submitter will give maker subsidy </td></tr>
<tr><td> signature    </td><td> <a href="../basic-types.md#ZkLinkSignature">ZkLinkSignature</a>           </td><td> ZkLink L3 signature                                                                </td></tr>
</tbody>
</table>


For example:

```json
{
  "type": "ContractMatching",
  "accountId": 0,
  "subAccountId": 0,
  "maker": [
    {
      "accountId": 0,
      "subAccountId": 0,
      "slotId": 0,
      "nonce": 0,
      "pairId": 0,
      "size": "0",
      "price": "0",
      "direction": 0,
      "feeRates": [
        0,
        0
      ],
      "hasSubsidy": 0,
      "signature": {
        "pubKey": "0x99575738f142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
        "signature": "957826468392052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
      }
    }
  ],
  "taker": {
    "accountId": 0,
    "subAccountId": 0,
    "slotId": 0,
    "nonce": 0,
    "pairId": 0,
    "size": "0",
    "price": "0",
    "direction": 0,
    "feeRates": [
      0,
      0
    ],
    "hasSubsidy": 0,
    "signature": {
      "pubKey": "0x43cbec0bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
      "signature": "366e759d61a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
    }
  },
  "fee": "0",
  "feeToken": 0,
  "signature": {
    "pubKey": "0x1234567bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
    "signature": "1234567861a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
  }
}
```

## sign contractMatching

{% tabs %}
{% tab title="Golang" %}
```go
import (
    fmt
    sdk "github.com/zkLinkProtocol/zklink_sdk/generated/uniffi/zklink_sdk"
)

func SignContractMatching() {
    privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
    // create zklink signer
	zklinkSigner, err := sdk.ZkLinkSignerNewFromHexEthSigner(privateKey)
	if err != nil {
		return
	}
	taker_contract_builder := sdk.ContractBuilder {
        sdk.AccountId(1),
        sdk.SubAccountId(1),
        sdk.SlotId(2),
        sdk.Nonce(10),
        sdk.PairId(1),
        *big.NewInt(45454),
        *big.NewInt(113),
        true,
        5,
        3,
        false,
    }

    unsigned_taker_contract := sdk.NewContract(taker_contract_builder)
    taker_contract, err := unsigned_taker_contract.CreateSignedContract(
        zklinkSigner,
    )

    maker_contract1_builder := sdk.ContractBuilder {
        sdk.AccountId(3),
        sdk.SubAccountId(1),
        sdk.SlotId(2),
        sdk.Nonce(6),
        sdk.PairId(1),
        *big.NewInt(43434),
        *big.NewInt(6767),
        true,
        1,
        2,
        false,
    }

    maker_contract2_builder := sdk.ContractBuilder {
        sdk.AccountId(5),
        sdk.SubAccountId(1),
        sdk.SlotId(2),
        sdk.Nonce(100),
        sdk.PairId(1),
        *big.NewInt(45656),
        *big.NewInt(343),
        true,
        8,
        20,
        true,
    }

    unsigned_maker_contract1 := sdk.NewContract(maker_contract1_builder)
    unsigned_maker_contract2 := sdk.NewContract(maker_contract2_builder)
    maker_contract1, err := unsigned_maker_contract1.CreateSignedContract(
        zklinkSigner,
    )
    maker_contract2, err := unsigned_maker_contract2.CreateSignedContract(
        zklinkSigner,
    )

    var makers []*sdk.Contract
    makers = make([]*sdk.Contract,2)
    makers[0] = maker_contract1
    makers[1] = maker_contract2

    builder := sdk.ContractMatchingBuilder {
        sdk.AccountId(1),
        sdk.SubAccountId(1),
        taker_contract,
        makers,
       *big.NewInt(5545),
        sdk.TokenId(17),
    }

    tx := sdk.NewContractMatching(builder)
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignContractMatching(tx)
    if err != nil {
        return
    }
    fmt.Println("L1 signature: %s", txSignature.Layer1Signature)
    fmt.Println("signed Tx: %s", txSignature.Tx)
    submitterSignature, err := signer.SubmitterSignature(txSignature.Tx)
    fmt.Println("submitter signature: %s, %s", submitterSignature.PubKey, submitterSignature.Signature)

    // get submitter signature
    zklinkTx := tx.ToZklinkTx()
    submitterSignature, err := signer.SubmitterSignature(zklinkTx)
    fmt.Println("submitter signatur: %s, %s", submitterSignature.PubKey, submitterSignature.Signature)
}
```

For more detail please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK
{% endtab %}

{%tab title="javascript" %}

```javascript
const {ContractMatchingBuilder,Signer,newContractMatching,newContract,ContractBuilder,RpcClient } = require('./node-dist/zklink-sdk-node');

async function testContractMatching() {
    const private_key = "be725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4";
    try {
        const signer = new Signer(private_key);
        let taker_contract_builder = new ContractBuilder(5,1,1,3,2,
            "343434343434","5454545445",true,50,22,false);
        let unsigned_taker_contract = newContract(taker_contract_builder);
        let taker_contract = signer.createSignedContract(unsigned_taker_contract);
        console.log(taker_contract);

        let maker_contract_builder1 = new ContractBuilder(5,1,1,4,2,
            "556556","898989",false,50,22,true);
        let unsigned_maker_contract1 = newContract(maker_contract_builder1);
        let maker_contract1 = signer.createSignedContract(unsigned_maker_contract1);
        console.log(maker_contract1);

        let maker_contract_builder2 = new ContractBuilder(5,1,1,5,2,
            "54554","78787878",false,50,22,false);
        let unsigned_maker_contract2 = newContract(maker_contract_builder2);
        let maker_contract2 = signer.createSignedContract(unsigned_maker_contract2);
        console.log(maker_contract2);

        let tx_builder = new ContractMatchingBuilder(5,1,taker_contract,[maker_contract1,maker_contract2],"34343",17);
        let tx = newContractMatching(tx_builder);
        console.log(tx);
        let tx_signature = signer.signContractMatching(tx);
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
