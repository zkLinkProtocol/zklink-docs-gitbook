## Type ZkLinkSigner
`ZkLinkSigner` includes the L1 private key(Eth or Starknet) and L3 private key.

### func ethSig
```dart
ZkLinkSigner ethSig(String sig)
```
Creat a `ZkLinkSigner` from eth personal sign.

**input:**
* sig: hex string of eth personal sign(with or without `0x` prefix)

### func starknetSig
```dart
ZkLinkSigner starknetSig(String sig)
```
Creat a `ZkLinkSigner` from starknet signature.

**input:**
* sig: hex string of starknet signature(with or without `0x` prefix)

### func getPubkey
```dart
String getPubkey()
```
Return hex string of public key.

### func getPubkeyHash
```dart
String getPubkeyHash()
```
Return hex string of public key hash.

## Type Signer
L1 private key.

### func ethSigner
```dart
Signer ethSigner(String ethPrivateKey)
```
Create a Ethereum private key signer.

**input:** 
* ethPrivateKey: hex string of private key(with or without `0x` prefix)

### func starknetSigner
```dart
Signer starknetSigner(String ethPrivateKey, String starknetChainId, String starknetAddr)
```

Create a Starknet signer.

**input:** 
* ethPrivateKey: hex string of starknet private key
* starknetChainId: chain id of starknet
* starknetAddr: starknet address
