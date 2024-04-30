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

## type Contract
The [Contract](../../../api-and-sdk/data-types/transaction/contract\_matching.md) struct of taker and maker in perpetual contract.

```dart
Contract(
    int accountId,
    int subAccountId,
    int slotId,
    int nonce,
    int pairId,
    String size,
    String price,
    bool direction,
    int makerFeeRate,
    int takerFeeRate,
    bool hasSubsidy,
)
```

## type ContractMatching
[ContractMatching](../../../api-and-sdk/data-types/transaction/contract\_matching.md) transaction type.

```dart
ContractMatching(
    int accountId,
    int subAccountId,
    Contract taker,
    List<Contract> maker,
    String fee,
    int feeToken,
    List<ContractPrice> contractPrices,
    List<SpotPriceInfo> marginPrices,
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

Get the json str of [ContractMatching](#type-contractmatching)
