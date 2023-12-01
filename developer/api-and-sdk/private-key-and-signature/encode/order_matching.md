
# Order encode
| Name         | Rule                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------|
| type         | 1 byte with value `0xff`                                                                        |
| accountId    | 4 bytes                                                                                         |
| subAccountId | 1 byte                                                                                          |
| slotId       | 2 bytes                                                                                         |
| nonce        | 4 bytes                                                                                         |
| baseTokenId  | 2 bytes                                                                                         |
| quoteTokenId | 2 bytes                                                                                         |
| price        | 15 bytes                                                                                        |
| isSell       | 1 byte                                                                                          |
| feeRates     | 2 bytes                                                                                         |
| hasSubsidy   | 1 byte                                                                                          |
| amount       | 5 bytes, refer to the `amount` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |

39 bytes in total.

# OrderMatching encode

| Name              | Rule                                                                                         |
| ----------------- |----------------------------------------------------------------------------------------------|
| type              | 1 byte with the value `0x08`                                                                 |
| accountId         | 4 bytes                                                                                      |
| subAccountId      | 1 byte                                                                                       |
| orderBytesHash    | 32 bytes, refer to Rust SDK `rescue_hash_orders`                                             |
| feeToken          | 2 bytes                                                                                      |
| fee               | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| expectBaseAmount  | 16 bytes                                                                                     |
| expectQuoteAmount | 16 bytes                                                                                     |

74 bytes in total.

# Example

```json
{
  "type": "OrderMatching",
  "accountId": 4,
  "subAccountId": 1,
  "taker": {
    "accountId": 11,
    "subAccountId": 1,
    "slotId": 5844,
    "nonce": 24,
    "baseTokenId": 42,
    "quoteTokenId": 1,
    "amount": "373400000000000000000",
    "price": "1210900000000000000",
    "isSell": 1,
    "feeRates": [5, 10],
    "hasSubsidy": 1,
    "signature": {
      "pubKey": "0x1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
      "signature": "3a5c5c23a74cc04d256f03eb1671acd3959d707252ed52b16cb7b2ffe332a804986e09e0d62bcf49fc9231b38f07b71199769a7343eddc8b43ed9dd2ef8a4405"
    }
  },
  "maker": {
    "accountId": 11,
    "subAccountId": 1,
    "slotId": 5915,
    "nonce": 14,
    "baseTokenId": 42,
    "quoteTokenId": 1,
    "amount": "5165400000000000000000",
    "price": "1210900000000000000",
    "isSell": 0,
    "feeRates": [5, 10],
    "hasSubsidy": 0,
    "signature": {
      "pubKey": "0x1aedae58e43fe6661db7f834ae438930443908d108fdf621bfd4741fedfcd82f",
      "signature": "9579e54f53aa709e72c7e4de9815d258cf92bb3e9c4b9d03c2f79a7a49b5bda062d7e81b278eb62f2452294d178728b460efdb80017c83748dd190e41e05b802"
    }
  },
  "fee": "405000000000000",
  "feeToken": 1,
  "expectBaseAmount": "373400000000000000000",
  "expectQuoteAmount": "452150060000000000000",
  "signature": {
    "pubKey": "0x84bf4edbe1f7056f079ba4c38359427f43d529fbab2e94e6d6b7a18efbf2fb87",
    "signature": "1242830780b17dd362e8d31952deab6d8b5d81cadd62779b2caab0821baa030a770afa9a2c249d8f45000c4b9c6f01ef6b002682760e5bc4e5d39ef7f511ce03"
  }
}
encode_bytes = [8, 0, 0, 0, 4, 1, 37, 108, 168, 155, 185, 27, 201, 224, 122, 31, 189, 115, 53, 97, 11, 151, 14, 133, 10, 198, 175, 169, 42, 38, 200, 180, 166, 37, 230, 119, 110, 0, 1, 50, 172, 0, 0, 0, 0, 0, 0, 0, 20, 61, 247, 73, 164, 90, 220, 0, 0, 0, 0, 0, 0, 0, 0, 0, 24, 130, 215, 179, 249, 239, 142, 192, 0]
```
