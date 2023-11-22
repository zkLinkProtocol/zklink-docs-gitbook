## OrderMatching

Order Matching

| Name               | Type              | Mandatory | Description                                                                                                                                                                                                                  |
|--------------------|-------------------|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type               | String            | yes       | OrderMatching                                                                                                                                                                                                                |
| accountId          | AccountId         | yes       | Initiator's account id. Only specific accounts can initiate this type of transaction on Layer2                                                                                                                               |
| subAccountId       | SubAccountId      | yes       | Initiator's subaccount id                                                                                                                                                                                                    |
| taker              | Order             | yes       | taker order                                                                                                                                                                                                                  |
| maker              | Order             | yes       | maker order                                                                                                                                                                                                                  |
| feeToken           | TokenId           | yes       | Fee token, deducted from the initiator's subaccount                                                                                                                                                                          |
| fee                | BigUint           | yes       | Fee returned via the <code>estimateTransactionFee</code> API. The value should be packable                                                                                                                                   |
| expectBaseAmount   | BigUint           | yes       | The maximum amount of base token that the initiator expects to be traded in this order matching, which cannot exceed the maximum amount that the maker and taker can actually trade. The value does not need to be packable  |
| expectQuoteAmount  | BigUint           | yes       | The maximum amount of quote token that the initiator expects to be traded in this order matching, which cannot exceed the maximum amount that the maker and taker can actually trade. The value does not need to be packable |
| signature          | ZklinkSignature   | yes       | The pub key hash corresponding to the signature must be aligned with the initiator account                                                                                                                                       |

where the type `Order` is

| Name         | Type                                                | Mandatory | Description        |
|--------------|-----------------------------------------------------|-----------|--------------------|
| accountId    | [Accountid](./data_types.md#AccountId)             | yes       | The Account id     |
| subAccountId | [SubAccountId](./data_types.md#SubAccountId)       | yes       | The sub-account id |
| slotId       | [SlotId](./data_types.md#SlotId)                   | yes       | slot id            |
| nonce        | [Nonce](./data_types.md#Nonce)                     | yes       | slot nonce         |
| baseTokenId  | [TokenId](./data_types.md#TokenId)                 | yes       | the base token, for example `BTC` in BTC/USDT pair                             |
| quoteTokenId | [TokenId](./data_types.md#TokenId)                 | yes       | The quote token, for example `USDT` in BTC/USDT pair                           |
| amount       | String                                              | yes       | The string format of BigUint, the amount request in this order                 |
| price        | String                                              | yes       | The string format of BigUint, the price request in this order                  |
| isSell       | u8                                                  | yes       | 1:seller, 0: buyer                                                             |
| feeRates     | [u8, u8]                                            | yes       | the fee of [maker, taker], 100 means 1.0%, the maximum 2.56%                   |
| hasSubsidy   | u8                                                  | yes       | 1: true, 0: false. If maker has subsidy, the submitter will give maker subsidy |
| signature    | [ZkLinkSignature](./data_types.md#ZkLinkSignature) | yes       | The ZkLink signature of this order |

For example:

```json
{
  "type": "OrderMatching",
  "accountId": 4,
  "subAccountId": 1,
  "taker": {
    "accountId": 11,
    "subAccountId": 1,
    "slotId": 5844,
    "nonce": 24,
    "baseTokenId": 42,
    "quoteTokenId": 1,
    "amount": "373400000000000000000",
    "price": "1210900000000000000",
    "isSell": 1,
    "feeRates": [5, 10],
    "signature": {
      "pubKey": "0x1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
      "signature": "3a5c5c23a74cc04d256f03eb1671acd3959d707252ed52b16cb7b2ffe332a804986e09e0d62bcf49fc9231b38f07b71199769a7343eddc8b43ed9dd2ef8a4405"
    }
  },
  "maker": {
    "accountId": 11,
    "subAccountId": 1,
    "slotId": 5915,
    "nonce": 14,
    "baseTokenId": 42,
    "quoteTokenId": 1,
    "amount": "5165400000000000000000",
    "price": "1210900000000000000",
    "isSell": 0,
    "feeRates": [5, 10],
    "signature": {
      "pubKey": "0x1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
      "signature": "9579e54f53aa709e72c7e4de9815d258cf92bb3e9c4b9d03c2f79a7a49b5bda062d7e81b278eb62f2452294d178728b460efdb80017c83748dd190e41e05b802"
    }
  },
  "fee": "405000000000000",
  "feeToken": 1,
  "expectBaseAmount": "373400000000000000000",
  "expectQuoteAmount": "452150060000000000000",
  "signature": {
    "pubKey": "0x84bf4edbe1f7056f079ba4c38359427f43d529fbab2e94e6d6b7a18efbf2fb87",
    "signature": "1242830780b17dd362e8d31952deab6d8b5d81cadd62779b2caab0821baa030a770afa9a2c249d8f45000c4b9c6f01ef6b002682760e5bc4e5d39ef7f511ce03"
  }
}
```

## Sign OrderMatching

{% tabs %}
{% tab title = "Golang" %}

```golang
import (
	"math/big"
	"fmt"
	sdk "github.com/zkLinkProtocol/zklink_sdk/go_example/generated/uniffi/zklink_sdk"
)

func SignOrderMatching() {
    privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
    // create zklink signer
	zklinkSigner, err := sdk.ZkLinkSignerNewFromHexEthSigner(privateKey)
	if err != nil {
		return
	}
    taker := sdk.NewOrder(
        sdk.AccountId(1),
        sdk.SubAccountId(1),
        sdk.SlotId(3),
        sdk.Nonce(1),
        sdk.TokenId(18),
        sdk.TokenId(145),
        *big.NewInt(323289),
        *big.NewInt(135),
        true,
        2,
        5,
        nil,
    )
    taker, err = sdk.CreateSignedOrder(
        zklinkSigner,
        taker,
    )

    maker := sdk.NewOrder(
         sdk.AccountId(2),
         sdk.SubAccountId(1),
         sdk.SlotId(3),
         sdk.Nonce(1),
         sdk.TokenId(18),
         sdk.TokenId(145),
         *big.NewInt(323355),
         *big.NewInt(135),
         false,
         2,
         5,
         nil,
    )
    maker, err = sdk.CreateSignedOrder(
        zklinkSigner,
        maker,
    )

    builder := sdk.OrderMatchingBuilder{
        sdk.AccountId(3),
        sdk.SubAccountId(1),
        taker,
        maker,
        *big.NewInt(1000),
        sdk.TokenId(18),
        *big.NewInt(808077878),
        *big.NewInt(5479779),
    }
    tx := sdk.NewOrderMatching(builder)
    signer, err := sdk.NewSigner(privateKey)
    if err != nil {
        return
    }
    txSignature, err := signer.SignOrderMatching(tx)
    fmt.Println("L1 signature: %s", txSignature.Layer1Signature)
    fmt.Println("signed Tx: %s", txSignature.Tx)
    submitterSignature, err := signer.SubmitterSignature(txSignature.Tx)
    fmt.Println("submitter signature: %s, %s", submitterSignature.PubKey, submitterSignature.Signature)
}
```
For more detail please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK
{% endtab %}

{ % tab title = "javascript" %}
```javascript
import init, *  as wasm  from "/path/to/zklink-sdk-web.js";

async function main() {
    await init();
    try {
        let signer = new wasm.JsonRpcSigner();
        await signer.initZklinkSigner();
        let maker = new wasm.Order(5,1,1,1,18,17,"10000000000000","10000000000",true,5,3);
        let signed_maker = signer.createSignedOrder(maker);
        console.log(signed_maker);
        let taker = new wasm.Order(5,1,1,1,18,17,"10000000000000","10000000000",false,5,3);
        let signed_taker = signer.createSignedOrder(taker_order);
        console.log(signed_taker);
        let tx_builder = new wasm.OrderMatchingBuilder(10, 1, signed_taker, signed_maker, "1000000000", 18,"10000000000000000", "10000000000000000");
        let order_matching = wasm.newOrderMatching(tx_builder);
        let signature = signer.signOrderMatching(order_matching);
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

main();
```

{% endtab %}

{% endtabs %}