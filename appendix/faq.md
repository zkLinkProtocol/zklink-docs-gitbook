# FAQ

#### What’s the difference between “multi-chain” and “cross-chain”

zkLink is a “bridge-less” “multi-chain” trading layer natively connected to multiple L1s and L2s.

“Cross-chain”, on the other hand, usually refers to bridges that “transfer” tokens from one chain to the other as wrapped tokens.

zkLink is a “multi-chain” design and can realize “cross-chain” as a side function.

#### How does zkLink ensure the security of user assets?

First of all, zkLink DOES NOT hold user assets as centralized exchanges do, non-custodial smart contracts do.

Secondly, the security of zkLink is ensured by its zkRollup architecture, which is widely recognized for its security features.

Moreover, the security of zkLink is tested and proved by the market. In the “zkLink Dunkirk User Asset Recovery Test”, even when the extreme case happens (zkLink server is down), users are able to withdraw their tokens from zkLink L2 to MetaMask with the help of [zkLink recovery program](https://github.com/zkLinkProtocol/recover\_state\_server) on GitHub. Check Dunkirk for detailed information.

#### What is SNARK?

SNARK stands for "Succinct Non-interactive ARgument of Knowledge", which is a technology implementation of zero-knowledge proofs. There is already plenty of public information and theses available on this topic.

#### How to get involved?

* Users
  * Users interact with dApps built on zkLink infrastructure. For example, to explore the multi-chain orderbook function, users can go to [ZKEX.com](http://zkex.com) (the first dApp launched on zkLink).
* Developers
  * Builders can easily deploy their products on top of zkLink utilizing the high-level API suits, and have access to various ecosystems and their users.

#### What will decentralization on zkLink look like?

1. dApps acting as sequencers, independently handling transaction batching and on-chain submission;
2. Market-driven validators and provers, allowing anyone to participate in the generation of zero-knowledge proofs via bidding;
3. Joint governance of zkLink DAO and dApps.

#### Does zkLink support EVM?

At the current stage, zkLink does not support EVM (Ethereum Virtual Machine). zkLink abides by the implementation pattern of App-specific Zero-knowledge Circuit. The benefits include high customizability, high performance with 1000+ TPS (Transactions Per Second), and high efficiency due to smaller app-specific circuits resulting in lower computational resource usage and on-chain gas consumption.

#### Do I need zkLink tokens to trade on zkLink?

No, you do not need zkLink tokens to trade on zkLink.

#### What is the difference between an L2 completed, L1 committed, and executed transaction on zkLink Explorer?

Different status: L2 Completed, L1 committed, and Executed transactions. These concepts are specific to Layer 2 transactions.&#x20;

* L2 Completed refers to the user's transaction being successfully batched on the zkLink Layer 2 and awaiting to be uploaded to Layer 1 for ZKP verification;
* L1 committed means the transaction has been successfully uploaded to Layer 1;
* Executed transactions refer to transactions that have been successfully verified on-chain.

####
