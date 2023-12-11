# Quick Start
It's great to hear that you are eager to dive into creating a DEX and exploring the zkLink SDK. It is essential to follow the suggested order of topics and make sure to have a solid understanding of zkLink's architecture by reading the overview beforehand. Additionally, to receive notifications from zkLink, you will need to prepare the WebSocket.

Assuming there are two traders, Alice and Bob, the steps to be covered include:

Depositing and creating accounts for Alice and Bob
Alice using USDT to buy BTC from Bob
Alice withdrawing half of the BTC she just acquired
Alice transferring the remaining BTC to another subaccount
I can assist you with any specific questions or details related to these steps. Feel free to ask!


In this quick start, you'll lean how to create a DEX and take a short tour through the zkLink SDK. 
It is essential to follow the suggested order of topics and make sure to have a solid understanding of zkLink's architecture by reading the [overview](../overview.md) beforehand.
Additionally, to receive notifications from zkLink server, you will need to prepare the WebSocket.

Assuming there are two traders, Alice and Bob, the steps to be covered include:

* Creating accounts for Bob and Alice
* Alice deposits coin1 and Bob deposit coin2
* Alice using coin1(id=18) to buy coin2(id=19) from Bob
* Alice withdrawing half of the coin2 she just acquired

# Create Account and Deposit
zkLink will create a user account when the user makes their first deposit. The DEX will not take any action except for receiving the user account creation event and the user balance update event from the websocket. For instance, when Alice makes her first deposit, you will receive the following data:
```json
{
  "type":"TxExecuteResult",
  "txHash":"0x05f2cf78d7b7a4a6b9f3c7e0c78d0c3a97438b9806e21834ee36530eb62d30fd",
  "tx":{
    "type":"Deposit",
    "from":"0x76ed7d63d9266f07ec86d44237daca3637a6650d",
    "to":"0xd81418a80a0df6feaea04467f908bc1cb1fc5be7",
    "fromChainId":7,
    "subAccountId":2,
    "l1SourceToken":17,
    "l2TargetToken":17,
    "amount":"1000000000000000000000000000",
    "serialId":26,
    "l2Hash":"0xa0f46cc2c2ee1480350c4b1c2a3b1b70a55f799078ae1a248ed6cf71431e0270",
    "ethHash":null
  },
  "receipt":{
    "executed":true,
    "executedTimestamp":1702099337833969,
    "success":true,
    "failReason":null,
    "block":null,
    "index":null
  },
  "updates":[
    // As the to_account (accountId is 1) does not exist, an AccountCreate is involved
    {
      "type":"AccountCreate",
      "updateId":0,
      "accountId":11,
      "address":"0xd81418a80a0df6feaea04467f908bc1cb1fc5be7"
    },
    // The balance of the to_account increased, depositAmount=newBalance-oldBalance
    {
      "type":"BalanceUpdate",
      "updateId":1,
      "accountId":11,
      "subAccountId":0,
      "coinId":18,
      "oldBalance":"0",
      "newBalance":"1000000000000000000000000000",
      "oldNonce":0,
      "newNonce":0
    },
    // The 'global asset' account records the increase of on-chain asset reserves
    {
      "type": "BalanceUpdate",
      "updateId": 17,
      "accountId": 1,
      "subAccountId": 2,
      "coinId": 18,
      "oldBalance": "3030000000000000000000",
      "newBalance": "3040000000000000000000",
      "oldNonce": 0,
      "newNonce": 0
    }
  ]
}
```
Now, you can maintain a new account with the account ID '31' with a balance '1000000000000000000000000000' of a coin ID (or token ID) of '18'. Similarly, and also same of Bob’s account information. For example:

| name  | accountId | subAccountId | coin id | balance              |
|-------|-----------|---|---------|----------------------|
| Alice | 31        | 0 | 18      | 10000000000000000000 |
| Bob   | 32        | 0 | 19      | 10000000             |

To obtain the token ID information, you will need to query the RPC [getSupportTokens](../api-and-sdk/json-rpc/json-rpc-api.md#getsupporttokens). For instance, you can retrieve the 'ZKL' token information from the RPC response.


```json
{
    "jsonrpc": "2.0",
    "result": {
        "1": {
            "id": 1,
            "symbol": "ZKL",
          	"usdPrice": "12.1",
            "chains": {
                "1": {
                    "chainId": 1,
                    "address": "0x1aef2b4c06b83cdb2783d3458cdbf3886a6ae7d4",
                  	"decimals": 18,
                    "fastWithdraw": false
                },
                "2": {
                    "chainId": 2,
                    "address": "0xa684f63605ccdedbce7e2e7e3fa06441758da6d1",
                  	"decimals": 10,
                    "fastWithdraw": true
                }
            }
        }
    },
    "id": 1
}
```

# Place Order

| name  | accountId | subAccountId | coin id | balance              |
|-------|-----------|---|---------|----------------------|
| Alice | 31        | 0 | 18      | 10000000000000000000 |
| Bob   | 32        | 0 | 19      | 10000000             |

Next, Alice and Bob place orders, and the DEX will match them and send them to zkLink. Prior to users placing orders, you need to ensure the trade pair price is prepared. You can accomplish this by obtaining oracle.

Afterward, Alice and Bob need to create an order and sign the order with the BitKeep wallet or Metamask wallet, and then send it to the DEX server.

Bob, as the maker, will create the maker order:

```js
import init, *  as wasm  from "./web-dist/zklink-sdk-web.js";

async create_maker_order() {
    await init();
    //use stand window.ethereum as metamask ..
    //await window.ethereum.request({ method: 'eth_requestAccounts' });
    //const provider = window.ethereum;
    const provider = window.bitkeep && window.bitkeep.ethereum;
    await provider.request({ method: 'eth_requestAccounts' });
    const signer = new wasm.JsonRpcSigner(provider);
    await signer.initZklinkSigner(null);
    console.log(signer);
    let account_id = 31;
    let sub_account_id = 0;
    // get the user nonce from rpc `getUser`
    let nonce = 1;
    // get the slot id from rpc `getAccountOrderSlots`
    let slot_id = 1;
    let base_token_id = 18;
    let quote_token_id = 19;
    let amount = "5000000";
    let price = "1000000000000";
    let is_sell = true;
    let maker_fee_ratio = 5;
    let taker_fee_ratio = 1;
    let maker_order = new wasm.Order(account_id, sub_account_id, slot_id, nocne, base_token_id,quote_token_id, amount, price, is_sell, maker_fee_ratio, taker_fee_ratio);
    let signed_order = signer.createSignedOrder(maker_order);
    console.log(signed_order);
}        
```

And Alice, as the taker, will create the taker order:

```js
    let account_id = 32;
    let sub_account_id = 0;
    // get the user nonce from rpc `getUser`
    let nonce = 1;
    // get the slot id from rpc `getAccountOrderSlots`
    let slot_id = 1;
    let base_token_id = 18;
    let quote_token_id = 19;
    let amount = "5000000";
    let price = "1000000000000";
    let is_sell = false;
    let maker_fee_ratio = 5;
    let taker_fee_ratio = 1;
    let taker_order = new wasm.Order(account_id, sub_account_id, slot_id, nocne, base_token_id,quote_token_id, amount, price, is_sell, maker_fee_ratio, taker_fee_ratio);
    let signed_order = signer.createSignedOrder(maker_order);
    let taker = signer.createSignedOrder(taker_order);
    console.log(taker);
```

You can obtain the user's Nonce and SlotId by using the RPC [getUser](../api-and-sdk/json-rpc/json-rpc-api.md#getAccount) and [getAccountOrderSlots](../api-and-sdk/json-rpc/json-rpc-api.md#getaccountorderslots) respectively.

# Prepare submitter private key

Before creating the order matching transaction, you will need to generate a submitter private key. This key will allow you to sign the OrderMatching transaction. Additionally, it's essential to notify zkLink to add the submitter public key to the whitelist. By doing so, zkLink will be able to verify the transactions' signatures from the DEX and reject those whose submitter public keys are not on the whitelist.

# Create OrderMatching Transaction
Once the taker order and the maker order are matched in the order matching engine, you can create the OrderMatching transaction. Sign the transaction with the submitter private key and then send it to zkLink using the [sendTransaction](../api-and-sdk/json-rpc/json-rpc-api.md#sendtransaction) RPC interface.

```go
    var contractPrices = make([]sdk.ContractPrice,2)
    contractPrices[0] = sdk.ContractPrice{
        sdk.PairId(1),
        *big.NewInt(1000000000000),
    }
    contractPrices[1] = sdk.ContractPrice{
        sdk.PairId(3),
        *big.NewInt(52552131),
    }

    var marginPrices = make([]sdk.SpotPriceInfo,2)
    marginPrices[0] = sdk.SpotPriceInfo {
       sdk.TokenId(18),
       *big.NewInt(10000000),
    }
    marginPrices[1] = sdk.SpotPriceInfo {
      sdk.TokenId(19),
      *big.NewInt(10000000),
    }

    builder := sdk.OrderMatchingBuilder {
        sdk.AccountId(0),
        sdk.SubAccountId(1),
        taker,
        maker,
        
        *big.NewInt(1000),
        sdk.TokenId(18),
        *big.NewInt(808077878),
        *big.NewInt(5479779),
    }
    tx := sdk.NewOrderMatching(builder)
    signer, err := sdk.NewSigner(privateKey, sdk.L1TypeEth)
    if err != nil {
        return
    }
    txSignature, err := signer.SignOrderMatching(tx)
    if err != nil {
        return
    }
    fmt.Println("tx signature: %s", txSignature)

    // get submitter signature
    zklinkTx := tx.ToZklinkTx()
    submitterSignature, err := signer.SubmitterSignature(zklinkTx)
    submitterSignature2, err := json.Marshal(SubmiterSignature {
        PubKey: submitterSignature.PubKey,
        Signature: submitterSignature.Signature,
    })
	request := RPCTransaction {
		Id:      1,
		JsonRpc: "2.0",
		Method:  "sendTransaction",
		Params: []json.RawMessage{
		    []byte(txSignature.Tx),
		    nil,
		    submitterSignature2,
		},
    }
	JsonTx, err := json.Marshal(request)
	fmt.Println("ChangePubKey rpc request:",  string(JsonTx))
	zklinkUrl := sdk.ZklinkTestNetUrl()
	response, err := http.Post(zklinkUrl, "application/json", bytes.NewBuffer(JsonTx))
	if err != nil {
        fmt.Println(err)
    }
    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)
    fmt.Println(string(body))
```

After sending the transaction to zkLink, you can receive the transaction state updates through the [WebSocket](../api-and-sdk/websocket).

```js
"updates": [
    // Orderslot of the maker
    {
        "type": "OrderUpdate",
        "updateId": 0,
        "accountId": 31,
        "subAccountId": 0,
        "slotId": 1,
        "oldTidyOrder": {
            "nonce": 1,
            "residue": "54800000000000000"
        },
        "newTidyOrder": {
            "nonce": 2,
            "residue": "0"
        }
    },
    // The balance of token0 in the maker account decreased
    {
        "type": "BalanceUpdate",
        "updateId": 1,
        "accountId": 31,
        "subAccountId": 0,
        "coinId": 18,
        "oldBalance": "10000000000000000000",
        "newBalance": "5000000000000000000",
        "oldNonce": 1,
        "newNonce": 1
    },
    // The balance of token1 in the maker account increased
    {
        "type": "BalanceUpdate",
        "updateId": 2,
        "accountId": 31,
        "subAccountId": 1,
        "coinId": 19,
        "oldBalance": "0",
        "newBalance": "5000000",
        "oldNonce": 1,
        "newNonce": 1
    },
    // Orderslot of the taker
    {
        "type": "OrderUpdate",
        "updateId": 3,
        "accountId": 32,
        "subAccountId": 0,
        "slotId": 163,
        "oldTidyOrder": {
            "nonce": 0,
            "residue": "1467400000000000000"
        },
        "newTidyOrder": {
            "nonce": 0,
            "residue": "1412600000000000000"
        }
    },
    // The balance of token1 in the taker account decreased
    {
        "type": "BalanceUpdate",
        "updateId": 4,
        "accountId": 32,
        "subAccountId": 1,
        "coinId": 19,
        "oldBalance": "10000000",
        "newBalance": "5000000",
        "oldNonce": 1,
        "newNonce": 1
    },
    // The balance of token0 in the taker account increased
    {
        "type": "BalanceUpdate",
        "updateId": 5,
        "accountId": 32,
        "subAccountId": 0,
        "coinId": 18,
        "oldBalance": "0",
        "newBalance": "10000000000000000000",
        "oldNonce": 1,
        "newNonce": 1
    },
    // The submitter balance of token0 decreased due to transaction fees
    {
        "type": "BalanceUpdate",
        "updateId": 6,
        "accountId": 6,
        "subAccountId": 1,
        "coinId": 18,
        "oldBalance": "9999680162000000000000",
        "newBalance": "9999679771000000000000",
        "oldNonce": 224,
        "newNonce": 224
    },
    // The submitter collect token0 as transaction fee
    {
        "type": "BalanceUpdate",
        "updateId": 7,
        "accountId": 6,
        "subAccountId": 0,
        "coinId": 18,
        "oldBalance": "3529001244641983488661500000",
        "newBalance": "3529001244727972908661500000",
        "oldNonce": 224,
        "newNonce": 224
    },
    // The submitter collect token1 as transaction fee
    {
        "type": "BalanceUpdate",
        "updateId": 8,
        "accountId": 6,
        "subAccountId": 0,
        "coinId": 19,
        "oldBalance": "1594000000063937300000000000",
        "newBalance": "1594000000063964700000000000",
        "oldNonce": 224,
        "newNonce": 224
    },
    // The balance of the fee account increased
    {
        "type": "BalanceUpdate",
        "updateId": 9,
        "accountId": 0,
        "subAccountId": 0,
        "coinId": 18,
        "oldBalance": "483683000000000000",
        "newBalance": "484074000000000000",
        "oldNonce": 0,
        "newNonce": 0
    }
]
```

From the state updates, you can verify that the transaction has been successfully executed. Subsequently, you can check the users' updated balance:

| name  | accountId | subAccountId | coin id | balance             |
|-------|-----------|---|---------|---------------------|
| Alice | 31        | 0 | 18      | 5000000000000000000 |
| Alice | 32        | 0 | 19      | 5000000             |
| Bob   | 31        | 0 | 18      | 5000000000000000000 |
| Bob   | 32        | 0 | 19      | 5000000             |
