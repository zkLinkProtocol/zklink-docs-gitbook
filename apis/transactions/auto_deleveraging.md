Automatic deleveraging

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> type            </td><td>String          </td><td>yes       </td><td>The value is "AutoDeleveraging"                                                              </td><tr>
<tr><td> accountId       </td><td>AccountId       </td><td>yes       </td><td>Account id                                                                                 </td><tr>
<tr><td> subAccountId    </td><td>SubAccountId    </td><td>yes       </td><td>Subaccount id                                                                              </td><tr>
<tr><td> subAccountNonce </td><td>Nonce           </td><td>yes       </td><td>The nonce of subaccount                                                                    </td><tr>
<tr><td> oraclePrices    </td><td>OraclePrices    </td><td>yes       </td><td>it contains a list of [ContractPrice]() and list of [SpotPriceInfo]()                      </td><tr>
<tr><td> adlAccountId    </td><td>AccountId       </td><td>yes       </td><td>The ADL account id                                                                         </td><tr>
<tr><td> pairId          </td><td>PairId          </td><td>yes       </td><td>The pair id, for example the id of BTC-USDT pair                                           </td><tr>
<tr><td> adlSize         </td><td>BigUint         </td><td>yes       </td><td>The ADL size                                                                               </td><tr>
<tr><td> adlPrice        </td><td>BigUint         </td><td>yes       </td><td>The ADL price                                                                              </td><tr>
<tr><td> fee             </td><td>BigUint         </td><td>yes       </td><td>The fee                                                                                    </td><tr>
<tr><td> feeToken        </td><td>TokenId         </td><td>yes       </td><td>The token id of the fee                                                                    </td><tr>
<tr><td> signature       </td><td>ZkLinkSignature </td><td>yes       </td><td>The pub key hash corresponding to the signature must be aligned with the initiator account </td><tr>
</tbody>
</table>


where the `ContractPrice` and `SpotPriceInfo` defined as below:

{% tabs %}
{% tab title="ContractPrice" %}

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td>pairId</td><td>PairId</td><td>yes</td><td>The pair id of trade pair, for example the id of BTC-USDT pair</td></tr>
<tr><td>marketPrice </td><td>BugUint</td><td>yes</td><td> The market price of the associated pair</td></tr>
</tbody>
</table>

{% endtab %}

{% tab title="SpotPriceInfo" %}

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> tokenId     </td><td> TokenId </td><td> yes       </td><td> The pair id of trade pair, for example the id of BTC-USDT pair </td></tr>
<tr><td> price </td><td> BigUint </td><td> yes       </td><td> The spot price                                              </td></tr>
</tbody>
</table>


{% endtab %}

{% end tabs %}

For example:

```json

{
  "type": "AutoDeleveraging",
  "accountId": 0,
  "subAccountId": 0,
  "subAccountNonce": 0,
  "oraclePrices": {
    "contractPrices": [
      {
        "pairId": 1,
        "marketPrice": "100"
      }
    ],
    "marginPrices": [
      {
        "tokenId": 1,
        "price": "100"
      }
    ]
  },
  "adlAccountId": 0,
  "pairId": 0,
  "adlSize": "0",
  "adlPrice": "0",
  "fee": "100",
  "feeToken": 1,
  "signature": {
    "pubKey": "0x43cbec0bf142a942df9db99d27bd4ceeb8f4e75f9444b4cee4e3170965854404",
    "signature": "366e759d61a5052073e13147ed3e8e1642dfea10cd423bbb9a795932a15a4c122fa5e71c35a7d59198fa2d7ed28bb1f44e5c5392049607347855243ddc027d00"
  }
}
```

## Sign autoDeleveraging

{% tabs %}
{% tab title="Golang" %}
```go

```

For more detail please refer to [Golang example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Golang) in SDK
{% endtab %}

{%tab title="javascript" %}

```javascript

```

For more detail please refer to [javascript example](https://github.com/zkLinkProtocol/zklink_sdk/tree/main/examples/Javascript)
{% endtab %}
{% endtabs %}
