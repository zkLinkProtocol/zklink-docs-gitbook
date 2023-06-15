# Introduction

---

zkLink is a **unified multi-chain trading infrastructure** secured with **zk-SNARKS**, empowering the next-generation of decentralized trading products such as order book DEX, NFT marketplaces, and more.

zkLink builds a ZK-Rollup middleware that natively connects to various L1s and L2s, and provides   an array of high-level APIs. Developers can easily deploy trading dApps with high customizability and access to aggregated liquidity, while their end users can benefit from seamless multi-chain trading experience. Moreover, zkLink also supports OFT (Omnichain Fungible Token) issuing and bridging. 

## What problems does zkLink solve?

### **Liquidity Fragmentation:**

The emergence of high-performance chains and zkEVMs has resulted in liquidity silos as a side-effect, leading to low capital utilization rates and making it hard for users to hold or trade across distinct ecosystems. 

### **Navigation Complexity:**

Currently the process to trade multi-chain tokens is laborious and costly, involving stablecoins as an intermediary and cross-chain bridges. The unwieldy user experience often deters traders from investigating new DeFi projects and applications.  

### **Security Risks During Inter-Chain Transactions**:

Data transmission have emerged as the most vulnerable component of cross-chain trading, including bridges and chain-interoperability process whose security depends on a multi-sig committees. Security concern hinders users enthusiasm to explore new ecosystems other than Ethereum.

## Benefits of using zkLink infra

### Asset Aggregation

- **Multi-chain Token Listing and Trading:** list and trade tokens across various L1s and L2s, including FTs and NFTs, without a bridge to mitigate cross-chain risks and fees.
- **Multi-chain Token Portfolio Management:** a single wallet to manage multi-chain portfolios, just as in a centralized exchange.

### Liquidity Aggregation

- **Token Merge:** tokens issued on different blockchains by the same entity (e.g., USDT ERC20, USDT SPL, USDT BEP20) will be merged into a single token (USDT) within the zkLink rollup network.
- **Stablecoin Liquidity Unification:** USD, a unified pricing currency, is introduced within the zkLink system, which eliminates disparities among fiat-backed stablecoins from different chains. The auto-conversion from selected stablecoins (USDC, USDP, BUSD, etc,.) to USD is optional for dApps.

### App-specific Zero-knowledge Circuit

- **High customizability:** developers can choose which networks to connect, which to put DA on, and tailor specific functions.
- **High performance:** 1000+ TPS to bridge the gap between high-frequency traders' needs and on-chain products.
- **High efficiency:** the app-specific circuit is considerably smaller than general-purpose zk circuits, resulting in lower computational resource and on-chain gas consumption.
