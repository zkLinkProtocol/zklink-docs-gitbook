
## type WithdrawBuilder

```go
type WithdrawBuilder struct {
	AccountId        AccountId
	SubAccountId     SubAccountId
	ToChainId        ChainId
	ToAddress        ZkLinkAddress
	L2SourceToken    TokenId
	L1TargetToken    TokenId
	Amount           BigUint
	Fee              BigUint
	Nonce            Nonce
	WithdrawFeeRatio uint16
	WithdrawToL1     bool
	Timestamp        TimeStamp
}
```

The builder is used to build the [Withdraw](./#type-withdraw) transaction.

## type Withdraw
[Withdraw](../../../api-and-sdk/data-types/transaction/withdraw.md) transaction type, it's a opaque data type.

### func NewWithdraw(builder WithdrawBuilder)
```go
func NewWithdraw(builder WithdrawBuilder) *Withdraw
```
Create a new [Withdraw](#type-withdraw) transaction.

**input:**
builder: [WithdrawBuilder](#type-withdrawbuilder)

### func (*Withdraw) GetBytes

```go
func (_self *Withdraw) GetBytes() []uint8
```
Get the encoded bytes used to create the L3 signature.

### func (*Withdraw) TxHash

```go
func (*Withdraw) TxHash() []uint8
```

Get the transaction hash of [Withdraw](#type-withdraw) transaction

### func (*Withdraw) IsValid

```go
func (*Withdraw) IsValid() bool
```

Check if all the fields in [Withdraw](#type-withdraw) are valid. For example, if the `ChainId` is exceeded the maximum ChainId, it will return false.

### func (*Withdraw) CreateSignedTx

```go
func (*Withdraw) CreateSignedTx(signer *ZkLinkSigner) (*Withdraw, error)
```
The ZkLinkSigner will sign the [Withdraw] transaction, replace the default signature in the transaction.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)


### func (*Withdraw) GetSignature

```go
func (_self *Withdraw) GetSignature() ZkLinkSignature
```
Get L3 signature inside the transaction.


### func (*Withdraw) IsSignatureValid

```go
func (*Withdraw) IsSignatureValid() bool
```

Check if the L3 signature in the Withdraw transaction is valid or not.

### func (*Withdraw) EthSignature

```go
func (*Withdraw) EthSignature(ethSigner *EthSigner, l2SourceTokenSymbol string) (PackedEthSignature, error)
```

Create the Ethereum signature.
**input:**:
* ethSigner: the [Ethereum signer](../signer.md#type-ethsigner)
* l2SourceTokenSymbol: the symbol string of l2 token, for example: "USD"

### func (*Withdraw) SubmitterSignature

```go
func (_self *Withdraw) SubmitterSignature(signer *ZkLinkSigner) (ZkLinkSignature, error)
```
Create a submitter signature.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)

### func (*Withdraw) ToZklinkTx

```go
func (*Withdraw) ToZklinkTx() ZkLinkTx
```

Change the Withdraw transaction to the [ZkLinkTx](../basic_types.md#zklinktx)
