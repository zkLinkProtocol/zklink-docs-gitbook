## type ContractPrice

```dart
ContractPrice(
    int pairId,
    String marketPrice,
)
```

**input:**
* pairId: The contract pair id defined by zkLink.
* marketPrice: The market price of the contract pair

## type SpotPriceInfo

```dart
SpotPriceInfo(
    int tokenId,
    String price,
)
```

**input:**
* tokenId: The token id defined by zkLink.
* price: The spot price of the token.

## type AutoDeleveraging
[AutoDeleveraging](../../../api-and-sdk/data-types/transaction/auto\_deleveraging.md) transaction type.

```dart
AutoDeleveraging(
    int accountId,
	int subAccountId,
	int subAccountNonce,
	List<ContractPrice> contractPrices,
	List<SpotPriceInfo> marginPrices,
	int adlAccountId,
	int pairId,
	String adlSize,
	String adlPrice,
	String fee,
	int feeToken,
)
```

### func sign

```dart
void sign(ZkLinkSigner zkLinkSigner)
```

Sign transaction with given [ZkLinkSigner](../signer.md#type-zklinksigner)

### func toJson

```dart
String toJson()
```

Get the json str of [AutoDeleveraging](#type-autodeleveraging)
