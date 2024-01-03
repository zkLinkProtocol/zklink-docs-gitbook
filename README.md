---
description: 'zkLink: A Multi-Chain Rollup Infrastructure Based on Zero-Knowledge Technology'
---

# Introduction

As the blockchain space undergoes rapid advancement, there is an increasing number of layer 1 blockchains such as Ethereum, Solana, Avalanche, and Ethereum layer 2 scaling solutions including ZK-Rollups and Optimistic Rollups. Therefore, an intricate multi-chain, multi-layer landscape has emerged as the current reality and is deemed the inescapable future of the crypto ecosystem where users navigate between the different L1 chains and L2 rollups to satisfy their particular requirements, utilizing a diverse range of crypto tokens.

Despite this multi-chain, multi-layer landscape creating considerable value for crypto users worldwide, it has also brought about unforeseen challenges. These challenges include liquidity silos that are isolated to specific chains, increased trading costs for multichain assets, fragmented user experience, and a complex application development environment.

1.  **Liquidity fragmentation.**

    The rise of various new blockchain and rollup networks has led to fragmented liquidity. As a result, this situation of liquidity fragmentation makes it difficult for users to manage their assets and carry out transactions across chains, resulting in lower capital use rates.
2.  **Multi-chain product deployment challenge**.

    As the Ethereum Layer 2 ecosystems and alternative Layer 1 chains continue to grow, they form liquidity silos. Developers, therefore, need to deploy their products on various networks in order to attract users and liquidity. However, different programming languages and tools, such as EVM, CairoVM, and Solana VM, present notable challenges to developers. Even entirely compatible protocols like multiple EVM compatible chains (Rollups) have many subtle differences.
3.  **Navigation complexity and high cost.**

    Previously, users found it difficult and expensive to navigate between blockchains, for example, to swap tokenA on chainA for tokenB on chainB through a DEX. This procedure turned out to be quite intricate and involves multiple fees.

    * First, it requires the installation of a wallet and purchase of the gas token for chainB.
    * Next, users need to trade tokenA for a stablecoin or another intermediary token which can be bridged to chainB.
    * Then, users need to purchase tokenB on a local DEX.

    The emergence of numerous cross-chain asset bridge applications has improved this experience to an extent. However, the cost of cross-chain token exchange remains high, and users still struggle with conducting affordable and seamless token interactions across different chains.
4.  **Security risks during inter-chain transactions.**

    Preserving asset security poses a formidable technical challenge in cross-chain transactions. For example, in recent years, cross-chain asset bridges have been one of the most susceptible components to hacks in the crypto ecosystem.

zkLink addresses the above challenges of blockchain interoperability and standardization by building a multi-chain rollup infra protocol to simplify multi-chain dApp deployment, and resolve liquidity fragmentation. It leverages zero-knowledge proof technology to provide a high throughput, low-cost App Rollup deployment solution.

By harnessing the potential of zero-knowledge proof technology, the zkLink protocol features key functionalities such as:

* Multi-chain liquidity aggregation across L1 blockchains and L2 rollups.
* Quick multi-chain product deployment with SDK and APIs.
* A trading-specific-zkVM, empowering high-throughput, low-cost App Rollup solution for high performance financial applications such as Order Book DEX.

## Key Features of zkLink Protocol

### Native Asset Aggregation

Applications using zkLink rollup infra solution will be able to access and list the native tokens across the connected L1s and L2s, including FTs and NFTs, allowing users to trade multi-chain assets on a unified user interface. Cross-chain asset bridges are not needed in the process, thus avoiding cross-chain asset bridging risks and bridging fees.

At the same time, multi-chain token portfolios can be managed with a single wallet. For instance, Alice deposits 2 UNI from her Metamask wallet to zkLink on Ethereum, and then deposits 3 BNB from BNB Chain to zkLink from the same wallet address — as a result, Alice will receive 2 UNI + 3 BNB under the same wallet address on the zkLink rollup network. This hypothetical example applies the same to tokens from Polygon, Starknet, zkSync, Linea, Arbitrum, Optimism, Scroll, and Solana, etc. Therefore, users can easily manage their multi-chain token portfolios using a single wallet with a simplified user experience.

### Liquidity Aggregation and Unification

Tokens issued on different L1 chains and L2 rollups by the same entity, for instance, USDT ERC20, USDT BEP20, USDT ARB, etc — will be merged into a single USDT token in the zkLink App Rollups and zkLink L3 network.

The same applies for ETH. As ETH is the native asset for Ethereum and all the Ethereum layer 2 networks, ETH from Ethereum, zkSync, and Starknet, etc., will be merged into a single ETH token, thus eliminating chain disparities.

In summary, tokens of the same kind but issued on various chains will be merged into one single token, fostering unified and aggregated liquidity.

### Customizable App Rollup Deployment

zkLink protocol decouples the four layers of the rollup framework and provides fast and customizable rollup deployment solution. zkLink is focused on the development of execution layer plus settlement layer, and will integrate third party modular solutions for DA layer and sequencing layer, allowing developers to customize the key components to meet diverse demands of different use cases.

* **Network Collections and Settlement Layer Solution**. Developers can choose which chains the App Rollup can access to, including but not limited to: ETH, BNB Chain, Avalanche, Polygon PoS, Solana, zkSync, Starknet, Scroll, Polygon zkEVM, Linea, Taiko, Arbitrum, Optimism, Base, etc.
* **Execution Environment**: TS-zkVM.
* **Decentralized Sequencer**: Espresso, Astria, Fairblock, etc.
* **Modular DA Solutions**: In addition to Ethereum, developers can choose Celestia, EigenDA, Polygon Avail, DAC organized by zkLink, etc.

