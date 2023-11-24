
# ChangePubKey encode

| Name         | Rule                                                                                         |
| ------------ |----------------------------------------------------------------------------------------------|
| type         | 1 byte with value `0x06`                                                                     |
| chainId      | 1 byte                                                                                       |
| accountId    | 4 bytes                                                                                      |
| subAccountId | 1 byte                                                                                       |
| newPkHash    | 20 bytes                                                                                     |
| feeToken     | 2 bytes                                                                                      |
| fee          | 2 bytes, refer to the `fee` pack method in [BigUint pack algorithm](#BigUint-pack-algorithm) |
| nonce        | 4 bytes                                                                                      |
| ts           | 4 bytes                                                                                      |

39 bytes in total.

# Example

```json
{
  "type": "ChangePubKey",
  "chainId": 1,
  "accountId": 39,
  "subAccountId": 1,
  "newPkHash": "0xbfb4f4a68dc9e49f7785082a8c12354ed663b6e0",
  "feeToken": 1,
  "fee": "1285000000000000",
  "nonce": 0,
  "signature": {
    "pubKey": "0xed53a138751ed1e456f46e74eff3463d2420e488a4f608bde0f28d13c7104d29",
    "signature": "3b91c0421df4295281596746722ae20ccf270c5fc0561f93a0219db1faea6518f033e778dd552f90a9a6afd06427428b2ac4ea6f6893a3f162b32683d1108a02"
  },
  "ethAuthData": {
    "type": "EthECDSA",
    "ethSignature": "0x8e548e3727a94533b3963877b87966e308e6eef7762f78de567ff14b4e0e87780d37a845501ffd2cdbc7d6f0d620c14589212761f1637ea8214b0b6bac10aa9b1b"
  },
  "ts": 1675650037
}
```

the encode result will be:

```json

```
