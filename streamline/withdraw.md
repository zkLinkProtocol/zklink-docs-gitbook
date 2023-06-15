# Withdraw

---
There are 4 types of the withdraw function : withdraw, forced withdraw (permissionless),  proxy withdraw, and Dunkirk exit.

---
## Withdraw Calls


|           | EVM Signature | NonEvm Signature | Comment |
|-----------|--------|---------|---------|
|Forced Withdraw | • `ECDSA`([secp256k1](https://en.bitcoin.it/wiki/Secp256k1))  | • `ECDSA`([Stark Curve](https://docs.starkware.co/starkex/crypto/stark-curve.html))  | On-chain transaction that needs to be initiated towards the according networks |
|Dunkirk Exit      | • `ECDSA`([secp256k1](https://en.bitcoin.it/wiki/Secp256k1)) | • `ECDSA`([Stark Curve](https://docs.starkware.co/starkex/crypto/stark-curve.html))  | On-chain transaction that needs to be initiated towards the according networks |
|Withdraw      | • `ECDSA`([EIP712](https://eips.ethereum.org/EIPS/eip-712), [secp256k1](https://en.bitcoin.it/wiki/Secp256k1))  <br/> • `EDDSA`([alt_babyjubjub](https://docs.rs/sapling-crypto_ce/latest/sapling_crypto_ce/alt_babyjubjub/index.html)) | • `ECDSA` ([Stark Curve](https://docs.starkware.co/starkex/crypto/stark-curve.html))&nbsp;&nbsp;&nbsp; <br/> • `EDDSA` ([alt_babyjubjub](https://docs.rs/sapling-crypto_ce/latest/sapling_crypto_ce/alt_babyjubjub/index.html))| zkLink L2 operation that requires two signatures: ECDSA for verification from dApp-end, and EDDSA for circuit verification |
|Proxy Withdraw      | • `ECDSA`([EIP712](https://eips.ethereum.org/EIPS/eip-712), [secp256k1](https://en.bitcoin.it/wiki/Secp256k1))  <br/> • `EDDSA`([alt_babyjubjub](https://docs.rs/sapling-crypto_ce/latest/sapling_crypto_ce/alt_babyjubjub/index.html)) | • `ECDSA` ([Stark Curve](https://docs.starkware.co/starkex/crypto/stark-curve.html))  <br/> • `EDDSA` ([alt_babyjubjub](https://docs.rs/sapling-crypto_ce/latest/sapling_crypto_ce/alt_babyjubjub/index.html))| zkLink L2 operation that requires two signatures: ECDSA for verification from dApp-end, and EDDSA for circuit verification |
|Comment | Ethereum, zkSync, Scroll, Linea, BSC, Polygon, Avalanche, etc,. | In the current version, the only non-EVM network that zkLink supports is Starknet |

**Note:**
- `proxy withdraw` applies to accounts that can not generate pubkeyhash. For example, a user mistakenly transfers tokens to a smart contract address that does not support pubkeyhash generation. To withdraw the token from Layer2 to Layer1 in such a case, a third party proxy is required to send the withdraw request . Noted that the to_address must be a smart contract address.

## Fast Withdraw


>💡 **Fast Withdraw** is not a Layer2 function, but a supplementary feature to Layer2 withdraw function.


zkLink verify contract supports Brokers to prepay the withdrawal to users as a substitute to regular withdraws, only if the user agree to pay the broker fee. 

The record of prepayment information is stored in Layer1 smart contracts. When a withdraw is zk_verified on-chain, the according prepayment record will be checked in `accepts`; if it is included, the to_address will be replaced with the broker address.

Note:

- The serial execution of the broker logic defined in the smart contract makes sure that: 1. multiple brokers cannot take the same fast withdraw request simultaneously; 2. a single request cannot be approved multiple times.
- The broker records are stored in the contract. Once the prepayment is successfully executed, the broker will definitely receive the prepaid principal.

```
	/// @dev Accept infos of fast withdraw of account
    /// uint32 is the account id
    /// byte32 is keccak256(abi.encodePacked(receiver, tokenId, amount, withdrawFeeRate, nonce))
    /// address is the accepter
    mapping(uint32 => mapping(bytes32 => address)) public accepts;

    /// @dev Broker allowance used in accept, accepter can authorize broker to do accept
    /// @dev Similar to the allowance of transfer in ERC20
    /// @dev The struct of this map is (tokenId => accepter => broker => allowance)
    mapping(uint16 => mapping(address => mapping(address => uint128))) internal brokerAllowances;
```

![Fast Withdraw Flow](../img/fast_withdraw_flow.jpg)