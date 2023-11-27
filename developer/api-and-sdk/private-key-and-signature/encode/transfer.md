# Transfer encode
| Name             | Rule                                                                                            |
| ---------------- |-------------------------------------------------------------------------------------------------|
| type             | 1 byte with value `0x04`                                                                        |
| accountId        | 4 bytes                                                                                         |
| fromSubAccountId | 1 byte                                                                                          |
| to               | 32 bytes, extended to 32 bytes with prefix `0x00` prefix                                        |
| toSubAccountId   | 1 byte                                                                                          |
| token            | 2 bytes                                                                                         |
| amount           | 5 bytes, refer to the `amount` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| feeAmount        | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm)    |
| nonce            | 4 bytes                                                                                         |
| ts               | 4 bytes                                                                                         |

56 bytes in total.

# Example

```json

```
the encode result is

```json

```
