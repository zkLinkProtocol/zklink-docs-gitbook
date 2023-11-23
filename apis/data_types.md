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


### ChainId

The chain id defined by ZkLink, the type is `u8`

### AccountId
The account id defined by ZkLink, the type is `u32`

### SubAccountId
The subaccount id defined by ZkLink, the type is`u8`

### TokenId
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


### SlotId
The id of slot in [Order](#Order), the type is `u32`.

### PairId
The trading pair ID of the pertetual contract, the type is `u16`. The PairId is defined by decentralized exchange, not defined by ZkLink.

### MarginId
The margin ID of the pertetual contract, the type is `u8`.

### Nonce
The nonce type in transaction and account, the type is `u32`

### ChainType

* 0: EVM
* 1: StarkNet

### TxHash
Transaction Hash, it is the unique address of a transaction that acts as a record or proof that the transaction has taken place.
It is the  hexadecimal serialized string  of `[u8; 32]` starting with `0x`, for example：

`0x7264f1d95b5339f77f2b24939bada1cbca183c77110e514159bbcfad3aa303d2`

### H256
the type is string, it is the  hexadecimal serialized string  of `[u8; 32]` starting with `0x`, for example:

`0x052fdba72bbb6fcc10940fc22dc76e459dda32604a17a920b8f2d2d0f0caff8f`

### PubKeyHash
The public key hash of Zklink layer side, it is the  hexadecimal serialized string  of `[u8; 20]`, for example:

`0x3cfdecf8eba46d5411bdc29365f5536f024c195f`

### ZkLinkSignature
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


### TxLayer1Signature
The transaction L1 signature, for the ethereum, there are two types signatures [EIP-191](https://eips.ethereum.org/EIPS/eip-191) and [EIP1271](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1271.md).

| Name      | Type   | Required | Description                        |
|-----------|--------|-----------|------------------------------------|
| type      | String | yes       | the type of signature              |
| signature | String | yes       | signature string, starts with `0x` |

where the `type` can be `EthereumSignature`, `EIP1271Signature` and `StarkSignature`:

{% tabs %}

{% tab title="EthereumSignature" %}

The ethereum ECDSA signature, for example:

```json
{
   "type": "EthereumSignature",
   "signature": "0xc29d647a5e9e078c66594f04881c34d6b57e1085ab825f17ffb1d0fe233e9834191b374daaaf1e44e5749f6cf44f2143799373fc5e7e844d48fec5e6bc08f0651b"
}
```

{% endtab %}


{% tab title="EIP1271Signature" %}

The ethereum EIP1271 signature, for example:

```json
{
   "type": "EIP1271Signature",
   "signature": "0xc29d647a5e9e078c66594f04881c34d6b57e1085ab825f17ffb1d0fe233e9834191b374daaaf1e44e5749f6cf44f2143799373fc5e7e844d48fec5e6bc08f0651b"
}
```

{% endtab %}

{% tab title="StarkSignature" %}

The starknet ECDSA signature, for example:

```json
{
   "type": "StarkSignature",
   "signature": "0xc29d647a5e9e078c66594f04881c34d6b57e1085ab825f17ffb1d0fe233e9834191b374daaaf1e44e5749f6cf44f2143799373fc5e7e844d48fec5e6bc08f0651b"
}
```

{% endtab %}

{% endtabs %}




