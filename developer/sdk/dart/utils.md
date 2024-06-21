### func isTokenAmountPackable

```dart
bool isTokenAmountPackable(String amount)
```
Checks whether the token amount can be packed (and thus used in the transaction)

### func isFeeAmountPackable

```dart
bool isFeeAmountPackable(String fee)
```
Checks whether the fee amount can be packed (and thus used in the transaction)

### func closestPackableTokenAmount

```dart
String closestPackableTokenAmount(String amount)
```
Returns the closest possible packable token amount.
Returned amount is always less or equal to the provided amount.

### func closestPackableFeeAmount

```dart
String closestPackableFeeAmount(String fee)
```
Returns the closest possible packable fee amount.
Returned amount is always less or equal to the provided amount.

### Example
```dart
var amount = "1234567899808787";
print("Original amount: " + amount);
expect(isTokenAmountPackable(amount: amount), false);
amount = closestPackableTokenAmount(amount: amount);
expect(isTokenAmountPackable(amount: amount), true);
print("Converted amount: " + amount);
var fee = "10000567777";
print("Original fee: " + fee);
expect(isFeeAmountPackable(fee: fee), false);
fee = closestPackableFeeAmount(fee: fee);
expect(isFeeAmountPackable(fee: fee), true);
print("Converted fee: " + fee);
```
