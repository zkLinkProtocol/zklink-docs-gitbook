## type Funding
[Funding](../../../api-and-sdk/data-types/transaction/funding.md) transaction type.

```dart
Funding(
    int accountId,
	int subAccountId,
	int subAccountNonce,
	List<int> fundingAccountIds,
	String fee,
	int feeToken,
)
```

### func sign

```dart
void sign(ZkLinkSigner zkLinkSigner)
```

Sign transaction with given `ZkLinkSigner`

### func toJson

```dart
String toJson()
```

Get the json str of [Funding](#type-funding)
