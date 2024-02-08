---
description: >-
  This section is designed for dApp developers, offering a concise overview of
  zkLink's infra to streamline the understanding of zkLink's features for
  efficient dApp development and integration.
---

# Developer Overview

### Glossary

The Difference Between zkLink Origin and zkLink Nexus.\[go to zkLink Origin and zkLink Nexus.]

{% tabs %}
{% tab title="L1 and Layer1" %}
zkLink Origin can be deployed on top of different Layer 1s such as Ethereum, Polygon, and BSC, among others. In addition, when referring to Layer 1 (L1) in this article, we’re referring to the chain of which zkLink’s contract is deployed.

In addition, when mentioning “onChain Tx”, we’re specifically referring to transactions made by users (developers) on the Layer 1 base chain.

The chain zkLink is deployed upon and it’s subsequent native token can also be deployed to the zkLink Layer. However, this scenario isn’t a cross-chain bridge, but rather a Rollup Bridge.
{% endtab %}

{% tab title="L2 and Layer2" %}
The deployment networks for both zkLink Origin and zkLink Nexus include Layer 2 networks such as StarkNet, zkSync, Linea, and Arbitrum, among others.

When mentioning Layer2 (L2) in this article, we’re referring to the Rollup Network deployed by the zkLink contract and the onChain Tx specifically refers to transactions made by users (developers) on the Layer 2 network.

The Rollup to which zkLink is connected and its token can be natively deposited to the zkLink Layer.
{% endtab %}

{% tab title="L3 and zkLink Layer3" %}
Because zkLink’s deployment scenarios are mostly Layer2 networks, we define zkLink as a Layer 3. Therefore, in this article, when the term zkLink Layer is mentioned, it’s the same meaning as Layer 3.
{% endtab %}
{% endtabs %}

## Integrate Architecture

<figure><img src="../.gitbook/assets/rollup (9) (1).jpg" alt=""><figcaption><p>figure. 1</p></figcaption></figure>

zkLink X's Multi-Chain ZK-Rollup technology is composed of both on-chain and off-chain components.

The off-chain component is termed the Off-chain State Tree, and all account states are stored in the Offchain State Tree. The zkLink Validator plays the role of maintaining the Offchain State Tree. Any change that occurs in the account state, a corresponding zero-knowledge proof (zk-proof) must be generated. A Prover is responsible for generating the zk-proof.

The on-chain component is the smart contract that’s deployed on the different chains, which is referred to as the zkLink Contract. In addition to being responsible for verifying the zero-knowledge proof, the on-chain smart contract also needs to coordinate the user’s Deposit and Withdraw actions.

Different from the zkEVM rollup, zkLink X is an application-specific Rollup. So under normal circumstances, users will not directly interact with a zkLink Validator.

### OnChain Component

There are two on-chain protocols that are currently supported by zkLink: EVM and Cairo VM.

* Processing logic related to deposits and withdrawals
* Verifying zero-knowledge proofs
* Processing interaction logic related to Data Availability (DA)
* Processing the logic of multi-chain state synchronization
  * The multi-chain state synchronization logic of zkLink Origin and zkLink Nexus are different.

### OffChain Component

* **DApp Back-end Service**: Depending on the DApp’s business type, the functions of this module will vary. For example, regarding commonly used Central Limited Order Book (CLOB) exchange, the most critical function of the DApp’s back-end service is to maintain the order book and the matching engine.
* **zkLink Validator**: The zkLink Validator is responsible for receiving and processing transactions from the DApp, and checking whether the transactions comply with the rules defined by the Circuit. The Validator is also responsible for organizing the Prover to generate a zk-proof and submit the zk-proof to the zkLink Contract. When in development, the developer does not need to care about the details of how the Validator operates, instead, the developer only needs to refer to the SDK provided by zkLink in order to quickly integrate the services provided by the Validator. In the end, this saves the developer tremendous effort.
* **Prover**: The Prover is responsible for generating the zero-knowledge proof.

***

## Quickly Understand Interaction Flows

In the following section we’ll describe the basic development process for building on zkLink Infra for users and DApp developers.

### User Interaction

In order to provide developers with the ability to quickly understand the development process on zkLink, we’ll select a sample of common operations as case examples.

#### **1. Account Creation and Activation**

The Offchain State Tree maintained by the Validator records all account and asset information. When a user makes a deposit for the first time from on-chain (whether a Layer 1 or a Layer 2) to the zkLink Layer, the Offchain State Tree will create a new Account record unit, which we refer to as an L3 Account (zkLink Account).&#x20;

After creating a zkLink Account, the next step is the activation process.

Activate means that a user generate a zkLink key to operate the zkLink Account. The operation of specifying the zkLink key is called `ChangePubkey` . The user generates a pubkey hash based on the zkLink key and signs a `ChangePubkey` transaction by controlling the zkLink Account wallet (EOA or Passkey). The signature states: “the zkLink key corresponding to this pubkey hash can control my account.”

After a zkLink Validator receives the `ChangePubKey` transaction, the Circuit will verify and record the pubkey hash in the Offchain State Tree.

#### 2. User Deposit Token to Dapp

Users can directly call the zkLink Contract to perform deposit operations (generally speaking, the front-end page for user operations is provided by the DApp, and hence, from the user’s perspective, the entire process is interacting with the DApp directly).

The zkLink Validator will continue to synchronize with the Layer1 block, and after obtaining the Deposit Event, if it’s the first time the user deposits, a default account will be created. After the account and account balance are updated, the DApp will be notified through websocket (or kafka).

Since users interact directly with the chain, they will need to pay gas fees. However, if the user is using a AA Wallet, they may not need to pay the gas fee directly.

In addition, zkLink supports Passkey-based account solutions.

#### 3. User Withdraw Asset to Layer1 (Layer2)

The user is required to personally sign Withdraw Transaction (note: this is not an onChain tx, and there is no need for the user to consider gas fee issues). The user submits the Signature to the DApp, and after the DApps confirms the Signature, it is sent to the zkLink Validator.

Under normal circumstances, one would need to wait until the zkLink Validator completes steps b1/b2/b3 to implement the zero-knowledge proof and upload it to the chain before the deposit can be received. Users can also select Fast Withdraw, and after signing, the acceptor (broker) will immediately submit the payment to the user on the Layer 1 (or Layer 2). However, the user needs to pay a fee to the acceptor.

The DApp can customize the rules for the Withdrawal Fee.

#### 4. User Place Order

Taking a trading DApp as an example. The DApp maintains the Order book and the user can sign a maker (or taker) order. The DApp’s Matching Engine then is responsible for matching orders from different users. Once the matching is successful, the DApp will initiate a `ContractMatching Transaction` based on the order information from the taker and maker, and send it to the zkLink Validator.

User Place Order does not need to interact with the chain, which means that similar to Withdraw, users do not need to consider issues related to gas costs. Moreover, DApps can customize their trading fee rules and ration (zkLink Circuit will constrain the upper limit of handling fees).

### Developer Interaction

In this section, we have chosen a few scenarios that developers most care about to help quickly understand zkLink’s interaction process.

#### 1. User Deposit Token to Dapp

Users can directly call the zkLink Contract to perform deposit operations (generally speaking, the front-end for users is provided by the DApp, and hence, the entire process is the user interacting with the DApp directly).

The zkLink Validator will sustain synchronization with the Layer 1 block. After obtaining the Deposit Event, if it is the first time the user deposits, a default account will be created. After the account and balance are updated, the DApp will be notified through websocket (kafka).

Generally speaking, after receiving notification from the zkLink Validator, the DApp should maintain a copy of the user account balance data locally in the DApp.

#### 2. Matching Engine

The Matching Engine saves the order information signed by the different users. Once the matching is successful, the DApp makes a `ContractMatching Transaction` (for Spot trades, it is `Order Matching Transaction`), and sends it to the zkLink Validator. After receiving the Transaction, the zkLink Validator will immediately return it to the DApp with a message indicating whether the execution was successful.

After the DApp receives the receipt from the zkLink Validator, the transaction can be considered as settled. The DApp’s front-end page should inform the user that the transaction was successful.

For users, zkLink’s Validator process of generating proofs (b1/b2/b3) does not affect the user’s transaction speed, but only affects the execution speed of the user’s Standard Withdraw transaction. Because Standard Withdraw must wait for the transactions inside the batch to complete the zero-knowledge proof verification before it can be executed on-chain. Of course, users can also choose Fast Withdraw and complete the process promptly.

#### 3. Interact with zkLink validator

In order to give the DApp authority, zkLink defines the Submitter role where all transactions sent by the DApp to the zkLink Validator require the Submitter’s signature. Send to zklink validator via rpc call with submitter signature (L3 signature of EdDSA private key, see Private key & Signature for details)&#x20;

The zkLink Validator will verify the user’s Transaction signature as well as the Submitter signature. The Transaction can only be processed after the signature verification is valid.

Note that the zkLink Validator has a whitelist of Submitter public keys in the background. Therefore, before forwarding user Transactions, one needs to provide the Submitter public key to zkLink Validator to join the whitelist. At the same time, keep the private key used as a Submitter signature appropriately.

#### 4. Transactions Users Do Not Partake In

In certain circumstances, the DApps needs to change the user status and settings, but this does not need to be initiated by the user, for example, liquidation, ADL (auto-deleveraging), funding rate, and other operations. zkLink provides the corresponding Transaction types for these operations. The operation method also requires the Submitter to sign and submit Transaction to the zkLink Validator.

### What Does The zkLink Validator Do？

#### 1. Generate Layer 3 Block

After the zkLink Validator receives a sufficient number of Transactions, it will generate a new Layer 3 block.

#### 2. Prove Layer 3 Block

The zkLink Validator will send the Layer3 Block and the witness required for generating the zk proof to the Prover. (b1/b2 in Figure 1).

#### 3. Aggregate Proofs

The zkLink Validator will recursively aggregate the zk-proofs that correspond to multiple Layer 3 Blocks to generate recursive zk-proofs. The purpose of this is to reduce the number of zk-verifications on-chain and to reduce gas costs.

#### 4. Send Proof to Layer1（Layer2）

Send the aggregated zero-knowledge proof to the chain to complete the verification of the zero-knowledge proof (b3 in Figure 1).

#### 5. Monitor onChain Event

The zkLink Validator monitors contract events on-chain, obtains the on-chain status of the signature, and changes the status of the corresponding Transaction.
