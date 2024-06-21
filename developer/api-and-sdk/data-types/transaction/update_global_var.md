This transaction is used to update the global variable settings.

<table>
<thead><tr><th width="20">Name</th><th width="20">Type</th><th width="10">Required</th><th width="250">Description</th></tr></thead>
<tbody>
<tr><td> type         </td><td> String                  </td><td> yes       </td><td> The value is "UpdateGlovalVar "            </td></tr>
<tr><td> fromChainId  </td><td> <a href="../basic-types.md#chainid">ChainId</a>                 </td><td> yes       </td><td> The layer3 chain id                        </td></tr>
<tr><td> subAccountId </td><td> <a href="../basic-types.md#subaccountid">SubAccountId</a>            </td><td> yes       </td><td> The subaccount id                          </td></tr>
<tr><td> parameter    </td><td> <a name="parameter" href="#">Parameter</a> </td><td> yes       </td><td> Different operation has different variable </td></tr>
<tr><td> seriaId      </td><td> u64                     </td><td> yes       </td><td> The serial id                              </td></tr>
</tbody>
</table>

<a id="parameter">There are 5 parameters</a>, different operations correspond to different parameters:

{% tabs %}
{% tab title="feeAccount" %}

Modify the collect-fee account.

| Name       | Type                    | Required | Description        |
|------------|-------------------------|-----------|--------------------|
| feeAccount | <a href="../basic-types.md#accountid">AccountId</a> | yes       | The fee account id |
 
For example:

```json
{
  "type": "UpdateGlobalVar",
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "feeAccount": {
      "accountId": 10
    }
  },
  "serialId": 100
}
```

{% endtab %}

{% tab title="insuranceFundAccount" %}

Modify the insurance fund account

| Name       | Type                     | Required | Description        |
|-----------|--------------------------|-----------|--------------------|
| accountId | <a href="../basic-types.md#accountid">AccountId</a>  | yes       | The account id of  insuranceFundAccount|

For Example:

```json
{
  "type": "UpdateGlobalVar",
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "insuranceFundAccount": {
      "accountId": 9
    }
  },
  "serialId": 100
}
```
{% endtab %}


{% tab title="marginInfo" %}

Modify the margin info in the specified index.

| Name     | Type                       | Required | Description        |
|----------|----------------------------|-----|---------------------------|
| marginId | [MarginId](#marginid)      | yes | The margin id             |
| tokenId  | [TokenId](#tokenid)        | yes | The Token id              |
| ratio    | u8                         | yes |the ratio, 100 means 1.0% |

For example 

```json
{
  "type": "UpdateGlobalVar",
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "marginInfo": {
      "marginId": 1,
      "tokenId": 9,
      "ratio": 0
    }
  },
  "serialId": 100
}
```

{% endtab %}


{% tab title="contractInfo" %}

Modify the info of every perpetual contract pair.

| Name                  | Type     | Required         | Description                                             |
|-----------------------|-------------------|------------|---------------------------------------------------------|
| pairId                | [PairId](#pairid) | yes | The pair id                                             |
| symbol                | String            | yes | The symbol of the contract                              |
| initialMarginRate     | u16               | yes | The initial margin rate of the contract, 100 means 0.1% |
| maintenanceMarginRate | u16               | yes | The maintenance margin rate, 100 means 0.1%             |

```json
{
  "type": "UpdateGlobalVar",
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "contractInfo": {
      "pairId": 2,
      "symbol": "BTCUSDC",
      "initialMarginRate": 6,
      "maintenanceMarginRate": 8
    }
  },
  "serialId": 100
}
```

{% endtab %}


{% tab title="fundingInfos" %}
Update the funding rates to accumulated funding rates of the Global Vars for all position(contract pair) in this period

| Name          | Type  | Required | Description       |
|---------------|-------|-----------|-------------------|
| infos         | array | yes       | funding info list |
| > pairId      | [PairId](#pairid) | yes       | The pair id                                                           |
| > price       | BigUint          | yes       | the mark price of the trade pair                                      |
| > fundingRate | i16         | yes       | the fee funding rate, the actual result needs to be divided by `10^6` |

For example:

```json
{
  "type": "UpdateGlobalVar",
  "fromChainId": 1,
  "subAccountId": 1,
  "parameter": {
    "fundingInfos": {
      "infos": [
        {
          "pairId": 0,
          "price": "1000000000000000000",
          "fundingRate": 32767
        },
        {
          "pairId": 1,
          "price": "1000000000000000",
          "fundingRate": 0
        }
      ]
    }
  },
  "serialId": 100
}
```

{% endtab %}

{% endtabs %}

