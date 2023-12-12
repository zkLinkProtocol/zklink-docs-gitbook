## H256
Hex string of `[32]uint8` with prefix `0x`

## ZkLinkAddress
String format of L1 address, it's 20 bytes length or 32 bytes length

## ZkLinkTx
Json string format of all Transaction types.

## TxLayer1Signature
Json string format of L1 signature, it is an Enum type that contains 3 L1 signature type, for example:

```js
// the Ethereum signature
{
    "type": "EthereumSignature",
    "signature": "0x91dc468f37b6ef35cd0972881d37636f0c8f8dc974608ee9bf2e20ec03c546876092999bb802e6d673bb9fc858d750fa3e578b6bd2f3fe5a8e74ca23504a42661c"
}

// the EIP1271 signature
{
    "type": "EIP1271Signature",
    "signature": "0x91dc468f37b6ef35cd0972881d37636f0c8f8dc974608ee9bf2e20ec03c546876092999bb802e6d673bb9fc858d750fa3e578b6bd2f3fe5a8e74ca23504a42661c"
}

// the starknet signature
{
    "type": "StarknetSignature",
    "signature": "0x91dc468f37b6ef35cd0972881d37636f0c8f8dc974608ee9bf2e20ec03c546876092999bb802e6d673bb9fc858d750fa3e578b6bd2f3fe5a8e74ca23504a42661c"
}

```
