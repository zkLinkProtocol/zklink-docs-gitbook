# Deposit

Deposit from Layer 1 to [zkLink layer](deposit.md).


<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr></tr><td> type          </td><td> String                                 </td><td> yes       </td><td> The value is "Deposit"                                                                          </td></tr>
<tr></tr><td> fromChainId   </td><td> <a href="../basic-types.md#chainid">ChainId</a>    </td><td> yes       </td><td> The chain id defined  by zkLink, the chain that the deposit is initiated on                     </td></tr>
<tr></tr><td> from          </td><td> String                                 </td><td> yes       </td><td> The initiator address of the deposit                                                            </td></tr>
<tr></tr><td> to            </td><td> String                                 </td><td> yes       </td><td> The recipient of the deposit. An account will be created if it does not exist on zkLink Layer3 </td></tr>
<tr></tr><td> subAccountId  </td><td> <a href="../basic-types.md#subaccountid"SubAccountId>SubAccountId</a>                           </td><td> yes       </td><td> The subaccount id of the recipient                                                              </td></tr>
<tr></tr><td> l1SourceToken </td><td> <a href="../basic-types.md#tokenid">TokenId</a>                                </td><td> yes       </td><td> The token deducted from the initiator on Layer 1                                                </td></tr>
<tr></tr><td> l2TargetToken </td><td> <a href="../basic-types.md#tokenid">TokenId</a></td><td> yes       </td><td> The token received by the recipient on zkLink Layer 3                                           </td></tr>
<tr></tr><td> amount        </td><td> BigUint                                </td><td> yes       </td><td> The amount of deposit                                                                           </td></tr>
<tr></tr><td> serialId      </td><td> u64                                    </td><td> yes       </td><td> The serial number of the event, used as nonce                                                   </td></tr>
<tr></tr><td> ethHash       </td><td> <a href="../basic-types.md#txhash">TxHash</a>                                 </td><td> yes       </td><td> The transaction that generated this event on Layer 1                                            </td></tr>
</tbody>
</table>

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
