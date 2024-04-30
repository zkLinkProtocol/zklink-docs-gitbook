# Withdraw encode

| Name             | Rule                                                                                             |
|------------------|--------------------------------------------------------------------------------------------------|
| type             | 1 byte with value `0x03`                                                                         |
| toChainId        | 1 byte                                                                                           |
| accountId        | 4 bytes                                                                                          |
| subAccountId     | 1 byte                                                                                           |
| to               | 32 bytes, extend to 32 bytes with `0x00` prefix                                                  |
| l2SourceToken    | 2 bytes                                                                                          |
| l1TargetToken    | 2 bytes                                                                                          |
| amount           | 16 bytes, refer to the `amount` pack method in [BigUint pack algorithm](#biguint-pack-algorithm) |
| fee              | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#biguint-pack-algorithm)     |
| nonce            | 4 bytes                                                                                          |
| withdrawToL1     | 1 byte                                                                                           |
| withdrawFeeRatio | 2 bytes                                                                                          |
| callData         | No limit, be used to send to layer1 to call a smart contract                                     |
| ts               | 4 bytes                                                                                          |

72 bytes in total.

# Example

```json
{
  "toChainId": 1,
  "accountId": 10,
  "subAccountId": 1,
  "to": "0xafaff3ad1a0425d792432d9ecd1c3e26ef2c42e9",
  "l2SourceToken": 18,
  "l1TargetToken": 18,
  "amount": "10000",
  "callData": null,
  "fee": "3",
  "nonce": 1,
  "signature": {
    "pubKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "signature": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
  },
  "withdrawToL1": 0,
  "withdrawFeeRatio": 0,
  "ts": 1693472232
}
excode_bytes = [35, 38, 100, 21, 162, 218, 169, 88, 46, 176, 84, 204, 61, 64, 69, 248, 70, 224, 44, 240, 208, 221, 29, 8, 236, 225, 227, 255, 131, 200, 226, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

