
## Parameter
Parameter is an interface which contains 5 types of struct

### type ParameterFeeAccount

```go
type ParameterFeeAccount struct {
	AccountId AccountId
}
```
This parameter is used to modify the collect-fee account.

### type ParameterInsuranceFundAccount

```go
type ParameterInsuranceFundAccount struct {
	AccountId AccountId
}
```
This parameter is used to modify the insurance fund account.

### type ParameterMarginInfo

```go
type ParameterMarginInfo struct {
	MarginId MarginId
	TokenId  TokenId
	Ratio    uint8
}
```
This parameter is used to modify the margin info in the specified index.

### type ParameterFundingInfos 

```go
type ParameterFundingInfos struct {
	Infos []FundingInfo
}
```
This parameter is used to ppdate the funding rates to accumulated funding rates of the Global Vars for all position(contract pair) in this period.

#### type FundingInfo

```go
type FundingInfo struct {
	PairId      PairId
	Price       BigUint
	FundingRate int16
}
```

### type ParameterContractInfo

```go
type ParameterContractInfo struct {
	PairId                PairId
	Symbol                string
	InitialMarginRate     uint16
	MaintenanceMarginRate uint16
}
```
This parameter is used to modify the info of every perpetual contract pair.

## type UpdateGlobalVarBuilder

```go
type UpdateGlobalVarBuilder struct {
    FromChainId  ChainId
    SubAccountId SubAccountId
    Parameter    Parameter
    SerialId     uint64
}
```

The builder is used to build new [UpdateGlobalVar](#type-updateglobalvar) transaction.

## type UpdateGlobalVar
[UpdateGlobalVar](../../../api-and-sdk/data-types/transaction/update_global_var.md) transaction type, it's a opaque data type.

### func NewUpdateGlobalVar
```go
func NewUpdateGlobalVar(builder UpdateGlobalVarBuilder) *UpdateGlobalVar
```
Create a new [UpdateGlobalVar](#type-updateglobalvar) transaction.

### func (*UpdateGlobalVar) GetBytes
```go
func (*UpdateGlobalVar) GetBytes() []uint8
```
Get the encoded bytes to create the L3 signature. See more in [Private Key and Signature](../../../api-and-sdk/private-key-and-signature/encode/funding.md)

### func (*UpdateGlobalVar) TxHash
```go
func (*UpdateGlobalVar) TxHash() []uint8
```
Get the transaction hash of the transaction.

### func (*UpdateGlobalVar) JsonStr
```go
func (*UpdateGlobalVar) JsonStr() string
```
Get the json string of the transaction.

### func (*UpdateGlobalVar) IsValid
```go
func (*UpdateGlobalVar) IsValid() bool
```
Check if the transaction is valid or not.

### func (*UpdateGlobalVar) ToZklinkTx

```go
func (*UpdateGlobalVar) ToZklinkTx() ZkLinkTx
```
Change the transaction to the [ZkLinkTx](../basic_types.md#zklinktx)
