# Origin: Settlement on ETH and Alt-L1s

In contrast to Nexus, zkLink Origin's architecture allows for the integration with OP-L2s and Alt-L1s, in addition to ZK-L2s.

A zkLink Origin rollup will settle the transactions and transition of states on Ethereum, ALt-L1s, OP-L2s, and ZK-L2s, corresponding to the actual networks connected, as long as one of the networks supports zk-SNARKs proof verification, and there is a secure and fast way to achieve states synchronization across the networks.

To establish a fast and secure communication mechanism between the alt-L1s and Ethereum, zkLink introduces a Light Oracle Network for cross-chain message transfer, the process of which is described in section 4.3. Therefore, the security assumption of zkLink Origin is that the rollup sequencers, responsible for packaging transactions, cannot collude with all nodes of the Light Oracle Network for malicious activities.
