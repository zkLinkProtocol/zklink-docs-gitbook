
# Encode

| Name         | Rule                                                   |
|--------------|--------------------------------------------------------|
|type | 1 byte, `0x0d`                                       |
| accountId         | 4 bytes                                                |
| subAccountId      | 1 byte                                                 |
| accountIdNonce | 4 bytes                                                |
| fundingAccountIds | 4 bytes(when length is 1) or 31 bytes(when length > 1) |
| fee | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm)                |
| feeToken | 2 bytes                                                |

The encoding process of `fundingAccountIds` is as blew:
* If the length is 1, encode the item directly in big endian;
* If the length > 1, encode the item in big endian into a bytes list in order;
* Pass the bytes list to the `rescue_hash` function in Rust SDK, and get the 31 bytes result.

# Example

For the `fundingAccountIds` length is 1:

```json

{
  "accountId": 1,
  "subAccountId": 2,
  "subAccountNonce": 3,
  "fundingAccountIds": [ 1 ],
  "fee": "0",
  "feeToken": 0,
  "signature": {
    "pubKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "signature": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
  }
}
```

the encode result is:

```json

```

For the `fundingAccountIds` length is larger than 1:

```json
{
  "accountId": 1,
  "subAccountId": 2,
  "subAccountNonce": 3,
  "fundingAccountIds": [ 1, 2, 3 ],
  "fee": "0",
  "feeToken": 0,
  "signature": {
    "pubKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "signature": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
  }
}
```

the encode result will be:

```json

```
