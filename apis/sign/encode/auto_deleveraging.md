# AutoDeleveraging encode

| Name  | Rule                                                                                                                   |
|-------|------------------------------------------------------------------------------------------------------------------------|
| type  | 1byte with the value `0x0b`                                                                                            |
| accountId | 4 bytes                                                                                                                |
| subAccountId    | 1 byte                                                                                                                 |
| subAccountNonce | 4 bytes                                                                                                                |
| oraclePrices    | 31 bytes, encode the `ContractPrice` and `MarginPrice` in order, pass the bytes to Rust SDK `rescue_hash` method |
| adlAccountId    | 4 bytes                                                                                                                |
| pairId          | 1 byte                                                                                                                 |
| adlSize         | 5 bytes, refer to `amount` pack method in [BigUint pack algorithm](../algorithm.md#BigUint-pack-algorithm)             |
| adlPrice        | 15 bytes, encode to big endian bytes, then pass to the`pad_front` function in Rust SDK                                 |
| fee             | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](../algorithm.md#BigUint-pack-algorithm)                |
| feeToken        | 2 bytes                                                                                                                |


# ContractPrice encode

| Name   | Rule                                                                              |
|--------|-----------------------------------------------------------------------------------|
| pariId | 1 byte                                                                            |
|marketPrice| 15 bytes, encode in big endian, then pass to the `pad_front` function in Rsut SDK |

# MarginPrice encode

| Name        | Rule                                                                                  |
|-------------|---------------------------------------------------------------------------------------|
| tokenId     | 2 byte, change to `u16`, then encode in big endian                                    |
| marketPrice | 15 bytes, encode in big endian, then pass to the`pad_front` function in Rust SDK |


## Example
For the auto deleveraging as below:

```json
{
  "type": "AutoDeleveraging",
  "accountId": 0,
  "subAccountId": 0,
  "subAccountNonce": 0,
  "oraclePrices": {
    "contractPrices": [
      {
        "pairId": 1,
        "marketPrice": "100"
      }
    ],
    "marginPrices": [
      {
        "tokenId": 1,
        "price": "100"
      }
    ]
  },
  "adlAccountId": 0,
  "pairId": 0,
  "adlSize": "0",
  "adlPrice": "0",
  "fee": "100",
  "feeToken": 1,
  "signature": {
    "pubKey": "0x43cbec0bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
    "signature": "366e759d61a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
  }
}
```

the encode result will be:

```json

```
