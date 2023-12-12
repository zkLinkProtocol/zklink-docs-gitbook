## ContractPrice

```go
type ContractPrice struct {
    PairId      PairId
    MarketPrice BigUint
}
```

* **PairId:** The contract pair id defined by zkLink.
* **MarketPrice:** The market price of the contract pair

## SpotPrice

```go
type SpotPriceInfo struct {
	TokenId TokenId
	Price   BigUint
}
```
* **TokenId:** The token id defined by zkLink.
* **price:** The spot price of the token.

## type Order
The spot order type of taker and maker. it's a opaque data type.

### func NewOrder

```go
func NewOrder(
    accountId AccountId,
    subAccountId SubAccountId,
    slotId SlotId,
    nonce Nonce,
    baseTokenId TokenId,
    quoteTokenId TokenId,
    amount BigUint,
    price BigUint,
    isSell bool,
    hasSubsidy bool,
    makerFeeRate uint8,
    takerFeeRate uint8,
    signature *ZkLinkSignature
) *Order
```
Create a new Order.

**input:**
* accountId: the user account id 
* subAccountId: the user sub account id
* slotId: the slot id of order
* nonce: the nonce of user account
* baseTokenId: the token id of the base token of trade pair, for example,  "BTC" token id of "BTCUSDT" pair
* quoteTokenId: the token id of the quote token of trade pari, for example, "USDT" token id of "BTCUSDT" pair
* amount: the amount of base token
* price: the price of base token
* isSell: sell token or not
* hasSubsidy: subsidy only works for maker and makerFeeRate
* makeFeeRate: the fee maker rate, 100 means 1%, max is 2.56%
* takerFeeRate: the fee taker rate, 100 means 1%, max is 2.56%
* signature: optional, the L3 signature of the Order

### func (*Order) GetSignature

```go
func (*Order) GetSignature() ZkLinkSignature
```
Get the L3 signature of the Order

### func (*Order) GetSignature

```go
func (*Order) GetSignature() ZkLinkSignature {
```
Get the Order signature.

### func (*Order) GetBytes

```go
func (*Order) GetBytes() []uint8
```
Get the encoded bytes to create the L3 signature.

### func  *Order) JsonStr

```go
func (*Order) JsonStr() string
```
Get the json string of the Order.

### func (*Order) IsValid

```go
func (*Order) IsValid() bool
```
Check if the Order is valid or not.

### func (*Order) IsSignatureValid

```go
func (*Order) IsSignatureValid() bool
```

Check if the L3 signature is valid or not.

### func (*Order) CreateSignedOrder

```go
func (*Order) CreateSignedOrder(zklinkSigner *ZkLinkSigner) (*Order, error)
```
Returns a new order with L3 signature.

**input:**
* zklinkSigner: [zkLinkSigner](../signer.md#type-zklinksigner)


## OrderMatchingBuilder

```go
type OrderMatchingBuilder struct {
    AccountId         AccountId
    SubAccountId      SubAccountId
    Taker             *Order
    Maker             *Order
    Fee               BigUint
    FeeToken          TokenId
    ContractPrices    []ContractPrice
    MarginPrices      []SpotPriceInfo
    ExpectBaseAmount  BigUint
    ExpectQuoteAmount BigUint
}
```
The builder is used to bo build [OrderMatching]() transaction.

## type OrderMatching
[OrderMatching]() transaction type, it's a opaque data type.

### func NewOrderMatching
```go
func NewOrderMatching(builder OrderMatchingBuilder) *OrderMatching
```
Create a new [OrderMatching](#type-ordermatching) transaction.

### func (*OrderMatching) GetBytes
```go
func (*OrderMatching) GetBytes() []uint8
```
Get the encoded bytes used to create the L3 signature.

### func (*OrderMatching) TxHash() []uint8

```go
func (*OrderMatching) TxHash() []uint8
```
Get the transaction hash of [OrderMatching](#type-ordermatching) transaction.

### func (*OrderMatching) JsonStr

```go
func (*OrderMatching) JsonStr() string
```
Get the json string of the [OrderMatching](#type-ordermatching) transaction.

### func (*OrderMatching) IsValid() bool

```go
func (*OrderMatching) IsValid() bool
```
Check if all the fields in OrderMatching are valid. For example, if the `ChainId` is exceeded the maximum ChainId, it will return false.

### func (*OrderMatching) GetSignature

```go
func (*OrderMatching) GetSignature() ZkLinkSignature
```
Get the L3 signature of OrderMatching transaction.

### func (*OrderMatching) IsSignatureValid

```go
func (*OrderMatching) IsSignatureValid() bool
```
Check if the L3 signature in the transaction is valid or not.

### func (*OrderMatching) CreateSignedTx

```go
func (*OrderMatching) CreateSignedTx(signer *ZkLinkSigner) (*OrderMatching, error)
```
Sign the Transfer transaction with the [ZkLinkSigner](../signer.md#type-zklinksigner), L1 signature and L3 signature will be created.

**input:**
* signer: [ZkLinkSigner](../signer.md#type-zklinksigner)

### func (*OrderMatching) ToZklinkTx

```go
func (*OrderMatching) ToZklinkTx() ZkLinkTx
```
Change the transaction to the [ZkLinkTx](../basic_types.md#zklinktx)
