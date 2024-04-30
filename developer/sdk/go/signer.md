## Type EthSigner
Ethereum private key, opaque type.

### Func NewEthSigner
```go
func NewEthSigner(privateKey string) (*EthSigner, error)
```
Create a Ethereum private key signer from hex string.

**input:**
* privateKey: hex string of private key

**return:** (*EthSigner, error)

### Func (*EthSigner) SignMessage
```
func (*EthSigner) SignMessage(message []uint8) (PackedEthSignature, error)
```

Sign the message bytes

**input:**
* message: `[]uint8`

**output:** (String format of `PackedEthSignature`, error)


### Func (*EthSigner) GetAddress
```go
func (*EthSigner) GetAddress() Address
```
Get the address of Ethereum

**output:** Hex string format(with '0x' prefix) of Ethereum address.


### example

```
func TestEthSigner(t *testing.T) {
    s := "0xb32593e347bf09436b058fbeabc17ebd2c7c1fa42e542f5f78fc3580faef83b7"
    signer, err := sdk.NewEthSigner(s)
    assert.Nil(t, err)
    msg := []byte("hello world")
    signature, err := signer.SignMessage(msg)
    assert.Nil(t, err)

    assert.Equal(t, signature, "0xa9aa0710adb18f84d4bed8057382fc433c3dcff1bddf3b2b1c2cb11386ef3be4172b5d0688143759d4e744acc434ae4f96575c7fa9096971fd02fb3d2aaa77121c")
    address := signer.GetAddress()
    assert.Equal(t, address, "0x9e372368c25056d44045e445d72d7b91ce3ee3b1")
}
```

## Type StarkSigner
The signer of Starknet, opaque type.

### NewStarkSigner
```go
func NewStarkSigner() *StarkSigner
```

Create a random Starknet signer

**output:** `*StarkSigner`

### func StarkSignerNewFromHexStr
```go
func StarkSignerNewFromHexStr(hexStr string) (*StarkSigner, error)
```

Create a Starknet signer from hex string

**input:** 
* hexStr: hex string of private key(with or without `0x` prefix)

### func(*StarkSigner) SignMessage
```go
func (*StarkSigner) SignMessage(message []uint8) (StarkECDSASignature, error)
```
Sign message bytes

**input:**
* message: `[]uint8`
  

### example

```go
func TestStarkSigner(t *testing.T) {
    s := "0x02c5dbad71c92a45cc4b40573ae661f8147869a91d57b8d9b8f48c8af7f83159"
    signer, err := sdk.StarkSignerNewFromHexStr(s)
    assert.Nil(t, err)
    msg := []byte("hello world")
    signature, err := signer.SignMessage(msg)
    assert.Nil(t, err)
    assert.Equal(t, signature, "0x0226424352505249f119fd6913430aad28321afcff18036139b6fa77c4fad6cc071f3cf1b6bc80b84f261c74613ed3d90e5d11b1cc391b2bbddc70b9b4cc31f9067799101a3d9e6421c4d3950983b004dcfb9061a77a0e3bc2f9032ac8675b3e")
}
```

## Type ZkLinkSigner
`ZkLinkSigner` includes the L1 private key(Eth or Starknet) and L3 private key, opaque type.

### func NewZkLinkSigner
```go
func NewZkLinkSigner() (*ZkLinkSigner, error)
```

Create a random seeded ZkLinkSigner


```go
signer, err := sdk.NewZkLinkSigner()
```

### func ZkLinkSignerNewFromHexEthSigner
```go
func ZkLinkSignerNewFromHexEthSigner(ethHexPrivateKey string) (*ZkLinkSigner, error)
```
Creat a `ZkLinkSigner` from eth hex private key.

**input:**
* hexPrivateKey: Eth hex private key string

```go
s := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
signer, err = sdk.ZkLinkSignerNewFromHexEthSigner(s)
```

### func ZkLinkSignerNewFromHexStarkSigner

```go
func ZkLinkSignerNewFromHexStarkSigner(hexPrivateKey string) (*ZkLinkSigner, error)
```

Create a `ZkLinkSigner` from Starknet hex private key.

**input:**
* hexPrivateKey: Starknet hex private key string


### func ZkLinkSignerNewFromBytes

```go
func ZkLinkSignerNewFromBytes(slice []uint8) (*ZkLinkSigner, error)
```
Change the bytes to ZkLinkSigner

**input:**
* slice: []uint8


```go
s := "0x02c5dbad71c92a45cc4b40573ae661f8147869a91d57b8d9b8f48c8af7f83159"
signer, err = sdk.ZkLinkSignerNewFromHexStarkSigner(s)
```

### 


### func (*ZkLinkSigner) PublicKey

```go
func (*ZkLinkSigner) PublicKey() PackedPublicKey
```
Get the public key of L3 private key

### func (*ZkLinkSigner) SignMusig

```go
func (*ZkLinkSigner) SignMusig(msg []uint8) (ZkLinkSignature, error)
```
Sign a bytes formatted message with L3 private key in ZkLinkSigner.

**input:**:

* msg: message bytes

### Example

```go
func TestZkLinkSigner(t *testing.T) {
    signer, err := sdk.NewZkLinkSigner()
	assert.Nil(t, err)
	assert.NotNil(t, signer)
	s := "be725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
	signer, err = sdk.ZkLinkSignerNewFromHexEthSigner(s)
	pub_key := signer.PublicKey()
	assert.Equal(t, pub_key, "0x7b173e25e484eed3461091430f81b2a5bd7ae792f69701dcb073cb903f812510")
	pubkey_hash := sdk.GetPublicKeyHash(pub_key)
	assert.Equal(t, pubkey_hash, "0xd8d5fb6a6caef06aa3dc2abdcdc240987e5330fe")
	msg := []uint8{0,1,2,3,4,5,6}
	signature, err := signer.SignMusig(msg)
	assert.Nil(t, err)
	assert.NotNil(t, signature)
	is_ok := sdk.VerifyMusig(signature, msg)
	assert.Equal(t, is_ok, true)
}
```

### func SignChangePubkeyWithEthEcdsaAuth

```go
func (*Signer) SignChangePubkeyWithEthEcdsaAuth(tx *ChangePubKey) (TxSignature, error)
```

**input:**
tx: [ChangePubKey](./transactions/1-change-pubkey.md#changepubkey)

Sign the `ChangePubkey` and get the `TxSignature` result, for example:

```go
privateKey := "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4"
chainId := sdk.ChainId(1)
accountId := sdk.AccountId(2)
subAccountId := sdk.SubAccountId(4)
newPkHash:= sdk.PubKeyHash("0xd8d5fb6a6caef06aa3dc2abdcdc240987e5330fe")
feeToken := sdk.TokenId(1)
fee := big.NewInt(100)
nonce := sdk.Nonce(100)
ethSignature := sdk.PackedEthSignature("0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001b")
// get current timestamp
now := time.Now()
timeStamp := sdk.TimeStamp(now.Unix())

// create ChangePubKey transaction type without signed
builder := sdk.ChangePubKeyBuilder{
    chainId,
    accountId,
    subAccountId,
    newPkHash,
    feeToken,
    *fee,
    nonce,
    &ethSignature,
    timeStamp,
}
tx := sdk.NewChangePubKey(builder)
signer, err := sdk.NewSigner(privateKey, sdk.L1TypeEth)
if err != nil {
    return
}
txSignature, err := signer.SignChangePubkeyWithEthEcdsaAuth(tx)
    fmt.Println("tx signature: %s", txSignature)
```

## func (*Signer) SignChangePubkeyWithOnchainAuthData
```go
func (*Signer) SignChangePubkeyWithOnchainAuthData(tx *ChangePubKey) (TxSignature, error)
```
Sign `ChangePubkey` with `OnChain` auth data

## func (*Signer) SignChangePubkeyWithCreate2dataAuth

```go
func (*Signer) SignChangePubkeyWithCreate2dataAuth(tx *ChangePubKey, crate2data Create2Data) (TxSignature, error) {
```
Sign `ChangePubkey` with `Create2Data` auth data

## Function


### func GetPublicKeyHash

```go
func GetPublicKeyHash(publicKey PackedPublicKey) PubKeyHash;
```

Get the public key hash of L3 PublicKey

**arguments:**
* publicKey: string of L3 PublicKey

**return:**
String of PubKeyHash

```go
assert.Equal(t, pub_key, "0x7b173e25e484eed3461091430f81b2a5bd7ae792f69701dcb073cb903f812510")
pubkey_hash := sdk.GetPublicKeyHash(pub_key)
```

### func EthSignatureOfChangePubkey
```go
func EthSignatureOfChangePubkey (tx *ChangePubKey, ethSigner *EthSigner) (PackedEthSignature, error)
```
Create the Ethereum signature of [ChangePubKey](../../api-and-sdk/data-types/transaction/change\_pubkey.md)


### func ZklinkMainNetUrl

```go
func ZklinkMainNetUrl() string
```
Get the ZkLink main net url.

### func ZklinkTestNetUrl

```go
func ZklinkTestNetUrl() string
```
