## type Withdraw
[Withdraw](../../../api-and-sdk/data-types/transaction/withdraw.md) transaction type.

```dart
Withdraw(
	int accountId,
	int subAccountId,
	int toChainId,
	String toAddress,
	int l2SourceToken,
	int l1TargetToken,
	String amount,
	String? callData,
	String fee,
	int nonce,
	bool withdrawToL1,
	int withdrawFeeRatio,
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

Get the json str of [Withdraw](#type-Withdraw)
