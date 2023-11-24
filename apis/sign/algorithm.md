# BigUint pack algorithm
The integer is composed of `mantissa` and `exponent` part of encoding, and pack algorithm is:

> `integer = mantissa * (exponent_base^exponent)`

where the `exponent_base` is `10`. This compression algorithm is mainly targeted at two types of data `fee` and `amount`:

* fee: 11 bits of mantissa and 5bits of exponent, total in 2 bytes
* amount: 35 bits of mantissa and 5bits of exponent, total in 5 bytes


## max value limitation of algorithm
For the `amount`,  the max mantissa is `2^35 - 1`, and the max exponent is `2^5 - 1` or `31`. According to the  pack algorithm,  when exponent larger than 30,
the result will exceed `u128`, so we use `30` as the max value of exponent, the max value of `amount` is

>  max_amount = (2^35 - 1) * 10^30 = 34359738367000000000000000000000000000

For the `fee`, the max mantissa is `2^11 - 1`, and the max exponent is `2^5 - 1` or `31`, which make the max value of `fee` is

> max_fee = (2^11 - 1) * 10^31 = 20470000000000000000000000000000000

## Numerical precision loss

According to the formula, it can be seen that the compression algorithm will be numerical precision loss. For the numbers smaller than `2^128` will be changed after compression and cannot be recovered.
For example, a `fee` integer is `(2^10+1)*10^2+1 = 102501` where the `mantissa` is `2^10+1=101` and the `exponent` is `2`, the pack result will be `(2^10+1)*10^2 = 102500` which results in a `1` numerical precision loss.

`version: 4457a91`
