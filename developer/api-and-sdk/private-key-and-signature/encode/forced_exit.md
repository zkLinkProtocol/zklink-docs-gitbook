
<table><thead><tr><th width="248">Name</th><th>Rule</th></tr></thead><tbody>
<tr><td>type</td><td>1 byte with the value "7"</td></tr>
<tr><td>toChainId</td><td>1 byte</td></tr>
<tr><td>initiatorAccountId</td><td>4 bytes</td></tr>
<tr><td>initiatorSubAccountId</td><td>1 byte</td></tr>
<tr><td>target</td><td>20 bytes</td></tr>
<tr><td>targetSubAccountId</td><td>1 byte</td></tr>
<tr><td>l3SourceToken</td><td>2 bytes</td></tr>
<tr><td>l1TargetToken</td><td>2 bytes</td></tr>
<tr><td>initiatorNonce</td><td>4 bytes</td></tr>
<tr><td>exitAmount</td><td>Refer to the SDK <strong>serializeAmountFull</strong>, 16 bytes</td></tr>
<tr><td>ts</td><td>4 bytes</td></tr>
</tbody></table>

56 bytes in total.

# Example
```json
{
  "type": "ForcedExit",
  "toChainId": 1,
  "initiatorAccountId": 7,
  "initiatorSubAccountId": 3,
  "initiatorNonce":4,
  "target": "0x3498f456645270ee003441df82c718b56c0e6666",
  "targetSubAccountId": 2,
  "l2SourceToken": 1,
  "l1TargetToken": 17,
  "exitAmount": "4100000000000000",
  "withdrawToL1": 1,
  "ts": 1646102148,
  "nonce": 0,
  "signature": {
    "pubKey": "0x0dd4f603531bd78bbecd005d9e7cc62a794dcfadceffe03e269fbb6b72e9c724",
    "signature": "a8719d0f771f34a177bbf199ab7b0decd03b5db29edf173ed980d19c7864c5a3761111620ab1982ef1bb7459d5a919727e51b895799e2706ddd5a5328146eb01"
  }
}
encode_bytes = [7, 1, 0, 0, 0, 7, 3, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 52, 152, 244, 86, 100, 82, 112, 238, 0, 52, 65, 223, 130, 199, 24, 181, 108, 14, 102, 102, 2, 0, 1, 0, 17, 0, 0, 0, 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 14, 144, 237, 163, 148, 64, 0, 1, 98, 29, 134, 132]
```
