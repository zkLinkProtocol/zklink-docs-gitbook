## type ChangePubKey
[ChangePubkey](../../../api-and-sdk/data-types/transaction/change\_pubkey.md) transaction type.

```dart
ChangePubkey(
    int chainId,
    int accountId,
    int subAccountId,
    String newPubkeyHash,
    int feeToken,
    String fee,
    int nonce,
    String? ethSignature,
    int? ts,
)
```

### func getEthSignMsg

```dart
String getEthSignMsg(int nonce, int accountId)
```

Get the Ethereum sign message

### func sign

```dart
void sign(ZkLinkSigner zkLinkSigner)
```

Sign transaction with given [ZkLinkSigner](../signer.md#type-zklinksigner)

### func toJson

```dart
String toJson()
```

Get the json str of [ChangePubKey](#type-changepubkey)

### func toEip712RequestPayload

```dart
String toEip712RequestPayload(int chainId, String address)
```

Get the EIP-712 structured data of [ChangePubKey](#type-changepubkey)

### func setEthAuthData

```dart
void setEthAuthData(String sig)
```

Set Ethereum authentication data with given EthECDSA signature

### Example

```dart
var zklinkSigner = ZkLinkSigner.ethSig(sig: "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001");
String pubkeyHash = zklinkSigner.getPubkeyHash();
print(pubkeyHash);
var tx = ChangePubKey(
    chainId: 1,
    accountId: 2,
    subAccountId: 4,
    newPubkeyHash: pubkeyHash,
    feeToken: 1,
    fee: "100",
    nonce: 100
);
tx.sign(zkLinkSigner: zklinkSigner);
print(tx.toEip712RequestPayload(chainId: 1, address: "0xa97153dd89c6f8F3BeA66190a6e62020aC7213de"));
String ethSignMsg = tx.getEthSignMsg(nonce: 100, accountId: 1);
tx.setEthAuthData(sig: "0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001b");
print(tx.toJson());
```
