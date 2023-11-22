## Deposit

Deposit from Layer 1 to [zkLink layer，详情]().


| Name          | Type                                   | Mandatory | Description                                                                                     |
|---------------|----------------------------------------|-----------|-------------------------------------------------------------------------------------------------|
| type          | String                                 | yes       | The value is "Depsite"                                                                          |
| fromChainId   | [ChainId](../data_types.md#ChainId)    | yes       | The chain id defined  by zkLink, the chain that the deposit is initiated on                     |
| from          | String                                 | yes       | The initiator address of the deposit                                                            |
| to            | String                                 | yes       | The recipient of the deposit. An account will be created if it does not exist on zkLink Layer 2 |
| subAccountId  | SubAccountId                           | yes       | The subaccount id of the recipient                                                              |
| l1SourceToken | TokenId                                | yes       | The token deducted from the initiator on Layer 1                                                |
| l2TargetToken | TokenId                                | yes       | The token received by the recipient on zkLink Layer 2                                           |
| amount        | BigUint                                | yes       | The amount of deposit                                                                           |
| serialId      | u64                                    | yes       | The serial number of the event, used as nonce                                                   |
| ethHash       | TxHash                                 | yes       | The transaction that generated this event on Layer 1                                            |

For example:

```json
{
    "type": "Deposit",
    "fromChainId": 1,
    "from": "0x76920dfacad4f28f97d6209977c1057b9e3e5cad",
    "subAccountId": 1,
    "l1SourceToken": 18,
    "l2TargetToken": 1,
    "amount": "4000000000000000000000",
    "to": "0x76920dfacad4f28f97d6209977c1057b9e3e5cad",
    "serialId": 53,
    "ethHash": "0xaaa1e7a5bc48e7cfaa562a4d1a5abc1d6dc5e7f7683e89eb00e895d438f0acab"
}
```
