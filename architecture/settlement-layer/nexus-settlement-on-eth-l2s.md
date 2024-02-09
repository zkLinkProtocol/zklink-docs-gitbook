# Nexus: Settlement on ETH L2s

A zkLink Nexus L3 rollup settles the transactions and transition of states directly on Ethereum's L2s, and the L2s settle on Ethereum. The finality of the transactions on a zkLink Nexus L3 rollup will be ultimately achieved on Ethereum layer1.

<figure><img src="../../.gitbook/assets/figure3.png" alt=""><figcaption><p>Security inheritance of zkLink Nexus</p></figcaption></figure>

The settlement process of a Nexus L3 rollup is shown above. Nexus rollup posts data and ZKP to the connected L2s, where the correctness of the states and transactions is verified via the verifier contract.&#x20;

Since the correctness of the states and transactions of L2s will be finalized on Ethereum via validity proof (for ZK-Rollup L2s) & fraud proof (for Optimistic-Rollup L2s), and multi-chain state synchronization is finalized via Ethereum, it could be stated that a zkLink Nexus L3 rollup inherits the security of Ethereum.&#x20;
