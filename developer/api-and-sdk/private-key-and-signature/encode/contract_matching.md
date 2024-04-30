
# Contract encode

| Name  | Rule                                                             |
|-------|------------------------------------------------------------------|
|type | 1 byte, `0xfe`                                                   |
|accountId | 4 bytes                                                          |
|subAccountId| 1 byte                                                           |
|slotId | 2 bytes, use the lower 2 bytes of u32                            |
|nonce | 3 bytes, encode to 4 bytes in big endian, keep the `bytes[1..3]` |
|pairId | 1 byte, encode to 2 bytes in big endian, keep the lower byte     |
|direction | 1 byte                                                           |
|size | 5 bytes                                                          |
|price | 15 bytes                                                         |
|feeRates | 2 bytes                                                          |
| hasSubsidy | 1 byte                                                           |

# ContractMatching encode

| Name         | Rule                                    |
|--------------|-----------------------------------------|
| type         | 1 byte, `0x09`                        |
| accountId    | 4 bytes                                 |
| subAccountId | 1 byte                                  |
| maker,taker  | 31 bytes                                |
| fee          | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](#biguint-pack-algorithm) |
| feeToken     | 2 bytes                                 |

41 bytes in total, where the `maker` and `taker` encode as bellow:
* [Encode](#contract-encode) the items in `maker` into a bytes list in order;
* [Encode](#contract-encode) the taker and append to the bytes list;
* pass the bytes list to the `rescue_hash_orders` function in SDK, and get the 31 bytes result.

# Example

```json
{
  "type": "ContractMatching",
  "accountId": 1,
  "subAccountId": 2,
  "maker": [
    {
      "accountId": 1,
      "subAccountId": 2,
      "slotId": 1,
      "nonce": 2,
      "pairId": 3,
      "size": "100",
      "price": "200",
      "direction": 0,
      "feeRates": [
        10,
        20
      ],
      "hasSubsidy": 0,
      "signature": {
        "pubKey": "0x99575738f142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
        "signature": "957826468392052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
      }
    }
  ],
  "taker": {
    "accountId": 2,
    "subAccountId": 3,
    "slotId": 2,
    "nonce": 3,
    "pairId": 4,
    "size": "100",
    "price": "200",
    "direction": 1,
    "feeRates": [
      100,
      200
    ],
    "hasSubsidy": 0,
    "signature": {
      "pubKey": "0x43cbec0bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
      "signature": "366e759d61a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
    }
  },
  "fee": "100",
  "feeToken": 2,
  "signature": {
    "pubKey": "0x54dc0c0bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
    "signature": "1234567861a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
  }
}
encode_bytes = [9, 0, 0, 0, 1, 2, 94, 132, 152, 149, 63, 148, 7, 42, 244, 97, 249, 239, 244, 227, 182, 160, 60, 184, 251, 76, 217, 159, 248, 7, 140, 72, 246, 187, 137, 19, 213, 0, 2, 12, 128]
```
