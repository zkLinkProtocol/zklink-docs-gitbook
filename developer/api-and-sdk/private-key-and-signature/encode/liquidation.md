# Encode ContractPrice

| Name   | Rule                                                                              |
|--------|-----------------------------------------------------------------------------------|
| pariId | 1 byte                                                                            |
|marketPrice| 15 bytes, encode in big endian, then pass to the `pad_front` function in Rsut SDK |

# Encode OrderPriceInfo

| Name        | Rule                                                                                  |
|-------------|---------------------------------------------------------------------------------------|
| tokenId     | 2 byte, change to `u16`, then encode in big endian                                    |
| marketPrice | 15 bytes, encode in big endian, then pass to the`pad_front` function in Rust SDK |


# Encode Liquidation

| Name         | Rule                                    |
|--------------|-----------------------------------------|
|type | 1 byte, `0x0d`                        |
| accountId         | 4 bytes                                 |
| subAccountId      | 1 byte                                  |
| accountIdNonce | 4 bytes                                 |
| oraclePrices | 31 bytes                                |
| liquidationAccountId | 4 bytes                                 |
| fee | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| feeToken | 2 bytes                                 |

49 bytes in total, where the `oraclePrices` encode process is as blew:

* Encode the `oraclePrices` into a bytes list in order;
* Pass the bytes list to the SDK `rescue_hash` function then get the 31 bytes result.

# Example


```json
{
  "type": "Liquidation",
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
  "liquidationAccountId": 0,
  "fee": "0",
  "feeToken": 0,
  "signature": {
    "pubKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "signature": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
  }
}
```

the encode result is

```json

```
