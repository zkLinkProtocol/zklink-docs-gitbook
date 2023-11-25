This transaction is used to update the global variable settings.

| Name         | Type                    | Required | Description                                |
|--------------|-------------------------|-----------|--------------------------------------------|
| type         | String                  | yes       | The value is "UpdateGlovalVar "            |
| fromChainId  | ChainId                 | yes       | The layer2 chain id                        |
| subAccountId | SubAccountId            | yes       | The subaccount id                          |
| parameter    | [Parameter](#parameter) | yes       | Different operation has different variable |
| seriaId      | u64                     | yes       | The serial id                              |

There are 5 parameters, different operations correspond to different parameters:

{% tabs %}
{% tab title="feeAccount" %}

Modify the collect-fee account.

| Name       | Type                    | Required | Description        |
|------------|-------------------------|-----------|--------------------|
| feeAccount | [AccountId](#AccountId)| yes       | The fee account id |
 
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
| accountId | [AccountId](#AccountId)  | yes       | The account id of  insuranceFundAccount|

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
| marginId | [MarginId](#MarginId)      | yes | The margin id             |
| tokenId  | [TokenId](#TokenId)        | yes | The Token id              |
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

Modify the info of every prepatual contract pair.

| Name                  | Type     | Required         | Description                                             |
|-----------------------|-------------------|------------|---------------------------------------------------------|
| pairId                | [PairId](#PairId) | yes | The pair id                                             |
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
| > pairId      | [PairId](#PairId) | yes       | The pair id                                                           |
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

