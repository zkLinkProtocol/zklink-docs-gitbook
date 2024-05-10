## type ContractPrice

### constructor

```javascript
/**
* @param {number} pair_id
* @param {string} market_price
*/
ContractPrice(pair_id, market_price)
```

**input:**
* pair_id: The contract pair id defined by zkLink.
* market_price: The market price of the contract pair

## type SpotPriceInfo

### constructor

```javascript
/**
* @param {number} token_id
* @param {string} price
*/
SpotPriceInfo(token_id, price)
```

**input:**
* token_id: The token id defined by zkLink.
* price: The spot price of the token.

## type FundingInfo

### constructor

```javascript
/**
* @param {number} pair_id
* @param {number} funding_rate
* @param {string} price
*/
FundingInfo(pair_id, funding_rate, price)
```

### type MarginInfo

### constructor

```javascript
/**
* @param {number} margin_id
* @param {number} token_id
* @param {number} ratio
*/
MarginInfo(margin_id, token_id, ratio)
```

### type ContractInfo

### constructor

```javascript
/**
* @param {number} pair_id
* @param {string} symbol
* @param {number} initial_margin_rate
* @param {number} maintenance_margin_rate
*/
ContractInfo(pair_id, symbol, initial_margin_rate, maintenance_margin_rate)
```

## type Parameter
The [Parameter](../../../api-and-sdk/data-types/transaction/update\_global\_var.md) struct of [UpdateGlobalVar](#type-updateglobalvar).

### constructor

```javascript
/**
* @param {number} insurance_fund_account
*/
Parameter(ParameterType.InsuranceFundAccount, insurance_fund_account)

/**
* @param {number} fee_account
*/
Parameter(ParameterType.FeeAccount, fee_account)

/**
* @param {FundingInfo[]} funding_infos
*/
Parameter(ParameterType.FundingInfos, funding_infos)

/**
* @param {MarginInfo} margin_info
*/
Parameter(ParameterType.MarginInfo, margin_info)

/**
* @param {ContractInfo} contract_info
*/
Parameter(ParameterType.ContractInfo, contract_info)
```

## type UpdateGlobalVarBuilder

### constructor

```javascript
/**
* @param {number} from_chain_id
* @param {number} sub_account_id
* @param {Parameter} parameter
* @param {number} serial_id
*/
UpdateGlobalVarBuilder(from_chain_id, sub_account_id, parameter, serial_id)
```

## type UpdateGlobalVar
[UpdateGlobalVar](../../../api-and-sdk/data-types/transaction/update\_global\_var.md) transaction type.

### constructor

```javascript
/**
* @param {UpdateGlobalVarBuilder} builder
*/
newUpdateGlobalVar(builder)
```

### Example

```javascript
const margin_info = new MarginInfo(2,17,10).jsValue();
const parameter = new Parameter(ParameterType.MarginInfo,margin_info)
console.log(parameter);

let tx_builder = new UpdateGlobalVarBuilder(1,8,parameter,1000);
console.log(tx_builder);
let tx = newUpdateGlobalVar(tx_builder);
console.log(tx.jsValue());
```