

# **Architecture**



Note： unless otherwise specified, the “Layer1” mentioned in this documentation refers to the various ecosystems that zkLink connects to, including but not limited to:

1. EVM-compatible public chains such as Ethereum mainnet, Binance Smart Chain, Polygon, Avalanche, etc,.
2. EVM-compatible ZK-Rollup VM including zkSync, Scroll, Linea, Polygon ZKVM, Taiko
3. EVM-incompatible ZK-Rollup VM: Starknet
4. EVM-compatible Optimistic-Rollups: Arbitrum, Optimism

Note: to avoid misunderstanding, we use “zkLink Layer2” or “zkLink off-chain rollup service” to refer to the infrastructure that zkLink developed. Although zkLink is more like a Layer3 network considering other Layer2s such as Starknet or zkSync that zkLink connects to, we identify zkLink as a Layer2 solution.


- 1 ethereum mainnet，bsc, polygon,avax 等evm兼容的公链
- 2 evm兼容的zkrollup vm,包括 zksync，scroll，linea，polygon zkvm,Taiko
- 3 EVM 不兼容的 zkrollup vm, 目前仅包括 StarkNet
- 4 EVM 兼容的OP rollup,目前包括 arbtrium，optimism



## Roles in the zkLink Ecosystem

| User | Users of dApps that are deployed on the zkLink infrastructure.  👉 User Interactions |
| --- | --- |
| dApp Developers | Developers who build dApps and can also act as Sequencers. In some cases, dApp developers may not need to run their own Sequencer, as the zkLink team provides a shared Sequencer service. |
| Layer1s and Layer2s | Various Layer1 and Layer2 ecosystems that zkLink connects to can be combined into a single network module. zkLink service can be customized and deployed on any compatible network module. 👉 Connected Network |
| Data Availability Layer | Different DA solutions are available including internal and external DAs. zkLink provides multiple options for developers to choose from. 👉 Data Availability |
| Light Oracle Network | In certain network configurations, zkLink architecture requires the assistance of a light oracle network for inter-chain communication. This is dependent on the specific version of zkLink rollup and the combination of connected ecosystems. |
| Community Participants | - Validators: coordinate with provers and upload ZKPs on-chain; receive incentives;
- Provers: generate zero-knowledge proofs to receive incentives;
- zkLink DAO: conduct governance activities for the stability of zkLink protocol; receive incentives. |
| zkLink Team | Develop, iterate, and maintain the zkLink protocol; coordinate with dApps and the community. |


## User Interactions

1. Users interact with dApps’ front-end. In the backstage, tokens deposited to the dApp will be locked in the zkLink contract (which may be exclusively deployed for a specific dApp). Users need to wait for a certain period of time (determined by the source chain) before the dApp receives this deposit.
2. Once the dApp confirms the deposit, the user will be able to see it in their balance and use other dApp features. In the case of ZKEX.com, users can do multi-chain spot trading or open positions in perpetual contracts after receiving the deposit. Read Deposit Flow for more details.   
3. Users can withdraw tokens at any time. They can either request on the dApp’s front-end or directly submit the withdrawal request to zkLink main contract. Read Withdraw Flow for more details.

Note:

1. Tokens might not necessarily be deposited directly from the user account into the zkLink main contract; developers can deploy separate proxy deposit contracts for higher flexibility. For example, zkJump deploys a proxy deposit contract and realizes bridging fee collection.
2. In some cases users will also interact directly with zkLink: forced withdraw (users initiate forced withdraw directly to zkLink Layer1 contract), and Dunkirk exit (users directly withdraw tokens from zkLink main contract when zkLink enters Dunkirk mode, which is irreversible)

<aside>
💡 About Dunkirk: the “Dunkirk Asset Recovery” program has been publicly tested to simulate the extreme case: when zkLink server is down, users can retrieve their assets via the open-source recovery program.

</aside>


## Developer Intergrations

Understand the development needs：

- zkLink is a unified multi-chain trading infrastructure secured with zk-SNARKS;
- zkLink excels at peer-to-peer trading scenarios, particularly order book trading;
- zkLink currently supports spot trading and derivative trading;
- zkLink currently supports depositing and merging of OFT, enabling OFT cross-chain transfers:
    - For example, USDC is issues on both Ethereum and BSC, and can be deposited to zkLink from either chain; users can deposit from Ethereum and withdraw to BSC;
    - merge: the same kind of token on different chains are automatically merged into a unified one on zkLink;
- zkLink will soon support the issuance and trading of NFTs;
- zkLink is not designed for direct integration with on-chain DeFi protocols or other Layer1 smart contracts. As an independent rollup infrastructure, zkLink acquires cost advantages at the expense of composability.

Explore integration examples:

1. ZKEX 3.0
2. ZKEX 2.0
3. zkJump

Steps to develop on zkLink:

1. Contact the zkLink team about integration details;
2. Stake a certain amount of tokens with the zkLink DAO to gain access to Sequencer system (optional; you can also opt for the shared Sequencer service);
3. Launch your product once everything is set up.
