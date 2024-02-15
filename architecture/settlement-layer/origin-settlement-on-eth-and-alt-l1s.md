# Origin: Settlement on ETH and Alt-L1s

In contrast to Nexus, zkLink Origin's architecture allows for the integration with Alt-L1s.

A zkLink Origin rollup will settle the transactions and transition of states on all the chains connected, as long as one of the networks (designated as primary chain) supports zk-SNARKs proof verification, and there is a secure and fast way to achieve states synchronization across the networks.

To establish a fast and secure communication mechanism between different chains, zkLink introduces a Light Oracle Network for cross-chain message transfer. The security assumption of zkLink Origin mode is that the rollup sequencers, responsible for packaging transactions, cannot collude with all nodes of the Light Oracle Network for malicious activities.
