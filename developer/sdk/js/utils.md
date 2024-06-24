### func isTokenAmountPackable

```javascript
/**
* @param {string} amount
* @returns {boolean}
*/
isTokenAmountPackable(amount: string)
``` 
Checks whether the token amount can be packed (and thus used in the transaction)

### func isFeeAmountPackable

```javascript
/**
* @param {string} fee
* @returns {boolean}
*/
isFeeAmountPackable(fee: string)
``` 
Checks whether the fee amount can be packed (and thus used in the transaction)

### func closestPackableTransactionAmount

```javascript
/**
* @param {string} amount
* @returns {string}
*/
closestPackableTransactionAmount(amount: string)
``` 
Returns the closest possible packable token amount.
Returned amount is always less or equal to the provided amount.

### func closestPackableTransactionFee

```javascript
/**
* @param {string} fee
* @returns {string}
*/
closestPackableTransactionFee(fee: string)
``` 
Returns the closest possible packable fee amount.
Returned amount is always less or equal to the provided amount.

### Example
```javascript
let amount = "1234567899808787";
console.log("Original amount: " + amount);
console.assert(isTokenAmountPackable(amount) == false);
amount = wasm.closestPackableTransactionAmount(amount);
console.assert(isTokenAmountPackable(amount));
console.log("Converted amount: " + amount);
let fee = "10000567777";
console.log("Original fee: " + fee);
console.assert(isFeeAmountPackable(fee) == false);
fee = wasm.closestPackableTransactionFee(fee)
console.assert(isFeeAmountPackable(fee));
console.log("Converted fee: " + fee);
```
