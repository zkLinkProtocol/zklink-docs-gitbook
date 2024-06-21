### func isTokenAmountPackable

```go
func IsTokenAmountPackable(amount BigUint) bool
```
Checks whether the token amount can be packed (and thus used in the transaction)

### func isFeeAmountPackable

```go
func IsFeeAmountPackable(fee BigUint) bool
```
Checks whether the fee amount can be packed (and thus used in the transaction)

### func closestPackableTokenAmount

```go
func ClosestPackableTokenAmount(amount BigUint) BigUint
```
Returns the closest possible packable token amount.
Returned amount is always less or equal to the provided amount.

### func closestPackableFeeAmount

```go
func ClosestPackableFeeAmount(fee BigUint) BigUint
```
Returns the closest possible packable fee amount.
Returned amount is always less or equal to the provided amount.

### Example
```go
amount := *big.NewInt(1234567899808787)
fmt.Println("Original amount: ", amount)
amount = sdk.ClosestPackableTokenAmount(amount)
fmt.Println("Converted amount:s", amount)
fee := *big.NewInt(10000567777)
fmt.Println("Original fee: ", fee)
fee = sdk.ClosestPackableFeeAmount(fee)
fmt.Println("Converted fee: ", fee)
```
