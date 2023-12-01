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
| amount           | 16 bytes, refer to the `amount` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| fee              | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm)     |
| nonce            | 4 bytes                                                                                          |
| withdrawToL1     | 1 byte                                                                                           |
| withdrawFeeRatio | 2 bytes                                                                                          |
| ts               | 4 bytes                                                                                          |

72 bytes in total.

# Example

```json
{
  "type": "Withdraw",
  "toChainId": 1,
  "accountId": 7,
  "subAccountId": 2,
  "to": "0x3498f456645270ee003441df82c718b56c0e6666",
  "l2SourceToken": 1,
  "l1TargetToken": 17,
  "amount": "995900000000000000",
  "fee": "4100000000000000",
  "withdrawToL1": 1,
  "withdrawFeeRatio": 50,
  "ts": 1646102148,
  "nonce": 0,
  "signature": {
    "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
  }
}
encode_bytes = [3, 1, 0, 0, 0, 7, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 52, 152, 244, 86, 100, 82, 112, 238, 0, 52, 65, 223, 130, 199, 24, 181, 108, 14, 102, 102, 0, 1, 0, 17, 0, 0, 0, 0, 0, 0, 0, 0, 13, 210, 37, 198, 3, 207, 192, 0, 51, 77, 0, 0, 0, 0, 1, 0, 50, 98, 29, 134, 132]
```

