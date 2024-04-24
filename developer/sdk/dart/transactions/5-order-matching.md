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

## type Order
The [Order](../../../api-and-sdk/data-types/transaction/order_matching.md) struct of taker and maker.

```dart
Order(
    int accountId,
    int subAccountId,
    int slotId,
    int nonce,
    int baseTokenId,
    int quoteTokenId,
    String amount,
    String price,
    bool isSell,
    int makerFeeRate,
    int takerFeeRate,
    bool hasSubsidy,
)
```

## type OrderMatching
[OrderMatching](../../../api-and-sdk/data-types/transaction/order_matching.md) transaction type.

```dart
OrderMatching(
    int accountId,
    int subAccountId,
    Order taker,
    Order maker,
    String fee,
    int feeToken,
    List<ContractPrice> contractPrices,
    List<SpotPriceInfo> marginPrices,
    String expectBaseAmount,
    String expectQuoteAmount,
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

Get the json str of [OrderMatching](#type-OrderMatching)
