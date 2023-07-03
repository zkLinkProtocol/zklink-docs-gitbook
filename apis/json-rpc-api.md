# JSON-RPC API

dApps can get account status, send transactions, and call other functions via zkLink API.&#x20;

The zkLink API follows the JSON-RPC standard and is accessed via POST.

## API Overview

### Networks

#### getSupportChains

#### getSupportTokens

#### getLatestBlockNumber

#### getTokenReserve

### Block

#### getBlockByNumber

#### getBlockOnChainNumber

### Account

#### getAccount

#### getAccountBalances

#### getAccountOrderSlots

#### getAccountSnapshot

### Transaction

#### getTransactionByHash

#### getAccountTransactionHistory

#### getFastWithdrawTxs

#### sendTransaction





## getSupportChains

Get the configuration of all supported chains.

{% tabs %}
{% tab title="Request" %}
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

The request returns an array `ChainResp:`

| Field             | Description                                                                       |
| ----------------- | --------------------------------------------------------------------------------- |
| chainId           | defined by zkLink                                                                 |
| chainType         | defined by zkLink {0:"EVM", 1:"STARKNET"}                                         |
| layerOneChainId   | the chain ID of L1 blockchains                                                    |
| mainContract      | the address of zkLink main contract that interacts with the rest module contracts |
| layerZeroContract | the address of layerZero bridge that syncs L1 states                              |
| web3Url           | the url of L1 RPC                                                                 |
| gasTokenId        | the id of the gas token, defined by zkLink                                        |
| validator         | validator address                                                                 |

## getSupportTokens

Get the data of all tokens with on-chain contract addresses.

{% tabs %}
{% tab title="Request" %}
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

The request returns a `HashMap<TokenId,TokenResp>`, which has a unique token id and is used for all token-related queries in the RPC service. Symbols are used only for display on the UI and do not promise uniqueness.

#### **TokenResp**

| Field    | Discription                                                                           |
| -------- | ------------------------------------------------------------------------------------- |
| id       | token id                                                                              |
| symbol   | token symbol                                                                          |
| usdPrice | token price                                                                           |
| chains   | `HashMap<ChainId,ChainTokenResp>` that includes the contract addresses on each chain  |

#### &#x20;**ChainTokenResp**

| Field        | Discription                                |
| ------------ | ------------------------------------------ |
| chainId      | defined by zkLink                          |
| address      | the on-chain contract address of the token |
| decimals     | the decimals of the token on L1            |
| fastWithdraw | whether the token supports fast withdraw   |

The decimal is the accuracy of the token on L1, which is not always 18 (6 for USDC is 6 and 18 for ZKL), and varies on different chains for the same token (6 for USDC on Ethereum and 18 for USDC on BSC).

&#x20;When a user deposits to zkLink from connected networks, the front-end needs to calculate the amount required for calling the contract according to the accuracy:

```json
var amount_of_user_input = 1.0 // input from the user
var amount_to_call_contract = amount_of_user_input * 10 ** token.decimals // the parameter during calling the contract
// Example
// A user deposits 2 USDC, the parameter is 2 * 10^6
// A user deposits 5 ZKL, the parameter is 5 * 10^18
```



