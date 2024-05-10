## Type JsonRpcSigner(for browser)
L1 private key.

### constructor

```javascript
/**
* @param {provider}
*/
newRpcSignerWithProvider(provider)
```

### Example

```javascript
await window.ethereum.request({ method: 'eth_requestAccounts' });
const provider = window.ethereum;
const signer = new wasm.newRpcSignerWithProvider(provider);
```

### func initZklinkSigner

```javascript
/**
* @param {string | undefined} [signature]
* @returns {Promise<void>}
*/
initZklinkSigner(signature)
``` 
Initialize ZkLink L3 private key

### func address

```javascript
/**
* @returns {string}
*/
address()
```
Return address

### func signatureSeed

```javascript
/**
* @returns {string}
*/
signatureSeed()
```
Return signature seed

## Type Signer(for nodejs)
L1 private key.

### constructor

```javascript
/**
* @param {string} private_key: hex string of private key(with or without `0x` prefix)
* @param {L1Type.Eth | L1Type.Starknet}
* @param {string | undefined} [starknet_chain_id]
* @param {string | undefined} [starknet_addr]
*/
Signer(private_key, l1_type, starknet_chain_id, starknet_addr)
```
Create a Eth or Starknet private key signer.

### func getPubkey

```javascript
/**
* @returns {string}
*/
getPubkey()
```
Return hex string of public key.

### func getPubkeyHash

```javascript
/**
* @returns {string}
*/
getPubkeyHash()
```
Return hex string of public key hash.

### func signMusig

```javascript
/**
* @param {Uint8Array} msg
* @returns {TxZkLinkSignature}
*/
signMusig(msg)
```
Sign and create [ZkLinkSignature](../../api-and-sdk/data-types/basic-types.md#zklinksignature) from raw message.

### func signChangePubkeyWithOnchain

```javascript
/**
* @param {ChangePubKey} tx
* @returns {json object} json string of tx
*/
signChangePubkeyWithOnchain(tx)
```

### func signChangePubkeyWithEthEcdsaAuth

```javascript
/**
* @param {ChangePubKey} tx
* @returns {json object} json string of tx
*/
signChangePubkeyWithEthEcdsaAuth(tx)
```

### func signChangePubkeyWithCreate2DataAuth

```javascript
/**
* @param {ChangePubKey} tx
* @param {Create2Data} create2_data
* @returns {json string of tx}
*/
signChangePubkeyWithCreate2DataAuth(tx, create2_data)
```

### Example

```javascript
const private_key = "00f0dfe9e420b857beb5165c4dbe4b21561dc4ca0206ca97f6ee7ad53bc79cab";
const new_pubkey_hash = "0x8255f5a6d0d2b34a19f381e448ed151cc3a59b9e";
const ts  = Math.floor(Date.now() / 1000);
try {
    let tx_builder = new ChangePubKeyBuilder(
        16,21,0,new_pubkey_hash,140,"1",
        0,"0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001b",
        ts);
    let tx = newChangePubkey(tx_builder);
    let addr = "0x04A69b67bcaBfA7D3CCb96e1d25C2e6fC93589fE24A6fD04566B8700ff97a71a";
    const signer = new Signer(private_key, L1Type.Starknet, "SN_GOERLI", addr);
    let creator_address = "0x6E253C951A40fAf4032faFbEc19262Cd1531A5F5";
    let salt = "0x0000000000000000000000000000000000000000000000000000000000000000";
    let code_hash = "0x4f063cd4b2e3a885f61fefb0988cc12487182c4f09ff5de374103f5812f33fe7";
    let tx_signature = signer.signChangePubkeyWithCreate2DataAuth(tx, new Create2Data(creator_address, salt, code_hash));
    console.log(tx_signature);
} catch (error) {
    console.error(error);
}
```

### func signWithdraw

```javascript
/**
* @param {Withdraw} tx
* @param {string} token_symbol
* @param {string | undefined} [chain_id]
* @param {string | undefined} [addr]
* @returns {json object} json string of tx
*/
signWithdraw(tx, token_symbol, chain_id, addr)
```

### func signTransfer

```javascript
* @param {Transfer} tx
* @param {string} token_symbol
* @param {string | undefined} [chain_id]
* @param {string | undefined} [addr]
* @returns {json object} json string of tx
*/
signTransfer(tx, token_symbol, chain_id, addr)
```

### func signForcedExit

```javascript
/**
* @param {ForcedExit} tx
* @returns {json object} json string of tx
*/
signForcedExit(tx)
```

### func createSignedOrder

```javascript
/**
* @param {Order} order
* @returns {Order} signed order
*/
createSignedOrder(order)
```

### func signOrderMatching

```javascript
/**
* @param {OrderMatching} tx
* @returns {json object} json string of tx
*/
signOrderMatching(tx)
```

### Example
```javascript
const private_key = "be725250b123a39dab5b7579334d5888987c72a58f4508062545fe6e08ca94f4";
try {
    const signer = new Signer(private_key, L1Type.Eth);
    const contract_price1 = new ContractPrice(0,"1");
    const contract_price2 = new ContractPrice(1,"1");
    const contract_price3 = new ContractPrice(2,"1");
    const contract_price4 = new ContractPrice(3,"1")
    let contract_prices = [];
    contract_prices.push(contract_price1.jsonValue());
    contract_prices.push(contract_price2.jsonValue());
    contract_prices.push(contract_price3.jsonValue());
    contract_prices.push(contract_price4.jsonValue());

    let margin_prices = [];
    const margin_price1 = new SpotPriceInfo(17,"1");
    const margin_price2 = new SpotPriceInfo(141,"1");
    const margin_price3 = new SpotPriceInfo(142,"1");
    margin_prices.push(margin_price1.jsonValue());
    margin_prices.push(margin_price2.jsonValue());
    margin_prices.push(margin_price3.jsonValue());
    let maker_order = new Order(5,20,1,1,18,17,"10000000000000","10000000000",true,5,3);
    let maker = signer.createSignedOrder(maker_order);
    console.log(maker);
    let taker_order = new Order(5,20,1,1,18,17,"10000000000000","10000000000",false,5,3);
    let taker = signer.createSignedOrder(taker_order);

    let tx_builder = new OrderMatchingBuilder(5,20,taker,maker,"11",17,contract_prices,margin_prices,"4343433","3957485749");
    let tx = newOrderMatching(tx_builder);
    console.log(tx);
    let tx_signature = signer.signOrderMatching(tx);
    console.log(tx_signature);
} catch (error) {
    console.error(error);
}
```

### func createSignedContract

```javascript
/**
* @param {Contract} contract
* @returns {Contract} signed contract
*/
createSignedContract(contract)
```

### func signContractMatching

```javascript
/**
* @param {ContractMatching} tx
* @returns {json object} json string of tx
*/
signContractMatching(tx)
```

### func signAutoDeleveraging

```javascript
/**
* @param {AutoDeleveraging} tx
* @returns {json object} json string of tx
*/
signAutoDeleveraging(tx)
```

### func signFunding

```javascript
/**
* @param {Funding} tx
* @returns {json object} json string of tx
*/
```

### func signLiquidation

```javascript
/**
* @param {Liquidation} tx
* @returns {json object} json string of tx
*/
```
