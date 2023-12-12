## ContractPrice

## SpotPrice

## OrderMatchingBuilder

```go
type OrderMatchingBuilder struct {
    AccountId         AccountId
    SubAccountId      SubAccountId
    Taker             *Order
    Maker             *Order
    Fee               BigUint
    FeeToken          TokenId
    ContractPrices    []ContractPrice
    MarginPrices      []SpotPriceInfo
    ExpectBaseAmount  BigUint
    ExpectQuoteAmount BigUint
}
```
