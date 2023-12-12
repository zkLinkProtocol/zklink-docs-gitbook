## type LiquidationBuilder

```go
type LiquidationBuilder struct
    AccountId            AccountId
    SubAccountId         SubAccountId
    SubAccountNonce      Nonce
    ContractPrices       []ContractPrice
    MarginPrices         []SpotPriceInfo
    LiquidationAccountId AccountId
    Fee                  BigUint
    FeeToken             TokenId
}
```
The builder used to create the [Liquidation](#type-liquidation) transaction.

## type Liquidation
[Liquidation](../../../api-and-sdk/data-types/transaction/liquidation.md) transaction type, it's a opaque data type.

### func NewLiquidation(builder LiquidationBuilder)

```go
func NewLiquidation(builder LiquidationBuilder) *Liquidation
```
Create a new [Liquidation](#type-liquidation) transaction.


### func (*Liquidation) GetBytes
```go
func (*Liquidation) GetBytes() []uint8
```
Get the encoded bytes to create the L3 signature. See more in [Private Key and Signature](../../../api-and-sdk/private-key-and-signature/encode/liquidation.md)

### func (*Liquidation) TxHash
```go
func (*Liquidation) TxHash() []uint8
```
Get the transaction hash of the transaction.

### func (*Liquidation) JsonStr
```go
func (*Liquidation) JsonStr() string
```
Get the json string of the transaction.

### func (*Liquidation) IsValid
```go
func (*Liquidation) IsValid() bool
```
Check if the transaction is valid.

### func (*Liquidation) GetSignature
```go
func (*Liquidation) GetSignature() ZkLinkSignature
```
Get L3 signature of the transaction.

### func (*Liquidation) IsSignatureValid
```go
func (*Liquidation) IsSignatureValid() bool
```
Check if the inside L3 signature is valid or not.

### func (*Liquidation) ToZklinkTx
```go
func (*Liquidation) ToZklinkTx() ZkLinkTx
```
Change the transaction to the [ZkLinkTx](../basic_types.md#zklinktx)

### func (*Liquidation) CreateSignedTx
```go
func (*Liquidation) CreateSignedTx(signer *ZkLinkSigner) (*Liquidation, error)
```
Create a new Liquidation transaction with L3 signature inside.
**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)
