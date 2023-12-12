## type FundingBuilder

```go
type FundingBuilder struct {
	AccountId         AccountId
	SubAccountId      SubAccountId
	SubAccountNonce   Nonce
	FundingAccountIds []AccountId
	Fee               BigUint
	FeeToken          TokenId
}
```

Builder that used to build the [Funding](#type-funding) transaction.

## type Funding
[Funding](../../../api-and-sdk/data-types/transaction/funding.md) transaction type, it's a opaque data type.

### func NewFunding(builder FundingBuilder)
```go
func NewFunding(builder FundingBuilder) *Funding
```
Create a new [Funding](#type-funding) transaction.

### func (*Funding) GetBytes
```go
func (*Funding) GetBytes() []uint8 {
```
Get the encoded bytes to create the L3 signature. See more in [Private Key and Signature](../../../api-and-sdk/private-key-and-signature/encode/funding.md)

### func (*Funding) TxHash

```go
func (*Funding) TxHash() []uint8 {
```
Get the transaction hash of the transaction.

### func (*Funding) JsonStr

```go
func (*Funding) JsonStr() string {
```
Get the json string of the transaction.

### func (*Funding) IsValid

```go
func (*Funding) IsValid() bool {
```
Check if the transaction is valid or not.

### func (*Funding) GetSignature

```go
func (*Funding) GetSignature() ZkLinkSignature {
```

Get L3 signature of the transaction.

### func (*Funding) IsSignatureValid

```go
func (*Funding) IsSignatureValid() bool {
```
Check if the inside L3 signature is valid or not.

### func (*Funding) ToZklinkTx

```go
func (*Funding) ToZklinkTx() ZkLinkTx {
```
Change the transaction to the [ZkLinkTx](../basic_types.md#zklinktx)

### func (*Funding) CreateSignedTx

```go
func (*Funding) CreateSignedTx(signer *ZkLinkSigner) (*Funding, error) {
```

Create a new AutoDeleveraging transaction with L3 signature inside.
**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)
