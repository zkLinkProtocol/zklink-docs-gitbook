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

```json

```
