# Parameter Encode

## feeAccount

| Name         | Rule          |
|--------------|---------------|
| type         | 1 byte, `0x00` |
| feeAccount  | 4 byte        |

5 bytes in total

## insuranceFundAccount

| Name         | Rule                        |
|--------------|-----------------------------|
| type | 1 byte, `0x01` |
| accountId | 4 bytes |

5 bytes in total

## marginInfo

| Name     | Rule                                                |
|----------|-----------------------------------------------------|
| type     | 1 byte, `0x02`                                      |
| marginId | 1 bytes                                             |
| tokenId  | 2 bytes, downcast to u16, then encode in big endian |
| ratio    | 1 byte                                              |

## contractInfo

| Name                  | Rule                                                      |
|-----------------------|-----------------------------------------------------------|
| type                  | 1 byte, `0x03`                                            |
| pariId                | 1 bytes, downcast to u8 as 1 byte                         |
| symbol                | 15 bytes, expand to 15 bytes with `0x00` at the beginning |
| initialMarginRate     | 2 bytes                                                   |
| maintenanceMarginRate | 2 bytes                                                   |

## fundingInfos

| Name        | Rule                                 |
|-------------|--------------------------------------|
| type | 1 byte, `0x04`                        |
|infos | 18 bytes * the length of fundingRates, encode `fundingInfo` in order |

where `fundingInfo` encode rule is:

| Name        | Rule                                               |
|-------------|----------------------------------------------------|
| pariId      | 1 bytes, downcast to u8 as 1 byte                  |
| price       | 15 bytes, refer to the Rust SDK `pad_front` method |
| fundingRate | 2 bytes                                            |

18 bytes in total, where `fundingRate` encode is as following:

* Get the absolute result as `u16` type，then encode as 2 bytes in big endian.
* If fundingRate < 0: `bytes[0] |= 0b1000_0000`

# UpdateGlobalVar Encode

| Name         | Rule                        |
|--------------|-----------------------------|
| type         | 1 byte, `0x0c`              |
| fromChainId  | 1 byte                      |
| subAccountId | 4 bytes                     |
| parameter    | refer to the Parameter encode |
| serialId | 8 bytes                     |

# Example

With FundingInfo encode:

```json
{
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "fundingInfos": {
      "infos": [
        {
          "pairId": 0,
          "price": "1000000000000000000",
          "fundingRate": 32767
        },
        {
          "pairId": 1,
          "price": "1000000000000000",
          "fundingRate": 0
        },
        {
          "pairId": 2,
          "price": "1000000000000",
          "fundingRate": -1
        },
        {
          "pairId": 3,
          "price": "1000000000",
          "fundingRate": 1
        }
      ]
    }
  },
  "serialId": 0
}
encode_bytes = [12, 1, 1, 4, 0, 0, 0, 0, 0, 0, 0, 0, 13, 224, 182, 179, 167, 100, 0, 0, 127, 255, 1, 0, 0, 0, 0, 0, 0, 0, 0, 3, 141, 126, 164, 198, 128, 0, 0, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 232, 212, 165, 16, 0, 128, 1, 3, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 59, 154, 202, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0]
```

With FeeAccount encode:

```json
{
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "feeAccount": {
      "accountId": 10
    }
  },
  "serialId": 0
}
encode_bytes = [12, 1, 1, 0, 0, 0, 0, 10, 0, 0, 0, 0, 0, 0, 0, 0]
```

with insuranceFundAccount encode:

```json
{
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "insuranceFundAccount": {
      "accountId": 9
    }
  },
  "serialId": 0
}
encode_bytes = [12, 1, 1, 1, 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 0, 0]
```

With MarginInfo encode:

```json
{
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "marginInfo": {
      "marginId": 1,
      "tokenId": 9,
      "ratio": 0
    }
  },
  "serialId": 0
}
encode_bytes = [12, 1, 1, 2, 1, 0, 9, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

With ContractInfo encode:

```json
{
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "contractInfo": {
      "pairId": 2,
      "symbol": "BTCUSDC",
      "initialMarginRate": 6,
      "maintenanceMarginRate": 8
    }
  },
  "serialId": 0
}
encode_bytes = [12, 1, 1, 3, 2, 0, 0, 0, 0, 0, 0, 0, 0, 66, 84, 67, 85, 83, 68, 67, 0, 6, 0, 8, 0, 0, 0, 0, 0, 0, 0, 0]
```
