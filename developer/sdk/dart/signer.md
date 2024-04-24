## Type ZkLinkSigner
`ZkLinkSigner` includes the L1 private key(Eth or Starknet) and L3 private key.

### func ethSig

```dart
ZkLinkSigner ethSig(String sig)
```
Creat a `ZkLinkSigner` from eth personal sign.

**input:**
* sig: hex string of eth personal sign(with or without `0x` prefix)

### func starknetSig

```dart
ZkLinkSigner starknetSig(String sig)
```
Creat a `ZkLinkSigner` from starknet signature.

**input:**
* sig: hex string of starknet signature(with or without `0x` prefix)

### func getPubkey

```dart
String getPubkey()
```
Return hex string of public key.

### func getPubkeyHash

```dart
String getPubkeyHash()
```
Return hex string of public key hash.

## Type Signer
L1 private key.

### func ethSigner

```dart
Signer ethSigner(String ethPrivateKey)
```
Create a Ethereum private key signer.

**input:** 
* ethPrivateKey: hex string of private key(with or without `0x` prefix)

### func starknetSigner

```dart
Signer starknetSigner(String ethPrivateKey, String starknetChainId, String starknetAddr)
```

Create a Starknet signer.

**input:** 
* ethPrivateKey: hex string of starknet private key
* starknetChainId: chain id of starknet
* starknetAddr: starknet address

### func signChangePubkeyWithOnchain

```dart
String signChangePubkeyWithOnchain(ChangePubKey tx)
```

### func signChangePubkeyWithEthEcdsaAuth

```dart
String signChangePubkeyWithEthEcdsaAuth(ChangePubKey tx)
```

### func signChangePubkeyWithCreate2DataAuth

```dart
String signChangePubkeyWithCreate2DataAuth(
    ChangePubKey tx,
    String creatorAddress,
    String saltArg,
    String codeHash,
```

### Example

```dart
var signer = Signer.ethSigner(ethPrivateKey: "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4");
var tx = ChangePubKey(
    chainId: 1,
    accountId: 2,
    subAccountId: 4,
    newPubkeyHash: "0xd8d5fb6a6caef06aa3dc2abdcdc240987e5330fe",
    feeToken: 1,
    fee: "100",
    nonce: 100,
);
print(signer.signChangePubkeyWithCreate2DataAuth(
    tx: tx,
    creatorAddress: '0x6E253C951A40fAf4032faFbEc19262Cd1531A5F5',
    saltArg: '0x0000000000000000000000000000000000000000000000000000000000000000',
    codeHash: '0x4f063cd4b2e3a885f61fefb0988cc12487182c4f09ff5de374103f5812f33fe7',
));
```

### func signWithdraw

```dart
String signWithdraw(
    Withdraw tx,
    String tokenSymbol,
    String? chainId,
    String? addr,
)
```

### func signTransfer

```dart
String signTransfer(
    Transfer tx,
    String tokenSymbol,
    String? chainId,
    String? addr,
)
```

### func signForcedExit

```dart
String signForcedExit(ForcedExit tx)
```

### func createSignedOrder

```dart
Order createSignedOrder(Order order)
```

### func signOrderMatching

```dart
String signOrderMatching(OrderMatching tx)
```

### Example
```dart
var signer = Signer.ethSigner(ethPrivateKey: "0xbe725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4");
var maker = signer.createSignedOrder(order: Order(
    accountId: 5,
    subAccountId: 1,
    slotId: 1,
    nonce: 1,
    baseTokenId: 18,
    quoteTokenId: 17,
    amount: "10000000000000",
    price: "10000000000",
    isSell: true,
    makerFeeRate: 5,
    takerFeeRate: 3,
    hasSubsidy: false,
));
var taker = signer.createSignedOrder(order: Order(
    accountId: 6,
    subAccountId: 1,
    slotId: 1,
    nonce: 1,
    baseTokenId: 18,
    quoteTokenId: 17,
    amount: "10000000000000",
    price: "10000000000",
    isSell: false,
    makerFeeRate: 5,
    takerFeeRate: 3,
    hasSubsidy: false,
));
var contractPrices = [
    ContractPrice(pairId: 1, marketPrice: "100"),
    ContractPrice(pairId: 2, marketPrice: "200"),
    ContractPrice(pairId: 3, marketPrice: "300"),
    ContractPrice(pairId: 4, marketPrice: "400"),
];
var marginPrices = [
    SpotPriceInfo(tokenId: 11, price: "100"),
    SpotPriceInfo(tokenId: 12, price: "200"),
    SpotPriceInfo(tokenId: 13, price: "300"),
];
var tx = OrderMatching(
    accountId: 10,
    subAccountId: 1,
    taker: taker,
    maker: maker,
    fee: "1000000000",
    feeToken: 18,
    contractPrices: contractPrices,
    marginPrices: marginPrices,
    expectBaseAmount: "10000000000000000",
    expectQuoteAmount: "10000000000000000",
);
print(signer.signOrderMatching(tx: tx));
```

### func createSignedContract

```dart
Contract createSignedContract(Contract contract)
```

### func signContractMatching

```dart
String signContractMatching(ContractMatching tx)
```

### func signAutoDeleveraging

```dart
String signAutoDeleveraging(AutoDeleveraging tx)
```

### func signFunding

```dart
String signFunding(Funding tx)
```

### func signLiquidation

```dart
String signLiquidation(Liquidation tx)
```
