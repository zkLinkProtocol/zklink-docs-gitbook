
## type ContractBuilder
```go
type ContractBuilder struct {
    AccountId    AccountId
    SubAccountId SubAccountId
    SlotId       SlotId
    Nonce        Nonce
    PairId       PairId
    Size         BigUint
    Price        BigUint
    Direction    bool
    TakerFeeRate uint8
    MakerFeeRate uint8
    HasSubsidy   bool
}
```
Builder used to build [Contract](#type-contract)

## type Contract
The [Contract](../../../api-and-sdk/data-types/transaction/contract_matching.md) struct of taker and maker in perpetual contract, it's a opaque data type.

### func NewContract
```go
func NewContract(builder ContractBuilder) *Contract
```
Create a new Contract.

### func (*Contract) IsLong
```go
func (*Contract) IsLong() bool
```
Return true if the contract opens a long position.

### func (*Contract) IsShort
```go
func (*Contract) IsShort() bool
```

Return true if the contract opens a short position.

### func (*Contract) GetSignature
```go
func (_self *Contract) GetSignature() ZkLinkSignature
```
Get the L3 signature of the contract.

### func (*Contract) IsSignatureValid

```go
func (*Contract) IsSignatureValid() bool
```

Check if the signature is valid or not.

### func (*Contract) GetBytes

```golang
func (*Contract) GetBytes() []uint8 
```

Get the encoded bytes that used to create the L3 signature.

### func (*Contract) CreateSignedContract
```go
func (*Contract) CreateSignedContract(zklinkSigner *ZkLinkSigner) (*Contract, error)
```
Create a new contract with L3 signature.

**input:**
* zklinkSigner: [zklinkSigner](../signer.md#type-zklinksigner)

## type ContractMatchingBuilder
```go
type ContractMatchingBuilder struct {
    AccountId    AccountId
    SubAccountId SubAccountId
    Taker        *Contract
    Maker        []*Contract
    Fee          BigUint
    FeeToken     TokenId
}
```

## type ContractMatching
The [ContractMatching](../../../api-and-sdk/data-types/transaction/contract_matching.md) transaction in perpetual contract, it's a opaque data type.

### NewContractMatching(builder ContractMatchingBuilder)

```go
func NewContractMatching(builder ContractMatchingBuilder) *ContractMatching
```

Create a new ContractMatching transaction.

### func (*ContractMatching) GetBytes

```go
func (_self *ContractMatching) GetBytes() []uint8
```
Get the encoded bytes that used to create the L3 signature. See more in [Private Key and Signature](../../../api-and-sdk/private-key-and-signature/encode/contract_matching.md).

### func (*ContractMatching) TxHash

```go
func (*ContractMatching) TxHash() []uint8
```
Get the transaction hash of the transaction.

### func (*ContractMatching) JsonStr

```go
func (*ContractMatching) JsonStr() string
```

Get the json string of the the transaction.

### func (*ContractMatching) IsValid

```go
func (*ContractMatching) IsValid() bool
```
Check if the transaction is valid or not.

### func (*ContractMatching) GetSignature

```go
func (*ContractMatching) GetSignature() ZkLinkSignature
```
Get the L3 signature of the transaction.

### func (*ContractMatching) IsSignatureValid

```go
func (*ContractMatching) IsSignatureValid() bool
```
Check if the inside L3 signature is valid or not.

### func (*ContractMatching) CreateSignedTx

```go
func (*ContractMatching) CreateSignedTx(signer *ZkLinkSigner) (*ContractMatching, error)
```
Create a new transaction with L3 signature.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)

### func (*ContractMatching) ToZklinkTx

```go
func (*ContractMatching) ToZklinkTx() ZkLinkTx
```

Change the transaction to the [ZkLinkTx](../basic_types.md#zklinktx)
