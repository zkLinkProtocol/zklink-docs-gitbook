# Transfer encode
| Name             | Rule                                                                                            |
| ---------------- |-------------------------------------------------------------------------------------------------|
| type             | 1 byte with value `0x04`                                                                        |
| accountId        | 4 bytes                                                                                         |
| fromSubAccountId | 1 byte                                                                                          |
| to               | 32 bytes, extended to 32 bytes with prefix `0x00` prefix                                        |
| toSubAccountId   | 1 byte                                                                                          |
| token            | 2 bytes                                                                                         |
| amount           | 5 bytes, refer to the `amount` pack method in [BigUint pack algorithm](#biguint-pack-algorithm) |
| feeAmount        | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#biguint-pack-algorithm)    |
| nonce            | 4 bytes                                                                                         |
| ts               | 4 bytes                                                                                         |

56 bytes in total.

# Example

```json
{
  "accountId": 10,
  "fromSubAccountId": 1,
  "toSubAccountId": 1,
  "to": "0xafaff3ad1a0425d792432d9ecd1c3e26ef2c42e9",
  "token": 18,
  "amount": "10000",
  "fee": "3",
  "nonce": 1,
  "signature": {
    "pubKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "signature": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
  },
  "ts": 1693472232
}
encode_bytes = [ 4, 0, 0, 0, 10, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 175, 175, 243, 173, 26, 4, 37, 215, 146, 67, 45, 158, 205, 28, 62, 38, 239, 44, 66, 233, 1, 0, 18, 0, 0, 4, 226, 0, 0, 96, 0, 0, 0, 1, 100, 240, 85, 232 ]
```
