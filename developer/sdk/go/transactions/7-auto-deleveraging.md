## type AutoDeleveragingBuilder

```go
type AutoDeleveragingBuilder struct {
	AccountId       AccountId
	SubAccountId    SubAccountId
	SubAccountNonce Nonce
	ContractPrices  []ContractPrice
	MarginPrices    []SpotPriceInfo
	AdlAccountId    AccountId
	PairId          PairId
	AdlSize         BigUint
	AdlPrice        BigUint
	Fee             BigUint
	FeeToken        TokenId
}
```

Builder that used to build AutoDeleveraging transaction.

## type AutoDeleveraging
[AutoDeleveraging](../../../api-and-sdk/data-types/transaction/auto_deleveraging.md) transaction type, it's a opaque data type.

### func NewAutoDeleveraging
```go
func NewAutoDeleveraging(builder AutoDeleveragingBuilder) *AutoDeleveraging
```
Create a new AutoDeleveraging transaction.

### func (*AutoDeleveraging) GetBytes

```go
func (*AutoDeleveraging) GetBytes() []uint8
```
Get the encoded bytes to create the L3 signature. See more in [Private Key and Signature](../../../api-and-sdk/private-key-and-signature/encode/auto_deleveraging.md)

### func (*AutoDeleveraging) TxHash
```go
func (*AutoDeleveraging) TxHash() []uint8
```
Get the transaction hash of the transaction.

### func (*AutoDeleveraging) JsonStr
```go
func (*AutoDeleveraging) JsonStr() string
```
Get the json string of the transaction.

### func (*AutoDeleveraging) IsValid
```go
func (*AutoDeleveraging) IsValid() bool
```
Check if the transaction is valid.

### func (*AutoDeleveraging) CreateSignedTx
```go
func (*AutoDeleveraging) CreateSignedTx(signer *ZkLinkSigner) (*AutoDeleveraging, error)
```
Create a new AutoDeleveraging transaction with L3 signature inside.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)

### func (*AutoDeleveraging) GetSignature
```go
func (*AutoDeleveraging) GetSignature() ZkLinkSignature
```
Get L3 signature of the transaction.

### func (*AutoDeleveraging) IsSignatureValid
```go
func (*AutoDeleveraging) IsSignatureValid() bool
```
Check if the inside L3 signature is valid or not.

### func  *AutoDeleveraging) ToZklinkTx
```go
func (*AutoDeleveraging) ToZklinkTx() ZkLinkTx
```
Change the transaction to the [ZkLinkTx](../basic_types.md#zklinktx)

