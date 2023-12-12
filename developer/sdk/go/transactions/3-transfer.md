## type TransferBuilder

```go
type TransferBuilder struct {
	AccountId        AccountId
	ToAddress        ZkLinkAddress
	FromSubAccountId SubAccountId
	ToSubAccountId   SubAccountId
	Token            TokenId
	Amount           BigUint
	Fee              BigUint
	Nonce            Nonce
	Timestamp        TimeStamp
}
```
Builder is used to build [Transfer](#type-transfer) transaction

## type Transfer
[Transfer](../../../api-and-sdk/data-types/transaction/transfer.md) transaction type, it's a opaque data type.

### func NewTransfer(builder TransferBuilder)

```go
func NewTransfer(builder TransferBuilder) *Transfer
```

Create a new [Transfer](#type-transfer) transaction.

### func (*Transfer) GetSignature

```go
func (*Transfer) GetSignature() ZkLinkSignature
```

Get the signature in the [Transfer](#type-transfer) transaction.

### func (*Transfer) GetBytes

```go
func (*Transfer) GetBytes() []uint8
```

Get the encoded bytes used to create the L3 signature.


### func (*Transfer) TxHash

```go
func (*Transfer) TxHash() []uint8
```

Get the transaction hash of [Transfer](#type-transfer) transaction.

### func (*Transfer) JsonStr

```go
func (*Transfer) JsonStr() string
```

Get the json string of the [Transfer](#type-transfer) transaction.

### func (*Transfer) IsValid

```go
func (*Transfer) IsValid() bool
```

Check if all the fields in Withdraw are valid. For example, if the `ChainId` is exceeded the maximum ChainId, it will return false.


### func (*Transfer) IsSignatureValid

```go
func (*Transfer) IsSignatureValid() bool
```

Check if the L3 signature in the transaction is valid or not.

### func (*Transfer) GetEthSignMsg

```go
func (*Transfer) GetEthSignMsg(tokenSymbol string) string
```

Get the message that used to create the Ethereum signature.

### func (*Transfer) EthSignature

```go
func (*Transfer) EthSignature(ethSigner *EthSigner, tokenSymbol string) (TxLayer1Signature, error)
```
Create Ethereum signature, returns a [TxLayer1Signature](../basic_types.md#txlayer1signature)

**input:**
* ethSigner: [EthSigner](../signer.md#type-ethsigner)
* tokenSymbol: the symbol string of the token, for example, `USDT`

### func (*Transfer) SubmitterSignature

```go
func (*Transfer) SubmitterSignature(signer *ZkLinkSigner) (ZkLinkSignature, error)
```

Create the submitter signature.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)

### func (*Transfer) CreateSignedTx

```go
func (*Transfer) CreateSignedTx(signer *ZkLinkSigner) (*Transfer, error)
```

Sign the Transfer transaction with the [ZkLinkSigner](../signer.md#type-zklinksigner), L1 signature and L3 signature will be created.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)

### func (*Transfer) ToZklinkTx

```go
func (*Withdraw) ToZklinkTx() ZkLinkTx
```

Change the transaction to the [ZkLinkTx](../basic_types.md#zklinktx)
