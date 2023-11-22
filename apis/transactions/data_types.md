# Basic types

* [ChainId](#ChainId)
* [AccountId](#accountid)
* [SubAccountId](#subaccountid)
* [TokenId](#tokenid)
* [SlotId](#slotid)
* [PairId](#pairid)
* [MarginId](#marginid)
* [Nonce](#nonce)
* [ChainType](#chaintype)
* [H256](#h256)
* [TxHash](#txhash)
* [PubKeyHash](#pubkeyhash)

# Basic structures

* [TxLayer1Signature](#txlayer1signature)
* [ZkLinkSignature](#ZkLinkSignature)


 <span id="ChainId">**ChainId**</span>

The chain id defined by ZkLink, the type is `u8`

## AccountId
The account id defined by ZkLink, the type is `u32`

## SubAccountId
The subaccount id defined by ZkLink, the type is`u8`

## TokenId
The type is`u32`, different token contract addresses correspond to different token ids.
There are many stablecoins, for example, USDC or BUSD are equivalent to USD.
In order to aggregate the liquidity of these stablecoins, zkLink created a virtual USD token on L2.
Users deposite USDC or BUSD at L1 and can choose to receive the same amount of USD at L2.
Conversely, when withdrawing USD, users can choose to withdraw an equivalent amount of USDC or BUSD to L1.

| TokenId  | description                                        |
| -------- |----------------------------------------------------|
| 0        | illegal                                            |
| 1        | USD                                                |
| 2-16     | mapping tokenId of stablecoin corresponding to USD |
| 17-31    | the token id of stablecoin                         |
| 32-65535 | other token id                                     |


## SlotId
The id of slot in [Order](#Order), the type is `u32`.

## PairId
The trading pair ID of the pertetual contract, the type is `u16`. The PairId is defined by decentralized exchange, not defined by ZkLink.

## MarginId
The margin ID of the pertetual contract, the type is `u8`.

## Nonce
The nonce type in transaction and account, the type is `u32`

## ChainType

* 0: EVM
* 1: StarkNet

## TxHash
Transaction Hash, it is the unique address of a transaction that acts as a record or proof that the transaction has taken place.
It is the  hexadecimal serialized string  of `[u8; 32]` starting with `0x`, for example：

`0x7264f1d95b5339f77f2b24939bada1cbca183c77110e514159bbcfad3aa303d2`

## H256
the type is string, it is the  hexadecimal serialized string  of `[u8; 32]` starting with `0x`, for example:

`0x052fdba72bbb6fcc10940fc22dc76e459dda32604a17a920b8f2d2d0f0caff8f`

## PubKeyHash
The hash of public key of layer2, it is the  hexadecimal serialized string  of `[u8; 20]`, for example:

`0x3cfdecf8eba46d5411bdc29365f5536f024c195f`

## ZkLinkSignature
The L2 transaction signature.

| Name      | Type   | Description                                                                                                        |
|-----------|--------|--------------------------------------------------------------------------------------------------------------------|
| pubKey    | String | the public key of the signature which used to verify the signature, hexadecimal serialized string with `0x` prefix |
| signature | String | the signature，hexadecimal serialized string without 0x` prefix                                                     |

For example:

```json
{
    "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "892c622afac908201df54a3cfdecf8eba46d5411bdc29365f5536f024c195f2893d6313a6371fe1659830e2560c1eaedbafcc835837593d017cd557074f0bb03"
}
```


## TxLayer1Signature
The transaction L1 signature, for the ethereum, there are two types signatures [EIP-191](https://eips.ethereum.org/EIPS/eip-191) and [EIP1271](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1271.md).

| Name      | Type   | Description                        |
|-----------|--------|------------------------------------|
| type      | String | the type of signature              |
| signature | String | signature string, starts with `0x` |

where the `type` can be:
* EthereumSignature: the ethereum ECDSA signature
* EIP1271Signature: the ethereum EIP1271 signature
* StarkSignature: the starknet ECDSA signature

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

```json
{
  "type": "StarkSignature",
  "signature": "0xc29d647a5e9e078c66594f04881c34d6b57e1085ab825f17ffb1d0fe233e9834191b374daaaf1e44e5749f6cf44f2143799373fc5e7e844d48fec5e6bc08f0651b"
}
```



## Contract

Contract is the order in pertetual contract

| Name         | Type                                | Description                                                                        |
|--------------|-------------------------------------|------------------------------------------------------------------------------------|
| accountId    | [AccountId](#AccountId)             | account id                                                                         |
| subAccountId | [SubAccountId](#SubAccountId)       | sub account id                                                                     |
| slotId       | [SlotId](#SlotId)                   | slot id                                                                            |
| nonce        | [Nonce](#Nonce)                     | nonce                                                                              |
| pairId       | [PairId](#PairId)                   | the pair id                                                                        |
| size         | BigUint                             | position size                                                                      |
| price        | BigUint                             | price                                                                              |
| direction    | u8                                  | 1: long, 0: short                                                                  |
| feeRates     | [u8, u8]                            | The fee rates of [maker, taker]，100 means 1.00%                                    |
| hasSubsidy   | u8                                  | 1: true, 0: false, if the maker has subsidy, the submitter will give maker subsidy |
| signature    | [ZkLinkSignature](#ZkLinkSignature) | ZkLink L2 signature                                                                |


```json
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
        "pubKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "signature": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
      }
    }
```


## FundingInfo

The funding info in pertetual contract

| Name        | Type              | Description                                            |
|-------------|-------------------|--------------------------------------------------------|
| pairId      | [PairId](#PairId) | the pair id                                            |
| price       | BigUint           | Mark price of the trading pair                         |
| fundingRate | i16               | The numerator of funding rate(the denominator is `10^6`） |

For example:

```json
{
  "pairId": 1,
  "price": "100",
  "fundingRate": 2
}
```

## ContractPrice

| Name        | Type              | Description                     |
|-------------|-------------------|---------------------------------|
| pairId      | [PairId](#PairId) | The pair id                     |
| marketPrice | BigUint           | The market price of the pair id |

```json
{
  "pairId": 1,
  "marketPrice": "100"
}
```

## MarginPrice

| Name       | Type                | Description                   |
|------------|---------------------|-------------------------------|
| tokenId    | [TokenId](#tokenId) | The token id                  |
| marketPrice | BigUint             | The market price of the token |


## SpotPriceInfo

| Name        | Type                | Description               |
|-------------|---------------------|---------------------------|
| tokenId     | [TokenId](#TokenId) | The token id              |
| marketPrice | BigUint             | THe market price of token |

For example:
```json
{
  "tokenId": 1,
  "price": "100"
}
```


# OraclePrices

```json
{
  "contractPrices": [
    {
      "pairId": 1,
      "marketPrice": "100"
    }
  ],
  "marginPrices": [
    {
      "tokenId": 1,
      "price": "100"
    }
  ]
}
```

| 字段             | 类型                                    | 描述                  |
|----------------|---------------------------------------|---------------------|
| contractPrices | [ContractPrices](#ContractPrice) list | contract price list |
| marginPrices   | [MarginPrice](#MarginPrice) list      | margin price list   |

# Parameter

This is the parameter in [UpdateGlobalVar]() which contains 6 types of data

1. feeAccount

Modify the collect-fee account.
 
   | Name       | Type                    | Description        |
   |------------|-------------------------|--------------------|
   | feeAccount | [AccountId](#AccountId) | The fee account id |

For Example:

```json
{
  "feeAccount": {
    "accountId": 1
  }
}
```

2. insuranceFundAccount

Modify the insurance fund account

```json
 {
    "insuranceFundAccount": {
      "accountId": 1
    }
}
```
| 字段        | 类型                        | 描述                 |
|-----------|---------------------------|--------------------|
| accountId | [AccountId](#AccountId)  | The account id of  insuranceFundAccount|


3. marginInfo

Modify the margin info in the specified index.

| 字段        | 类型                    | 描述                        |
|-----------|-----------------------|---------------------------|
| marginId | [MarginId](#MarginId) | The margin id             |
|tokenId | [TokenId](#TokenId)   | The Token id              |
|ratio | u8                    | the ratio, 100 means 1.0% |

For example:

```json
{
  "marginInfo": {
      "marginId": 1,
      "tokenId": 1,
      "ratio": 9
    }
}
```

4. contractInfo

Modify the info of every prepatual contract pair.

| Name                  | Type              | Description                                             |
|-----------------------|-------------------|---------------------------------------------------------|
| pairId                | [PairId](#PairId) | The pair id                                             |
| symbol                | String            | The symbol of the contract                              |
| initialMarginRate     | u16               | The initial margin rate of the contract, 100 means 0.1% |
| maintenanceMarginRate | u16               | The maintenance margin rate, 100 means 0.1%             |

```json
{
  "initialMarginRate": {
      "pairId": 1,
      "symbol": "BTCUSDC",
      "initialMarginRate": 2,
      "maintenanceMarginRate": 2
    }
}
```

5. fundingInfos

Update the funding rates to accumulated funding rates of the Global Vars for all position(contract pair) in this period

| Name  | Type                             | Description       |
|-------|----------------------------------|-------------------|
| infos | [FundingInfo](#FundingInfo) list | funding info list |

where `FundingInfo` is

| Name  | Type               | Description                      |
|-------|--------------------|----------------------------------|
|pairId | [PairId](#PairId)  | The pair id                      |
|price | BigUint            | the mark price of the trade pair |
|fundingRate | i16          |the fee funding rate, the actual result needs to be divided by `10^6`|

For example:

```json
{
  "fundingInfos": {
    "infos": [
      {
        "pairId": 0,
        "price": "0",
        "fundingRate": 0
      },
      {
        "pairId": 0,
        "price": "0",
        "fundingRate": 0
      }
    ]
  }
}
```

5. 更新所有合约对的的资金费率为累积资金费率

```json
{
  "fundingInfos": {
    "infos": [
      {
        "pairId": 0,
        "price": "0",
        "fundingRate": 0
      },
      {
        "pairId": 0,
        "price": "0",
        "fundingRate": 0
      }
    ]
  }
}
```
| 字段        | 类型                    | 描述     |
|-----------|-----------------------|--------|
|infos| [FundingInfo](#FundingInfo)列表| 资金费率列表 |

encode:

| 字段       | 规则          |
|----------|-------------|
| type     | **5**, 1 字节 |
|fundingRates| 18字节*列表长度   |

