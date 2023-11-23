TODO: describe the full exit transaction

| Name          | Type         | Required | Description                                                          |
|---------------|--------------|-----------|----------------------------------------------------------------------|
| type          | String       | yes       | The value is "FullExit"                                              |
| toChainId     | ChainId      | yes       | The chain id defined by zkLink, to which the user wish to withdrawal |
| accountId     | AccountId    | yes       | The id of the withdrawal account                                     |
| subAccountId  | SubAccountId | yes       | The id of the subaccount for withdrawal                              |
| exitAddress   | String       | yes       | The Layer1 address of the recipient                                  |
| l2SourceToken | TokenId      | yes       | The token deducted from the withdrawal account on Layer 2            |
| l1TargetToken | TokenId      | yes       | The token received by the recipient on Layer 1                       |
| serialId      | u64          | yes       | The serial number of the event, used as nonce                        |
| ethHash       | TxHash       | yes       | The transaction hash that generated this event on Layer 1            |

For example:

```json
{
    "type": "FullExit",
    "toChainId": 1,
    "accountId": 25,
    "subAccountId": 1,
    "exitAddress": "0xae08c2e27765faef5cb05908dbac12242caf91af",
    "l2SourceToken": 47,
    "l1TargetToken": 47,
    "serialId": 43,
    "ethHash": "0x748d32538f71d937d9e2c47adc26c499d0451b87e4fd337c2d6190c3271dafd7"
}
```
