## Type Create2Data

```go
type Create2Data struct {
	CreatorAddress ZkLinkAddress
	SaltArg        H256
	CodeHash       H256
}
```

## Type ChangePubKeyAuthDataEthCreate2

```go
type ChangePubKeyAuthDataEthCreate2 struct {
	Data Create2Data
}
```

## Type ChangePubKeyAuthDataEthEcdsa

```go
type ChangePubKeyAuthDataEthEcdsa struct {
	EthSignature PackedEthSignature
}
```

## Type ChangePubKeyAuthDataOnchain

```go
type ChangePubKeyAuthDataOnchain struct {}
```

## func CreateSignedChangePubkey

```go
func CreateSignedChangePubkey(zklinkSigner *ZkLinkSigner, tx *ChangePubKey, ethAuthData ChangePubKeyAuthData) (*ChangePubKey, error)
```
Create a signed [ChangePubkey](../../api-and-sdk/data-types/transaction/change_pubkey.md)

**input:**
* zklinkSigner: zklink [signer](#type-zklinksigner)
* tx: unsigned transaction [ChangePubkey](../../api-and-sdk/data-types/transaction/change_pubkey.md)
* ethAuthData: `ChangePubKeyAuthData` is a interface which can be [ChangePubKeyAuthDataOnchain](#ChangePubKeyAuthDataOnchain), [ChangePubKeyAuthDataEthCreate2](#ChangePubKeyAuthDataEthCreate2) or [ChangePubKeyAuthDataEthEcdsa](#ChangePubKeyAuthDataEthEcdsa)

## type ChangePubKeyBuilder
The ChangePubKeyBuilder is used to build the type [ChangePubKey](#ChangePubKey)
```go
type ChangePubKeyBuilder struct {
	ChainId       ChainId
	AccountId     AccountId
	SubAccountId  SubAccountId
	NewPubkeyHash PubKeyHash
	FeeToken      TokenId
	Fee           BigUint
	Nonce         Nonce
	EthSignature  *PackedEthSignature
	Timestamp     TimeStamp
}
```

## type ChangePubKey
[ChangePubkey](../../../api-and-sdk/data-types/transaction/change_pubkey.md) transaction type, it's a opaque data type.

### func NewChangePubKey
Create a [ChangePubkey](#type-changepubkey)

```go
func NewChangePubKey(builder ChangePubKeyBuilder) *ChangePubKey
```

**input:**
* builder: [ChangePubKeyBuilder](#type-changepubkeybuilder)

### func (*ChangePubKey) GetSignature

```go
func (*ChangePubKey) GetSignature() ZkLinkSignature
```

Get the L3 signature of the [ChangePubKey](#type-changepubkey)

### func (*ChangePubKey) GetBytes

```go
func (*ChangePubKey) GetBytes() []uint8
```

Get the encoded bytes of [ChangePubKey](#type-changepubkey), which is used to create the L3 signature.

### func (*ChangePubKey) TxHash

```go
func (*ChangePubKey) TxHash() []uint8
```

Get the Transaction Hash of [ChangePubKey](#type-changepubkey)


### func (*ChangePubKey) JsonStr

```go
func (*ChangePubKey) JsonStr() string
```

Get the json str of [ChangePubKey](#type-changepubkey)

### func (*ChangePubKey) IsValid

```go
func (*ChangePubKey) IsValid() bool
```

Check if all the fields in ChangePubKey are valid. For example, if the `ChainId` is exceeded the maximum ChainId, it will return false.

### func (*ChangePubKey) IsOnchain

```go
func (*ChangePubKey) IsOnchain() bool
```

Check if the transaction's auth data is OnChain.


### func (*ChangePubKey) IsSignatureValid

```go
func (*ChangePubKey) IsSignatureValid() bool
```

Check if the L3 signature is valid or not.

### func (*ChangePubKey) SubmitterSignature

```go
func (*ChangePubKey) SubmitterSignature(signer *ZkLinkSigner) (ZkLinkSignature, error) {
```
Create the submitter signature.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)

### func (*ChangePubKey) ToZklinkTx

```go
func (*ChangePubKey) ToZklinkTx() ZkLinkTx
```

Change the ChangePubKey transaction type to [ZkLinkTx](../basic_types.md#zklinktx) type.

### 
