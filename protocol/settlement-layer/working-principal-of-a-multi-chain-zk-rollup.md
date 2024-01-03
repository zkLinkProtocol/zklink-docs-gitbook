# Working Principal of A Multi-Chain ZK-Rollup

<figure><img src="../../.gitbook/assets/figure2.png" alt=""><figcaption><p>Working principal of a multi-chain ZK-Rollup</p></figcaption></figure>

In comparison to a classic ZK-Rollup architecture, zkLink proposes a new type of ZK-Rollup architecture which requires to deploy the rollup contracts on multiple L1 blockchains and L2 networks at the same time, thus is called a multi-chain ZK-Rollup.

In the architecture of a multi-chain ZK-Rollup, in order to optimize the verification cost, only one chain among the connected chains will be designated as the **primary chain**, which is responsible for ZKP verification, while the other chains will act as **secondary chains** that do not need to execute ZKP verification. It is mathematically equivalent to completing the verifications on all chains.

The working principal of a classic ZK-Rollup typically contains three stages: the Commit stage, Prove stage, and Execute stage. zkLink multi-chain ZK-Rollup architecture adds a Synchronize stage after the Prove stage.

The 4 stages are briefly described as below:

**1. Commit:** The sequencer submits all rollup data (excluding actual transaction data in Validium mode) to the primary chain, where the contract on the primary chain executes the complete zero knowledge proof verification process. Additionally, the sequencer will only commit block information to a specific secondary chain when a particular layer3 block contains transactions related to that secondary chain.

**2. Prove:** ZKPs \*\*\*\*are submitted to the primary chain and verified by the smart contract. This process occurs only on the primary chain. Therefore, the primary chain is required to have the capability to verify zk-SNARKs proofs.

**3. Synchronize:** The purpose of the Synchronize stage is to achieve states synchronization across the connected chains. After completing ZKP verification, the primary chain calculates the sync hashes for the different secondary chains. The primary chain, during the commit stage, has received transaction information related to all secondary chains, allowing it to calculate the sync hashes of the secondary chains.

And then, the secondary chains use the cross-chain messaging mechanism, which is detailed in section 4.2, to send their respective sync hashes to the primary chain. Once the primary chain receives all the sync hashes from the secondary chains, it compares them with its own calculated sync hash. If they are consistent, the primary chain notifies the secondary chains that they can proceed with transaction settlement and asset withdrawal.

**4.Execute:** This step aligns with the typical process found in a classic ZK-Rollup, which involves approving requests for fund withdrawals.
