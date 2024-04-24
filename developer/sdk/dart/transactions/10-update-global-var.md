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

## type FundingInfo

```dart
FundingInfo(
    int pairId,
    String price,
	int fundingRate,
)
```

## type Parameter
The [Parameter](../../../api-and-sdk/data-types/transaction/update_global_var.md) struct of [UpdateGlobalVar](#type-UpdateGlobalVar).

### func insuranceFundAccount

```dart
insuranceFundAccount(int accountId)
```

### func feeAccount

```dart
feeAccount(int accountId)
```

### func marginInfo

```dart
marginInfo(
	int marginId,
	String symbol,
	int tokenId,
	int ratio,
)
```

### func contractInfo

```dart
contractInfo(
	int pairId,
    String symbol,
	int initialMarginRate,
	int maintenanceMarginRate,
)
```

### func feeAccount

```dart
feeAccount(int accountId)
```

### func fundingInfos

```dart
fundingInfos(List<FundingInfo> infos)
```

## type UpdateGlobalVar
[UpdateGlobalVar](../../../api-and-sdk/data-types/transaction/update_global_var.md) transaction type.

```dart
UpdateGlobalVar(
    int fromChainId,
	int subAccountId,
	Parameter parameter,
	double serialId,
)
```

### func toJson

```dart
String toJson()
```

Get the json str of [UpdateGlobalVar](#type-UpdateGlobalVar)

### Example
```dart
var tx = UpdateGlobalVar(
	fromChainId: 1,
	subAccountId: 2,
	parameter: Parameter.feeAccount(accountId: 8),
	serialId: 101
);
print(tx.toJson());
```