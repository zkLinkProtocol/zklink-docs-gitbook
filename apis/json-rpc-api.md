# JSON-RPC API

dApps can get account status, send transactions, and call other functions via zkLink API.

The zkLink API follows the JSON-RPC standard and is accessed via POST.

## API OVERVIEW

{% tabs %}
{% tab title="Networks" %}
* [getSupportChains](json-rpc-api.md#getsupportchains): get the configuration of all supported chains.
* [getSupportTokens](json-rpc-api.md#getsupporttokens): get the data of all tokens with on-chain contract addresses.
* [getLatestBlockNumber](json-rpc-api.md#getlatestblocknumber): get the latest block height info.
* [getTokenReserve](json-rpc-api.md#gettokenreserve): get the withdrawable limit of a token on a certain L1 chain.
{% endtab %}

{% tab title="Block" %}
* [getBlockByNumber](json-rpc-api.md#getblockbynumber): get the block info by block height.
* [getBlockOnChainNumber](json-rpc-api.md#getblockonchainnumber): get the transaction information in a block that is executed on L1 blockchains.
{% endtab %}

{% tab title="Account" %}
* [getAccount](json-rpc-api.md#getaccount): get account info by address or account id.
* [getAccountBalances](json-rpc-api.md#getaccountbalances): get the balance info of an account.
* [getAccountOrderSlots](json-rpc-api.md#getaccountorderslots): get the order slot of an account.
* [getAccountSnapshot](json-rpc-api.md#getaccountsnapshot): get the account states of a certain block height, including the basic info, balance info, and order slot info.
{% endtab %}

{% tab title="Transaction" %}
* [getTransactionByHash](json-rpc-api.md#gettransactionbyhash): get transaction info.
* [getAccountTransactionHistory](json-rpc-api.md#getaccounttransactionhistory): get account history in descending order of the transaction id in the database.
* [getFastWithdrawTxs](json-rpc-api.md#getfastwithdrawtxs): get transaction info of fast\_withdraw transactions.
* [sendTransaction](json-rpc-api.md#sendtransaction): submit L2 transaction and return transaction hash.
{% endtab %}
{% endtabs %}

## JSON-RPC API METHODS <a href="#json-rpc-methods" id="json-rpc-methods"></a>

### getSupportChains

Get the configuration of all supported chains.

{% tabs %}
{% tab title="Request" %}
**Parameters**

None

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getSupportChains",
    "params": []
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`ChainResp`

```json
{
    "jsonrpc": "2.0",
    "result": [
      {
        "chainId":1,
        "chainType": 0,
        "layerOneChainId":1001,
        "mainContract":"0x03855e8120691526f82085453AEefac3f6A484F1",
        "layerZeroContract": "0xcb45b54BA16fBdc7092E56938225a11799eb1124",
        "web3Url": "https://rpc-mumbai.maticvigil.com",
        "gasTokenId": 33,
        "validator": "0x526212fbd41080b455ae81014b5b6bf859c30094"
      }
    ],
    "id": 1
}
```
{% endtab %}
{% endtabs %}

**ChainResp**

<table><thead><tr><th width="225.21167883211677">Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>chainId</td><td>uint32</td><td>defined by zkLink</td></tr><tr><td>chainType</td><td>uint8</td><td>defined by zkLink {0:"EVM", 1:"STARKNET"}</td></tr><tr><td>layerOneChainId</td><td>uint32</td><td>the chain ID of L1 blockchains</td></tr><tr><td>mainContract</td><td>address(EVM|StarkNet)</td><td>the address of zkLink main contract that interacts with the rest module contracts</td></tr><tr><td>layerZeroContract</td><td>address(EVM|StarkNet)</td><td>the address of layerZero bridge that syncs L1 states</td></tr><tr><td>web3Url</td><td>string</td><td>the url of L1 RPC</td></tr><tr><td>gasTokenId</td><td>uint32</td><td>the id of the gas token, defined by zkLink</td></tr><tr><td>validator</td><td>address(EVM|StarkNet)</td><td>validator address</td></tr></tbody></table>

### getSupportTokens

Get the data of all tokens with on-chain contract addresses.

{% tabs %}
{% tab title="Request" %}
**Parameters**

None

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getSupportTokens",
    "params": []
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`HashMap<TokenId,TokenResp>`

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
{% endtab %}
{% endtabs %}

It returns a `HashMap<TokenId,TokenResp>`, which has a unique token id and is used for all token-related queries in the RPC service. Symbols are used only for display on the UI and do not promise uniqueness.

#### **TokenResp**

<table><thead><tr><th width="259.07751937984494">Field</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>token id</td></tr><tr><td>symbol</td><td>token symbol</td></tr><tr><td>usdPrice</td><td>token price</td></tr><tr><td>chains</td><td><code>HashMap&#x3C;ChainId,ChainTokenResp></code> that includes the contract addresses on each chain</td></tr></tbody></table>

#### **ChainTokenResp**

<table><thead><tr><th width="255.19691119691117">Field</th><th>Description</th></tr></thead><tbody><tr><td>chainId</td><td>defined by zkLink</td></tr><tr><td>address</td><td>the on-chain contract address of the token</td></tr><tr><td>decimals</td><td>the decimals of the token on L1</td></tr><tr><td>fastWithdraw</td><td>whether the token supports fast withdraw</td></tr></tbody></table>

The decimal is the accuracy of the token on L1, which is not always 18 (6 for USDC and 18 for ZKL), and varies on different chains for the same token (6 for USDC on Ethereum and 18 for USDC on BSC).

When a user deposits to zkLink from connected networks, the front-end needs to calculate the amount required for calling the contract according to the accuracy:

```json
var amount_of_user_input = 1.0 // input from the user
var amount_to_call_contract = amount_of_user_input * 10 ** token.decimals // the parameter during calling the contract
// Example
// A user deposits 2 USDC, the parameter is 2 * 10^6
// A user deposits 5 ZKL, the parameter is 5 * 10^18
```

### getLatestBlockNumber

Get the latest block height info.

{% tabs %}
{% tab title="Request" %}
**Parameters**

None

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getLatestBlockNumber",
    "params": []
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`BlockNumberResp`

```json
{
    "jsonrpc": "2.0",
    "result": {
        "lastBlockNumber": 2,
        "timestamp": 1663488014,
        "committed": 1,
        "verified": 0 
    },
    "id": 1
}
```
{% endtab %}
{% endtabs %}

#### **BlockNumberResp**

<table><thead><tr><th width="234.55555555555554">Field</th><th>Description</th></tr></thead><tbody><tr><td>lastBlockNumber</td><td>the block height of the latest batch</td></tr><tr><td>timestamp</td><td>the timestamp of the last block</td></tr><tr><td>committed</td><td>the block height of the latest block committed to L1</td></tr><tr><td>verified</td><td>the block height of the latest block executed on L1</td></tr></tbody></table>

### getBlockByNumber

Get the block info by block height.

{% tabs %}
{% tab title="Request" %}
**Parameters**

* `blockNumber`: the block height, and returns the latest block height if null
* `includeTx` : whether contains transaction details: returns transaction hash if false. only successful transactions will be included in a block. call `getTransactionByHash` to query the failed transactions.
* `includeUpdate`: whether contains the state change by transactions; valid only when `includeTx` is true.

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getBlockByNumber",
    "params": [
      123, 
      true, 
      true 
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`BlockResp`

```json
{
  "jsonrpc": "2.0",
  "result": {
    "number": 14127,
    "commitment": "0x052fdba72bbb6fcc10940fc22dc76e459dda32604a17a920b8f2d2d0f0caff8f",
    "rootHash": "0x2a5bb557a7b39afc2480b84c56f230ac2be6148fc367edf5f9447e3521a0da56",
    "feeAccountId": 0,
    "blockSize": 20,
    "opsCompositionNumber": 401,
    "createdAt": "2023-02-04T04:41:35Z",
    "transactions": [
      {
        "txHash": "0x7c85b50664efb9672613b2b3ceafd6e1f9f47938053690b885f0a8071fac1ffa",
        "tx": {
          "type": "Deposit",
          "fromChainId": 2,
          "from": "0x3d809e414ba4893709c85f242ba3617481bc4126",
          "subAccountId": 0,
          "l1SourceToken": 18,
          "l2TargetToken": 18,
          "amount": "10000000000000000000",
          "to": "0xb92a8ba62ff1d141798c7133cccefb33d9073323",
          "serialId": 22,
          "ethHash": "0x6ac27ec5de06c51dc9f167aa13424d08026953c9706d39c72d64cabe59ad7266"
        },
        "updates": [
          {
            "type": "AccountCreate",
            "updateId": 15,
            "accountId": 4,
            "address": "0xb92a8ba62ff1d141798c7133cccefb33d9073323"
          },
          {
            "type": "BalanceUpdate",
            "updateId": 16,
            "accountId": 4,
            "subAccountId": 0,
            "coinId": 18,
            "oldBalance": "0",
            "newBalance": "10000000000000000000",
            "oldNonce": 0,
            "newNonce": 0
          },
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
      },
      {
        "txHash": "0x7aaf7535c5edecf19805b6395e92865c544f387c71d8f75648d0353100a821f6",
        "tx": {
          "type": "FullExit",
          "toChainId": 1,
          "accountId": 28,
          "subAccountId": 1,
          "exitAddress": "0xae08c2e27765faef5cb05908dbac12242caf91af",
          "l2SourceToken": 44,
          "l1TargetToken": 44,
          "serialId": 28,
          "ethHash": "0x57b1fda1f7dd3aac85af60dc69300e0209c0a6abadc047a8f88e0a894220ba82"
        },
        "updates": [
          {
            "type": "BalanceUpdate",
            "updateId": 0,
            "accountId": 28,
            "subAccountId": 1,
            "coinId": 44,
            "oldBalance": "1080000000000000000",
            "newBalance": "0",
            "oldNonce": 0,
            "newNonce": 0
          },
          {
            "type": "BalanceUpdate",
            "updateId": 1,
            "accountId": 1,
            "subAccountId": 1,
            "coinId": 44,
            "oldBalance": "1497300000000000000000",
            "newBalance": "1496220000000000000000",
            "oldNonce": 0,
            "newNonce": 0
          }
        ]
      },
      {
        "txHash": "0x7c2dab9dadc2f3471b71c5531bfa70d0a67d16df62cfd6ed43b5970b3dccf7ad",
        "tx": {
          "type": "ChangePubKey",
          "chainId": 2,
          "accountId": 2,
          "subAccountId": 0,
          "newPkHash": "0x870b67b523f93dad7a313f6b64b9608dedab3874",
          "feeToken": 18,
          "fee": "1000000000000000000",
          "nonce": 0,
          "signature": {
            "pubKey": "5f07954b65b5407a37ec0a2c54fb4647e2014475936057bb2f52a6faab938b02",
            "signature": "4ad95ddd573830c2e85065ee201e503b24fb56faeedc1790e8a35668805d7b00691cb8c78ce124fbe0c15ed903c72975750eb12f74a5c711b870fba3496f0402"
          },
          "ethAuthData": {
            "type": "EthECDSA",
            "ethSignature": "0xf8aa40b44c89e3be8a07fc25e90b9c069fde2f3fb01125f9a8683fba054b5b4961ccbc6e957fda303c64868d9f6bc3b0f8c55a38137195633f330573a813a5d31b"
          },
          "ts": 1677821209
        },
        "updates": [
          {
            "type": "AccountChangePubkeyUpdate",
            "updateId": 8,
            "accountId": 2,
            "oldPubkeyHash": "0x0000000000000000000000000000000000000000",
            "newPubkeyHash": "0x870b67b523f93dad7a313f6b64b9608dedab3874",
            "oldNonce": 0,
            "newNonce": 1
          },
          {
            "type": "BalanceUpdate",
            "updateId": 9,
            "accountId": 2,
            "subAccountId": 0,
            "coinId": 18,
            "oldBalance": "10000000000000000000",
            "newBalance": "9000000000000000000",
            "oldNonce": 1,
            "newNonce": 1
          },
          {
            "type": "BalanceUpdate",
            "updateId": 10,
            "accountId": 0,
            "subAccountId": 0,
            "coinId": 18,
            "oldBalance": "0",
            "newBalance": "1000000000000000000",
            "oldNonce": 0,
            "newNonce": 0
          }
        ]
      },
      {
        "txHash": "0xa2fa942af20e3ea1764158470a9f1f5609280dcd73cad5497ecf5ec20e8cad83",
        "tx": {
          "type": "Transfer",
          "accountId": 2,
          "fromSubAccountId": 0,
          "toSubAccountId": 1,
          "to": "0xdc9c9863167ee865edd5216964b8b99d43ee7a81",
          "token": 18,
          "amount": "1000000000000",
          "fee": "209000000000000",
          "nonce": 115,
          "signature": {
            "pubKey": "5f07954b65b5407a37ec0a2c54fb4647e2014475936057bb2f52a6faab938b02",
            "signature": "d6501c14f1ed8e3feeb4c3242697067dd60da0c56af544c8faeb2d055a21d3059c78c320008028f37fedc4d9578553782b2fd204610da670d1326a277dac2004"
          },
          "ts": 1677821506
        },
        "updates": [
          {
            "type": "BalanceUpdate",
            "updateId": 12,
            "accountId": 2,
            "subAccountId": 0,
            "coinId": 18,
            "oldBalance": "3018976060000000000000",
            "newBalance": "3018975850000000000000",
            "oldNonce": 115,
            "newNonce": 116
          },
          {
            "type": "BalanceUpdate",
            "updateId": 13,
            "accountId": 2,
            "subAccountId": 1,
            "coinId": 18,
            "oldBalance": "114000000000000",
            "newBalance": "115000000000000",
            "oldNonce": 115,
            "newNonce": 116
          },
          {
            "type": "BalanceUpdate",
            "updateId": 14,
            "accountId": 0,
            "subAccountId": 0,
            "coinId": 18,
            "oldBalance": "1023826000000000000",
            "newBalance": "1024035000000000000",
            "oldNonce": 0,
            "newNonce": 0
          }
        ]
      },
      {
        "txHash": "0x3b285c94d89ae17f7c288b10036a3dacff3460384601005afb440b1620bb538a",
        "tx": {
          "type": "Withdraw",
          "toChainId": 1,
          "accountId": 3,
          "subAccountId": 0,
          "to": "0x3d809e414ba4893709c85f242ba3617481bc4126",
          "l2SourceToken": 44,
          "l1TargetToken": 44,
          "amount": "1300000000000000000",
          "fee": "1531000000000000",
          "nonce": 81,
          "signature": {
            "pubKey": "b720c6110e673b55b5725dd0ff5778a8668ef4c7324718f78fa11def63081e85",
            "signature": "ec2c25aded9d1bbc85106178917a9da55a711449b4a49d0dbc01485290959225739ef0b3155498e96ba79cfbcb4a79a5f3ea4a179c6c210baca86820e8020b05"
          },
          "fastWithdraw": 1,
          "withdrawFeeRatio": 2000,
          "ts": 1677830493
        },
        "updates": [
          {
            "type": "BalanceUpdate",
            "updateId": 3,
            "accountId": 3,
            "subAccountId": 0,
            "coinId": 44,
            "oldBalance": "2993839561700000000000",
            "newBalance": "2992538030700000000000",
            "oldNonce": 81,
            "newNonce": 82
          },
          {
            "type": "BalanceUpdate",
            "updateId": 4,
            "accountId": 1,
            "subAccountId": 1,
            "coinId": 44,
            "oldBalance": "1500000000000000000000",
            "newBalance": "1498700000000000000000",
            "oldNonce": 0,
            "newNonce": 0
          },
          {
            "type": "BalanceUpdate",
            "updateId": 5,
            "accountId": 0,
            "subAccountId": 0,
            "coinId": 44,
            "oldBalance": "438300000000000",
            "newBalance": "1969300000000000",
            "oldNonce": 0,
            "newNonce": 0
          }
        ]
      },
      {
        "txHash": "0x8ada34680c8ba0e5cdbc14011e51c9989cab9339e0a7e679d14c14e7a587149d",
        "tx": {
          "type": "ForcedExit",
          "toChainId": 2,
          "initiatorAccountId": 3,
          "initiatorSubAccountId": 0,
          "target": "0x086cacda48e8a77680ba1e79177d1655f7130c95",
          "targetSubAccountId": 1,
          "l2SourceToken": 40,
          "l1TargetToken": 40,
          "fee": "19030000000000000",
          "feeToken": 18,
          "nonce": 178,
          "signature": {
            "pubKey": "b720c6110e673b55b5725dd0ff5778a8668ef4c7324718f78fa11def63081e85",
            "signature": "b119bd5971397b6abc499f6a2c09358b8c39d937be5bc96e3f186b4fc5026c80a933eea1b8f9c71cab8bb10ae554a2765008b937a724d026512f97cbeeacef05"
          },
          "ts": 1677836553
        },
        "updates": [
          {
            "type": "BalanceUpdate",
            "updateId": 20,
            "accountId": 3,
            "subAccountId": 0,
            "coinId": 18,
            "oldBalance": "2790050120799999999999",
            "newBalance": "2790031090799999999999",
            "oldNonce": 178,
            "newNonce": 179
          },
          {
            "type": "BalanceUpdate",
            "updateId": 21,
            "accountId": 15,
            "subAccountId": 1,
            "coinId": 40,
            "oldBalance": "2100000000000000000",
            "newBalance": "0",
            "oldNonce": 0,
            "newNonce": 0
          },
          {
            "type": "BalanceUpdate",
            "updateId": 22,
            "accountId": 1,
            "subAccountId": 2,
            "coinId": 40,
            "oldBalance": "4000001486590000000000000000",
            "newBalance": "4000001484490000000000000000",
            "oldNonce": 0,
            "newNonce": 0
          },
          {
            "type": "BalanceUpdate",
            "updateId": 23,
            "accountId": 0,
            "subAccountId": 0,
            "coinId": 18,
            "oldBalance": "7272586000000000000",
            "newBalance": "7291616000000000000",
            "oldNonce": 0,
            "newNonce": 0
          }
        ]
      },
      {
        "txHash": "0x640f38c6b09744d627b16e03339064d6edd39187cde8c4329fa19a2a60db8664",
        "tx": {
          "type": "OrderMatching",
          "accountId": 6,
          "subAccountId": 1,
          "taker": {
            "accountId": 13,
            "subAccountId": 1,
            "slotId": 163,
            "nonce": 0,
            "baseTokenId": 41,
            "quoteTokenId": 1,
            "amount": "1886200000000000000",
            "price": "1568210000000000000000",
            "isSell": 1,
            "feeRatio1": 5,
            "feeRatio2": 10,
            "signature": {
              "pubKey": "1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
              "signature": "57b7a0f06eb5d5dffbc8e8a6626ef7f9c472a8ac7761db36f8a0a2df2049988a9b43e8b3292a969b9b6cc5f689a5eecb8b05981ef1e3ca6e0de0da64ae6d6e01"
            }
          },
          "maker": {
            "accountId": 13,
            "subAccountId": 1,
            "slotId": 898,
            "nonce": 1,
            "baseTokenId": 41,
            "quoteTokenId": 1,
            "amount": "684900000000000000",
            "price": "1569150000000000000000",
            "isSell": 0,
            "feeRatio1": 5,
            "feeRatio2": 10,
            "signature": {
              "pubKey": "1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
              "signature": "b0aebe2bdd7a98a78d564d11c5f138cf428382628af55e85c250446b03dc0005f734e952819278d0b96fa970c6ab0018e33bb5bda7c55d69658551d2e606b904"
            }
          },
          "fee": "391000000000000",
          "feeToken": 1,
          "expectBaseAmount": "54800000000000000",
          "expectQuoteAmount": "85989420000000000000",
          "signature": {
            "pubKey": "84bf4edbe1f7056f079ba4c38359427f43d529fbab2e94e6d6b7a18efbf2fb87",
            "signature": "34e768dce60268f702b9a9ca68cb19d6785d2d1da8d7c799a29aa45e8c8bd9096c20cbf518c95fce2b2b58329492c35896162fe9cb1ff13f6e3a946b15bb2202"
          }
        },
        "updates": [
          {
            "type": "OrderUpdate",
            "updateId": 0,
            "accountId": 13,
            "subAccountId": 1,
            "slotId": 898,
            "oldTidyOrder": {
              "nonce": 1,
              "residue": "54800000000000000"
            },
            "newTidyOrder": {
              "nonce": 2,
              "residue": "0"
            }
          },
          {
            "type": "BalanceUpdate",
            "updateId": 1,
            "accountId": 13,
            "subAccountId": 1,
            "coinId": 1,
            "oldBalance": "200304228482442156994500000",
            "newBalance": "200304142493022156994500000",
            "oldNonce": 1,
            "newNonce": 1
          },
          {
            "type": "BalanceUpdate",
            "updateId": 2,
            "accountId": 13,
            "subAccountId": 1,
            "coinId": 41,
            "oldBalance": "199999971788889400000000000",
            "newBalance": "199999971843662000000000000",
            "oldNonce": 1,
            "newNonce": 1
          },
          {
            "type": "OrderUpdate",
            "updateId": 3,
            "accountId": 13,
            "subAccountId": 1,
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
          {
            "type": "BalanceUpdate",
            "updateId": 4,
            "accountId": 13,
            "subAccountId": 1,
            "coinId": 41,
            "oldBalance": "199999971843662000000000000",
            "newBalance": "199999971788862000000000000",
            "oldNonce": 1,
            "newNonce": 1
          },
          {
            "type": "BalanceUpdate",
            "updateId": 5,
            "accountId": 13,
            "subAccountId": 1,
            "coinId": 1,
            "oldBalance": "200304142493022156994500000",
            "newBalance": "200304228396452736994500000",
            "oldNonce": 1,
            "newNonce": 1
          },
          {
            "type": "BalanceUpdate",
            "updateId": 6,
            "accountId": 6,
            "subAccountId": 1,
            "coinId": 1,
            "oldBalance": "9999680162000000000000",
            "newBalance": "9999679771000000000000",
            "oldNonce": 224,
            "newNonce": 224
          },
          {
            "type": "BalanceUpdate",
            "updateId": 7,
            "accountId": 6,
            "subAccountId": 0,
            "coinId": 1,
            "oldBalance": "3529001244641983488661500000",
            "newBalance": "3529001244727972908661500000",
            "oldNonce": 224,
            "newNonce": 224
          },
          {
            "type": "BalanceUpdate",
            "updateId": 8,
            "accountId": 6,
            "subAccountId": 0,
            "coinId": 41,
            "oldBalance": "1594000000063937300000000000",
            "newBalance": "1594000000063964700000000000",
            "oldNonce": 224,
            "newNonce": 224
          },
          {
            "type": "BalanceUpdate",
            "updateId": 9,
            "accountId": 0,
            "subAccountId": 0,
            "coinId": 1,
            "oldBalance": "483683000000000000",
            "newBalance": "484074000000000000",
            "oldNonce": 0,
            "newNonce": 0
          }
        ]
      }
    ]
  },
  "id": 1
}
```
{% endtab %}
{% endtabs %}

#### **BlockResp**

<table><thead><tr><th width="280.55555555555554">Field</th><th>Description</th></tr></thead><tbody><tr><td>number</td><td>the block height</td></tr><tr><td>commitment</td><td>the commitment of the block, similar to the block hash of Ethereum</td></tr><tr><td>rootHash</td><td>the root hash of the state tree</td></tr><tr><td>feeAccountId</td><td>the id of the fee account</td></tr><tr><td>blockSize</td><td>the maximum chunk number that a block can contain</td></tr><tr><td>opsCompositionNumber</td><td>the vk of generating ZKPs</td></tr><tr><td>timestamp</td><td>the block timestamp</td></tr><tr><td>transactions</td><td>returns <code>[TxHash]</code> when <code>includeTx</code> is false, <code>TxResp</code> when true; ordered by transaction execution: the ones executed first come first in the array</td></tr></tbody></table>

#### **TxResp**

<table><thead><tr><th width="234.55555555555554">Field</th><th>Description</th></tr></thead><tbody><tr><td>txHash</td><td>the transaction hash</td></tr><tr><td>tx</td><td><code>ZkLinkTx</code></td></tr><tr><td>receipt</td><td><code>TxReceiptResp</code>, optional, since the transactions in a block always succeed, this field is ignored in <code>BlockResp</code></td></tr><tr><td>updates</td><td><code>[StateUpdateResp]</code>, ordered by <code>updateId</code>: the ones executed first come first in the array</td></tr></tbody></table>

**TxReceiptResp**

<table><thead><tr><th width="245.39130434782612">Field</th><th>Description</th></tr></thead><tbody><tr><td>executed</td><td>whether the transaction is executed</td></tr><tr><td>success</td><td>the result of execution, must be false if <code>executed</code> is false</td></tr><tr><td>failReason</td><td>the reason why the transaction fails. null if <code>success</code> is false</td></tr><tr><td>block</td><td>the block height of the transaction, 0 if <code>success</code> is false</td></tr><tr><td>index</td><td>the index of the transaction in a block, 0 if <code>success</code> is false</td></tr></tbody></table>

**StateUpdateResp**

The success of transaction execution will lead to the change of the state tree:

* AccountCreate: create a new account in the state tree
* AccountChangePubkeyUpdate: change in pubkey and nonce
* BalanceUpdate: change in account balance and nonce
* OrderUpdate: change in account slot

**AccountCreate**

<table><thead><tr><th width="254.39130434782612">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>AccountCreate</td></tr><tr><td>updateId</td><td>the position of the update in the block</td></tr><tr><td>accountId</td><td>the id of the new account</td></tr><tr><td>address</td><td>the account address</td></tr></tbody></table>

Example:

```json
{
  "type": "AccountCreate",
  "updateId": 40,
  "accountId": 42,
  "address": "0xe4efc3d7b69a19d3ae574cbc2915ddf598efe43f"
}
```

{% hint style="info" %}
Transactions that may generate `AccountCreate` include:

* Deposit
* Transfer
{% endhint %}

**AccountChangePubkeyUpdate**

<table><thead><tr><th width="191">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>AccountChangePubkeyUpdate</td></tr><tr><td>updateId</td><td>the position of the update in the block</td></tr><tr><td>accountId</td><td>the account id</td></tr><tr><td>oldPubkeyHash</td><td>the old pubkeyHash</td></tr><tr><td>newPubkeyHash</td><td>the new pubkeyHash</td></tr><tr><td>oldNonce</td><td>the old nonce</td></tr><tr><td>newNonce</td><td>the new nonce</td></tr></tbody></table>

Example:

```json
{
  "type": "AccountChangePubkeyUpdate",
  "updateId": 10,
  "accountId": 39,
  "oldPubkeyHash": "0x0000000000000000000000000000000000000000",
  "newPubkeyHash": "0xbfb4f4a68dc9e49f7785082a8c12354ed663b6e0",
  "oldNonce": 0,
  "newNonce": 1
}
```

{% hint style="info" %}
only `ChangePubKey` will generate `AccountChangePubkeyUpdate`
{% endhint %}

**BalanceUpdate**

<table><thead><tr><th width="191">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>BalanceUpdate</td></tr><tr><td>updateId</td><td>the position of the update in the block</td></tr><tr><td>accountId</td><td>the account id</td></tr><tr><td>subAccountId</td><td>the id of the sub account</td></tr><tr><td>coinId</td><td>token id</td></tr><tr><td>oldBalance</td><td>the balance of the sub account before change</td></tr><tr><td>newBalance</td><td>the balance of the sub account after change</td></tr><tr><td>oldNonce</td><td>the old nonce</td></tr><tr><td>newNonce</td><td>the new nonce</td></tr></tbody></table>

Example:

```json
{
  "type": "BalanceUpdate",
  "updateId": 1,
  "accountId": 2,
  "subAccountId": 1,
  "coinId": 18,
  "oldBalance": "66517000000000000",
  "newBalance": "66518000000000000",
  "oldNonce": 66518,
  "newNonce": 66519
}
```

{% hint style="info" %}
All `ZkLinkTx` will generate `BalanceUpdate`
{% endhint %}

**OrderUpdate**

<table><thead><tr><th width="268.3913043478261">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>OrderUpdate</td></tr><tr><td>updateId</td><td>the position of the update in the block</td></tr><tr><td>accountId</td><td>the account id</td></tr><tr><td>subAccountId</td><td>the id of the sub account</td></tr><tr><td>coinId</td><td>token id</td></tr><tr><td>oldTidyOrder</td><td>the <code>TidyOrder</code> before change</td></tr><tr><td>newTidyOrder</td><td>the <code>TidyOrder</code> after change</td></tr></tbody></table>

**TidyOrder**

<table><thead><tr><th width="271.5270935960591">Field</th><th>Description</th></tr></thead><tbody><tr><td>nonce</td><td>slot nonce</td></tr><tr><td>residue</td><td>slot residue</td></tr></tbody></table>

Example:

```json
{
  "type": "OrderUpdate",
  "updateId": 30,
  "accountId": 11,
  "subAccountId": 1,
  "slotId": 27,
  "oldTidyOrder": {
    "nonce": 14,
    "residue": "4607200000000000000000"
  },
  "newTidyOrder": {
    "nonce": 14,
    "residue": "4233800000000000000000"
  }
}
```

{% hint style="info" %}
Only `OrderMatching` will generate `OrderUpdate`
{% endhint %}

### getBlockOnChainNumber

Get transaction information in a block executed on L1 blockchains. Every block will be submitted to and executed on every L1 chain.

{% tabs %}
{% tab title="Request" %}
**Parameters**

* `chain_id`

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getBlockOnChainNumber",
    "params": [4906]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`BlockOnChainResp`

```json
{
    "jsonrpc": "2.0",
    "result": {
        "committed": [
            {
                "chainId": 2,
                "txHash": "0x7264f1d95b5339f77f2b24939bada1cbca183c77110e514159bbcfad3aa303d2"
            },
            {
                "chainId": 1,
                "txHash": "0x3e30f9bed591ec0339278faaed08e6200f4a5ded4668e3038e7ed70512a68967"
            }
        ],
        "verified": [
            {
                "chainId": 2,
                "txHash": "0xaa0f1fed256b21d8b2fd4b91450418061674971bce452121229126aa35effc37"
            },
            {
                "chainId": 1,
                "txHash": "0xa9548f4fe6a8d3127fdfd221ee2c97149bee8ad82744d2c698440ef3774f1122"
            }
        ]
    },
    "id": 1
}
```
{% endtab %}
{% endtabs %}

#### **BlockOnChainResp**

<table><thead><tr><th width="168">Field</th><th>Description</th></tr></thead><tbody><tr><td>committed</td><td>transaction info committed to L1, with type <code>OnChainResp</code></td></tr><tr><td>verified</td><td>transaction info executed on L1, with type <code>OnChainResp</code></td></tr></tbody></table>

#### **OnChainResp**

<table><thead><tr><th width="168">Field</th><th>Description</th></tr></thead><tbody><tr><td>chain_id</td><td>the chain id defined by zkLink</td></tr><tr><td>tx_hash</td><td>the hash of the transaction on L1 blockchains</td></tr></tbody></table>

{% hint style="info" %}
Noted that since blocks are committed to and executed on L1 blockchains in batches, the on-chain data of blocks in the same batch is the same. For example, when blocks \[4906, 4910] are in the same batch, their on-chain transaction info is the same.

Commitment and batching are asynchronous. For example, when the current block height is 1000, the committed block height can be 980, and the verified block height can be 950. The API caller should query the on-chain infor by the committed and verified block height via `getLatestBlockNumber`. For example, when the latest verified block height is 950, the return of the on-chain block info that is after 950 must be null.
{% endhint %}

### **getAccount**

Get account info by address or account id.

{% tabs %}
{% tab title="Request" %}
**Parameters**

* Address|AccountID - Address(20Bytes or 32Bytes) or integer account id.

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getAccount",
    "params": [
        "0x1aef2b4c06b83cdb2783d3458cdbf3886a6ae7d4"
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`AccountInfoResp`

```json
{
    "jsonrpc": "2.0",
    "result": {
        "id": 1,
        "address": "0x1aef2b4c06b83cdb2783d3458cdbf3886a6ae7d4",
        "nonce": 0,
        "pubKeyHash": "0x0000000000000000000000000000000000000000",
      	"subAccountNonces": {
          "1": 0,
          "2":13
        }
    },
    "id": 1
}
```
{% endtab %}
{% endtabs %}

#### **AccountInfoResp**

<table><thead><tr><th width="316">Field</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>account id</td></tr><tr><td>address</td><td>account address</td></tr><tr><td>nonce</td><td>account nonce</td></tr><tr><td>pubKeyHash</td><td>account pubKeyHash</td></tr><tr><td>subAccountNonces</td><td>nonce of the subaccount, HashMap&#x3C;SubAccountId,Nonce></td></tr></tbody></table>

{% hint style="info" %}
`pubKeyHash` being `0x0000000000000000000000000000000000000000` means unactivated account.
{% endhint %}

### getAccountBalances

Get the balance info of an account.

{% tabs %}
{% tab title="Request" %}
**Parameters:**

* `accountId`: account id
* `subAccountId`: optional, the id of the subaccount; null if the query requests the balance of all subaccounts.

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getAccountBalances",
    "params": [
        1,
        0
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`HashMap<SubAccountId,<TokenId,Balance>>`

```json
{
    "jsonrpc": "2.0",
    "result": {
        "0": {
           "18": "1498994167999999999973"
        }
    },
    "id": 1
}
```
{% endtab %}
{% endtabs %}

### getAccountOrderSlots

Get the order slot of an account.

{% tabs %}
{% tab title="Request" %}
**Parameters**

* `accountId`: account id
* `subAccountId`: optional, the id of the subaccount; null if the query requests the balance of all subaccounts.

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getAccountOrderSlots",
    "params": [
        1,
        null
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`HashMap<SubAccountId,<SlotId, TidyOrder>>`

```json
{
    "jsonrpc": "2.0",
    "result": {
      "0": {
        "3": {
          "nonce": 21,
          "residue": "348694029837858"
        }
      }
    },
    "id": 1
}
```
{% endtab %}
{% endtabs %}

### getTokenReserve

Get the withdrawable limit of a token on a certain L1 chain. There are 3 cases:

<table data-view="cards" data-full-width="false"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td>Case 1</td><td><p><strong>User Input:</strong></p><p>Token: <code>ZKL</code></p><p>Withdraw to: <code>ETH</code></p><p><strong>The maximum ZKL that the user can withdraw:</strong></p><p>RPC-API tokenId: <code>ZKL, false</code></p></td><td></td></tr><tr><td>Case 2</td><td><p><strong>User Input:</strong></p><p>Token: <code>USDC</code></p><p>Withdraw to: <code>ETH</code></p><p><strong>The maximum USDC that the user can withdraw:</strong></p><p>RPC-API tokenId: <code>USDC, false</code></p></td><td></td></tr><tr><td>Case 3</td><td><p><strong>User Input:</strong></p><p>Token: <code>USD</code></p><p>Withdraw to: <code>ETH</code></p><p>Withdraw as: <code>USDC</code></p><p><strong>The maximum USDC that the user can withdraw:</strong></p><p>RPC-API tokenId: <code>USDC, true</code></p></td><td></td></tr></tbody></table>

{% tabs %}
{% tab title="Request" %}
**Parameters**

* `tokenId`: tokenId
* `mapping`: whether to query mapping, valid only when tokenId corresponds to USD stable.

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getTokenReserve",
    "params": [
      17,
      true
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`HashMap<ChainId, Balance>`

```json
{
    "jsonrpc": "2.0",
    "result": {
			"0": "134",
			"1": "1000000000000000000000000000000000000000000000"
    },
    "id": 1
}
```
{% endtab %}
{% endtabs %}

### getAccountSnapshot

In zkLink v0.4.0, transactions are executed before being batched, thus the APIs that request account states may not return the states in the latest block. The related APIs include:

* getAccount
* getAccountBalances
* getAccountOrderSlots
* getTokenReserve

getAccountSnapshot is introduced to request the account states of a certain block height, including the basic info, balance info, and order slot info.

{% tabs %}
{% tab title="Request" %}
**Parameters:**

* `accountAddress` or `accountId`
* `subAccountId`: optional, the id of the subaccount; null if the query requests the balance of all subaccounts.
* blockNumber: optional, null to return the latest block snapshot

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getAccountSnapshot",
    "params": [
      10,
      1,
      103
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`AccountSnapshotResp`

```json
{
  "jsonrpc": "2.0",
  "result": {
    "id": 1,
    "address": "0x1aef2b4c06b83cdb2783d3458cdbf3886a6ae7d4",
    "nonce": 0,
    "pubKeyHash": "0x0000000000000000000000000000000000000000",
    "subAccountNonces": {
      "1": 0,
      "2":13
    }
    "balances": {
      "0": {
        "18": "1498994167999999999973"
      }
    },
    "orderSlots": {
      "0": {
        "3": {
          "nonce": 21,
          "residue": "348694029837858"
        }
      },
      "blockNumber": 103
    }
  },
  "id": 1
}
```
{% endtab %}
{% endtabs %}

#### AccountSnapshotResp

<table><thead><tr><th width="266">Field</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>account id</td></tr><tr><td>address</td><td>account address</td></tr><tr><td>nonce</td><td>account nonce</td></tr><tr><td>pubKeyHash</td><td>account pubKeyHash</td></tr><tr><td>subAccountNonces</td><td>nonce of the subaccount, HashMap&#x3C;SubAccountId,Nonce></td></tr><tr><td>balances</td><td><code>HashMap&#x3C;SubAccountId,&#x3C;TokenId,Balance>></code></td></tr><tr><td>orderSlots</td><td><code>HashMap&#x3C;SubAccountId,&#x3C;SlotId, TidyOrder>></code></td></tr><tr><td>blockNumber</td><td>the block height of the snapshot</td></tr></tbody></table>

### getTransactionByHash

Get transaction info.

{% tabs %}
{% tab title="Request" %}
**Parameters:**

* `txHash`
* `includeUpdate`: whether to include the state change by the transaction

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getTransactionByHash",
    "params": [
        "0x3210bb3d6719d730b30c4c9a0086d507040e25f83bea4ff4b8c2c91bf8e8c4f9",
      	true
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`TxResp`\*since the transaction may fail, the `TxResp` will also include `receipt`.

```json
{
    "jsonrpc": "2.0",
    "result": {
        "txHash": "0x4221afe405566e4b057d36060e6a5d33151a10a1b9b00da71705e534b6646f22",
        "tx": {
            "type": "Transfer",
            "accountId": 2,
            "fromSubAccountId": 0,
            "toSubAccountId": 1,
            "to": "0xdc9c9863167ee865edd5216964b8b99d43ee7a81",
            "token": 18,
            "amount": "1000000000000",
            "fee": "216000000000000",
            "nonce": 38192,
            "signature": {
                "pubKey": "5f07954b65b5407a37ec0a2c54fb4647e2014475936057bb2f52a6faab938b02",
                "signature": "79bdeaa739557a4be289ef2bf718253ce791adc3ce5fbcb7abcad4b0a2d6e203a9c38d68d987f96ffc7a391f2518f281874f33c7a5a7110d27591ad029b31005"
            },
            "ts": 1675406687
        },
        "receipt": {
            "executed": true,
            "success": true,
            "failReason": null,
            "block": 3947,
          	"index": 1
        },
        "updates": [
            {
                "type": "BalanceUpdate",
                "updateId": 9,
                "accountId": 2,
                "subAccountId": 0,
                "coinId": 18,
                "oldBalance": "1941710093000000000000",
                "newBalance": "1941709876000000000000",
                "oldNonce": 38192,
                "newNonce": 38193
            },
            {
                "type": "BalanceUpdate",
                "updateId": 10,
                "accountId": 2,
                "subAccountId": 1,
                "coinId": 18,
                "oldBalance": "38191000000000000",
                "newBalance": "38192000000000000",
                "oldNonce": 38192,
                "newNonce": 38193
            }
        ]
    },
    "id": 1
}
```
{% endtab %}
{% endtabs %}

### getAccountTransactionHistory

Get account history in descending order of the transaction id in the database. Current only `Deposit`, `Withdraw`, and `Transfer` are supported.

{% tabs %}
{% tab title="Request" %}
**Parameters**

* `Deposit`, `Withdraw` or `Transfer`
* account address
* the page index, starting from 0
* the page size

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getTransactionByHash",
    "params": [
        "0x3210bb3d6719d730b30c4c9a0086d507040e25f83bea4ff4b8c2c91bf8e8c4f9",
      	true
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`Page<ZkLinkTxHistory>`

```json
{
    "jsonrpc": "2.0",
    "result": {
        "totalPageNum": 20,
        "pageIndex": 0,
        "pageSize": 2,
        "pageData": [
            {
                "chainId": 0,
                "fromAccount": "0x3498f456645270ee003441df82c718b56c0e6666",
                "toAccount": "0xbfda941bd2a0eddb57b10f8e8d3486a738b92ccc",
                "amount": "10000000000000000000000000",
                "nonce": 262,
                "tx": {
                    "type": "Transfer",
                    "accountId": 4,
                    "fromSubAccountId": 0,
                    "toSubAccountId": 1,
                    "to": "0xbfda941bd2a0eddb57b10f8e8d3486a738b92ccc",
                    "token": 47,
                    "amount": "10000000000000000000000000",
                    "fee": "4290000000000",
                    "nonce": 262,
                    "signature": {
                        "pubKey": "84bf4edbe1f7056f079ba4c38359427f43d529fbab2e94e6d6b7a18efbf2fb87",
                        "signature": "a5389ea55bfb88a9457eadba7ef1821d1cc7a51d29ab1fe17b64b7294ea8160acde7ece1fd3c8028b92a1071f41f567e11b27fa30b39959c744f8ccd04052003"
                    },
                    "ts": 1675442473
                },
                "txHash": "0x741b9b668430c78f87c8a9fb6b257f93a151f1101160bcc681536536f982c5b5",
                "txReceipt": {
                    "executed": true,
                    "success": true,
                    "failReason": null,
                    "block": 9991,
                    "index": 3
                },
                "createdAt": "2023-02-03T16:41:13.501848Z"
            },
            {
                "chainId": 0,
                "fromAccount": "0x3498f456645270ee003441df82c718b56c0e6666",
                "toAccount": "0xbfda941bd2a0eddb57b10f8e8d3486a738b92ccc",
                "amount": "10000000000000000000000000",
                "nonce": 261,
                "tx": {
                    "type": "Transfer",
                    "accountId": 4,
                    "fromSubAccountId": 0,
                    "toSubAccountId": 1,
                    "to": "0xbfda941bd2a0eddb57b10f8e8d3486a738b92ccc",
                    "token": 46,
                    "amount": "10000000000000000000000000",
                    "fee": "1270000000000000",
                    "nonce": 261,
                    "signature": {
                        "pubKey": "84bf4edbe1f7056f079ba4c38359427f43d529fbab2e94e6d6b7a18efbf2fb87",
                        "signature": "df5b57a04d907bf2a4639cb4b405320da69416e45516c3d2d4346058fa8a538498d8ca7ec4c84d11fa58f71395d8c5f45adffa4e2ff418038dd0dbc3cf4e7a00"
                    },
                    "ts": 1675442469
                },
                "txHash": "0xb4e12570be6c01a49cfd6c14e7021b7e482ea938804242b41680acd09ffe59d8",
                "txReceipt": {
                    "executed": true,
                    "success": true,
                    "failReason": null,
                    "block": 9989,
                    "index": 6
                },
                "createdAt": "2023-02-03T16:41:10.744899Z"
            }
        ]
    },
    "id": 1
}
```
{% endtab %}
{% endtabs %}

#### **Page\<ZkLinkTxHistory>**

<table><thead><tr><th width="241.48806941431673">Field</th><th>Description</th></tr></thead><tbody><tr><td>totalPageNum</td><td>the number of pages</td></tr><tr><td>pageIndex</td><td>index of the current page</td></tr><tr><td>pageSize</td><td>the size of the page</td></tr><tr><td>pageData</td><td>page data<code>[ZkLinkTxHistory]</code></td></tr></tbody></table>

#### **ZkLinkHistory**

<table><thead><tr><th width="227.62655601659753">Field</th><th>Description</th></tr></thead><tbody><tr><td>chainId</td><td><p>chain id defined by zkLink</p><ul><li>Deposit: the chain that the deposit is from</li><li>Transfer and Withdraw: no specific meaning</li></ul></td></tr><tr><td>fromAccount</td><td><p>address of from_account</p><ul><li>Deposit: the L1 address that the deposit is from</li><li>Transfer: the from_address of the transfer</li><li>Withdraw: the from_address of the withdraw</li></ul></td></tr><tr><td>toAccount</td><td><p>address of to_account</p><ul><li>Deposit: L2 address that the deposit is to</li><li>Transfer: the to_address of the transfer</li><li>Withdraw: the L1 address that the withdraw is to</li></ul></td></tr><tr><td>amount</td><td><p>the amount of the transaction</p><ul><li>Deposit: the amount of the deposit</li><li>Transfer: the amount of the transfer</li><li>Withdraw: the amount of the withdraw</li></ul></td></tr><tr><td>nonce</td><td><p>the nonce of the transaction</p><ul><li>Deposit: the serialId of L1 event</li><li>Transfer and Withdraw: the nonce of the transaction</li></ul></td></tr><tr><td>tx</td><td><code>ZkLinkTx</code></td></tr><tr><td>txHash</td><td>the hash of the transaction</td></tr><tr><td>txReceipt</td><td><code>TxReceiptResp</code></td></tr><tr><td>createdAt</td><td>the time that the transaction is received by zkLink</td></tr></tbody></table>

{% hint style="info" %}
Deposit: returns the transaction history of which the account address equals to\_address;

Withdraw: returns the transaction history of which the account address equals from\_address;

Transfer: returns the transaction history of which the account address equals either to\_address or from\_address.
{% endhint %}

### getFastWithdrawTxs

Get transaction info of fast\_withdraw transactions.

{% tabs %}
{% tab title="Request" %}
**Parameters:**

* `lastTxTimestamp`: ISO 8601 standard with date, time, and time zone;
* `maxTxs`: the max value of the number of fast\_withdraw in the return.

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "getFastWithdrawTxs",
    "params": [
        "2023-02-03T16:41:10.744899Z",
        10
    ]
}
```
{% endtab %}

{% tab title="Response" %}
**Returns**

`Vec<FastWithdrawTxResp>`

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "txHash": "0xb4e12570be6c01a49cfd6c14e7021b7e482ea938804242b41680acd09ffe59d8",
      "tx": {
        "type": "Withdraw",
        "toChainId": 1,
        "accountId": 7,
        "subAccountId": 2,
        "to": "0x3498f456645270ee003441df82c718b56c0e6666",
        "l2SourceToken": 1,
        "l1TargetToken": 17,
        "amount": "995900000000000000",
        "fee": "4100000000000000",
        "withdrawFeeRatio": 50,
        "fastWithdraw": 1,
        "ts": 1646102148,
        "nonce": 0, // subaccount nonce
        "signature": {
          "pubKey": "0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
          "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
        }
      },
      "executedTimestamp": "2023-02-03T16:41:10.744899Z"
    }
  ],
  "id": 1
}
```
{% endtab %}
{% endtabs %}

#### **FastWithdrawTxResp**

<table><thead><tr><th width="284.6573875802998">Field</th><th>Description</th></tr></thead><tbody><tr><td>txHash</td><td>transaction hash on zkLink</td></tr><tr><td>tx</td><td>zkLink transaction</td></tr><tr><td>executedTimestamp</td><td>the execution timestamp of the transaction</td></tr></tbody></table>

zkLink will scan the `maxTxs` number of executed withdraw transactions, and returns the fast\_withdraw transactions among which; thus the number of returns might be less than `maxTxs`, even be 0.

In the first scan, `lastTxTimestamp` can be set as 0 to scan from the beginning. Then the `executedTimestamp` of the last record can be used as the `lastTxTimestamp` of the next scan.

### sendTransaction

Submit L2 transaction and return transaction hash.

{% tabs %}
{% tab title="Request" %}
**Parameters:**

* `ZkLinkTx`: [L2 transaction](layer2-transaction.md)
* EthereumSignature: layer1 signature with type Option\<TxLayer1Signature>; required for Layer2 transactions apart from `ChangePubKey` or `OrderMatching`.
* submitterSignature: `Option<TxSignature>`, required only when the transaction involves subaccounts (except #0 subaccount); `submitterSignature` is a zk signature to `tx` hash.

```json
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "sendTransaction",
    "params": [
        {
            "accountId": 8,
            "fromSubAccountId": 3,
            "toSubAccountId": 3,
            "from": "0x3498F456645270eE003441df82C718b56c0e6666",
            "to": "0xbfDa941Bd2a0eddB57b10f8E8d3486A738B92cCC",
            "tokenId": 3,
            "amount": "998000000000000000",
            "fee": "3000000000000000",
            "ts": 1646101085,
            "nonce": 1,
            "type": "Transfer",
            "token": 3,
            "signature": {
                "pubKey": "0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
                "signature": "892c622afac908201df54a3cfdecf8eba46d5411bdc29365f5536f024c195f2893d6313a6371fe1659830e2560c1eaedbafcc835837593d017cd557074f0bb03"
            }
        },
        {
            "type": "EthereumSignature",
            "signature": "0xc29d647a5e9e078c66594f04881c34d6b57e1085ab825f17ffb1d0fe233e9834191b374daaaf1e44e5749f6cf44f2143799373fc5e7e844d48fec5e6bc08f0651b"
        },
      	null
    ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x3210bb3d6719d730b30c4c9a0086d507040e25f83bea4ff4b8c2c91bf8e8c4f9"
}
```
{% endtab %}
{% endtabs %}

#### **TxLayer1Signature**

L1 signatures apply [EIP-191](https://eips.ethereum.org/EIPS/eip-191) specification with zkLink `Eth sig message`.

<table><thead><tr><th width="206">Field</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td><p>the type of the signature</p><ul><li>EthereumSignature: Ethereum ECDSA signature</li><li>EIP1271Signature: Ethereum <a href="https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1271.md">EIP1271</a> signature</li></ul></td></tr><tr><td>signature</td><td>signature output, a hex string</td></tr></tbody></table>

Examples:

```json
{
  "type": "EthereumSignature",
  "signature": "0xc29d647a5e9e078c66594f04881c34d6b57e1085ab825f17ffb1d0fe233e9834191b374daaaf1e44e5749f6cf44f2143799373fc5e7e844d48fec5e6bc08f0651b"
}
```

```json
{
  "type": "EIP1271Signature",
  "signature": "0xc29d647a5e9e078c66594f04881c34d6b57e1085ab825f17ffb1d0fe233e9834191b374daaaf1e44e5749f6cf44f2143799373fc5e7e844d48fec5e6bc08f0651b"
}
```

#### **TxSignature**

Signatures for L2 transactions use zkLink `Encode`.&#x20;

<table><thead><tr><th width="158">Field</th><th>Description</th></tr></thead><tbody><tr><td>pubKey</td><td>the public key of ，a hex string without <code>0x</code> prefix</td></tr><tr><td>signature</td><td>signature output, a hex string without <code>0x</code> prefix</td></tr></tbody></table>

Example:

```json
{
  "pubKey": "0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
  "signature": "892c622afac908201df54a3cfdecf8eba46d5411bdc29365f5536f024c195f2893d6313a6371fe1659830e2560c1eaedbafcc835837593d017cd557074f0bb03"
}
```



`version: 4457a91`
