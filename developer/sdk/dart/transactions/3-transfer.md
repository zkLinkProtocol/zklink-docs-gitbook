## type Transfer
[Transfer](../../../api-and-sdk/data-types/transaction/transfer.md) transaction type.

```dart
Transfer(
	accountId,
	String toAddress,
	int fromSubAccountId,
	int toSubAccountId,
	int token,
	String fee,
	String amount,
	int nonce,
	int? ts,
)
```

### func getEthSignMsg

```dart
String getEthSignMsg(String tokenSymbol)
```

Get the Ethereum sign message

### func sign

```dart
void sign(ZkLinkSigner zkLinkSigner)
```

Sign transaction with given `ZkLinkSigner`

### func toJson

```dart
String toJson()
```

Get the json str of [Transfer](#type-transfer)
