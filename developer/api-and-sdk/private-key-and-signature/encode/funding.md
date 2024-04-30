
# Encode

| Name         | Rule                                                   |
|--------------|--------------------------------------------------------|
|type | 1 byte, `0x0d`                                       |
| accountId         | 4 bytes                                                |
| subAccountId      | 1 byte                                                 |
| accountIdNonce | 4 bytes                                                |
| fundingAccountIds | 4 bytes(when length is 1) or 31 bytes(when length > 1) |
| fee | 2 bytes, refer to `fee` pack method in [BigUint pack algorithm](#biguint-pack-algorithm)                |
| feeToken | 2 bytes                                                |

The encoding process of `fundingAccountIds` is as blew:
* If the length is 1, encode the item directly in big endian;
* If the length > 1, encode the item in big endian into a bytes list in order;
* Pass the bytes list to the `rescue_hash` function in Rust SDK, and get the 31 bytes result.

# Example

For the `fundingAccountIds` length is 1:

```json
{
  "type": "Funding",
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
[13, 0, 0, 0, 1, 2, 0, 0, 0, 3, 0, 0, 0, 1, 0, 0, 0, 0]
```

For the `fundingAccountIds` length is larger than 1:

```json
{
  "type": "Funding",
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
encode_bytes = [13, 0, 0, 0, 1, 2, 0, 0, 0, 3, 229, 21, 12, 161, 198, 143, 182, 230, 55, 168, 20, 44, 52, 201, 150, 10, 189, 128, 111, 32, 198, 206, 155, 157, 175, 56, 77, 76, 234, 70, 83, 0, 0, 0, 0]
```
