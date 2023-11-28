
<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>

<tr><td> type          </td><td> String       </td><td> yes       </td><td> The value is "FullExit"                                              </td></tr>
<tr><td> toChainId     </td><td> <a href="../data_types.md#chainid">ChainId</a>      </td><td> yes       </td><td> The chain id defined by zkLink, to which the user wish to withdrawal </td></tr>
<tr><td> accountId     </td><td> <a href="../data_types.md#accountid">AccountId    </a></td><td> yes       </td><td> The id of the withdrawal account                                     </td></tr>
<tr><td> subAccountId  </td><td> <a href="../data_types.md#subaccountid">SubAccountId </a></td><td> yes       </td><td> The id of the subaccount for withdrawal                              </td></tr>
<tr><td> exitAddress   </td><td> String       </td><td> yes       </td><td> The Layer1 address of the recipient                                  </td></tr>
<tr><td> l2SourceToken </td><td> <a href="../data_types.md#tokenid">TokenId      </a></td><td> yes       </td><td> The token deducted from the withdrawal account on Layer3            </td></tr>
<tr><td> l1TargetToken </td><td> <a href="../data_types.md#tokenid">TokenId</a></td><td> yes       </td><td> The token received by the recipient on Layer 1                       </td></tr>
<tr><td> serialId      </td><td> u64          </td><td> yes       </td><td> The serial number of the event, used as nonce                        </td></tr>
<tr><td> ethHash       </td><td> <a href="../data_types.md#txhash">TxHash</a></td><td> yes       </td><td> The transaction hash that generated this event on Layer 1            </td></tr>
</tbody>
</table>

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
