## type ForcedExitBuilder

```go
type ForcedExitBuilder struct {
    ToChainId             ChainId
    InitiatorAccountId    AccountId
    InitiatorSubAccountId SubAccountId
    Target                ZkLinkAddress
    TargetSubAccountId    SubAccountId
    L2SourceToken         TokenId
    L1TargetToken         TokenId
    InitiatorNonce        Nonce
    ExitAmount            BigUint
    WithdrawToL1          bool
    Timestamp             TimeStamp
}
```

Builder used to create ForcedExit transaction.

## type ForcedExit
[ForcedExit](../../../api-and-sdk/data-types/transaction/forced_exit.md) transaction type, it's a opaque data type.


### func NewForcedExit

```go
func NewForcedExit(builder ForcedExitBuilder) *ForcedExit
```

Create a new [ForcedExit](#type-forcedexit) transaction.

### func (*ForcedExit) GetSignature

```go
func (*ForcedExit) GetSignature() ZkLinkSignature
```
Get the signature in the [Transfer](#type-transfer) transaction.

### func (*ForcedExit) GetBytes

```go
func (_self *ForcedExit) GetBytes() []uint8
```
Get the encoded bytes used to create the L3 signature.

### func (*ForcedExit) TxHash

```go
func (*ForcedExit) TxHash() []uint8
```
Get the transaction hash of [ForcedExit](#type-forcedexit) transaction.

### func (*ForcedExit) JsonStr

```go
func (*ForcedExit) JsonStr() string
```
Get the json string of the [ForcedExit](#type-forcedexit) transaction.

### func (*ForcedExit) IsValid

```go
func (_self *ForcedExit) IsValid() bool
```
Check if all the fields in Withdraw are valid. For example, if the `ChainId` is exceeded the maximum ChainId, it will return false.

### func (*ForcedExit) IsSignatureValid

```go
func (*ForcedExit) IsSignatureValid() bool
```
Check if the L3 signature in the Transfer transaction is valid or not.


### func (*ForcedExit) SubmitterSignature

```go
func (*ForcedExit) SubmitterSignature(signer *ZkLinkSigner) (ZkLinkSignature, error)
```

Create the submitter signature.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)

### func (*ForcedExit) CreateSignedTx

```go
func (*ForcedExit) CreateSignedTx(signer *ZkLinkSigner) (*ForcedExit, error)
```
Sign the Transfer transaction with the [ZkLinkSigner](../signer.md#type-zklinksigner), L1 signature and L3 signature will be created.

### func (*ForcedExit) ToZklinkTx

```go
func (*ForcedExit) ToZklinkTx() ZkLinkTx
```
Change the transaction to the [ZkLinkTx](../basic_types.md#zklinktx)
