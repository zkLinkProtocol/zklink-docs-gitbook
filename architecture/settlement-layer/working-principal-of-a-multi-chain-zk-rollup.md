# Working Principal of A Multi-Chain ZK-Rollup

<figure><img src="../../.gitbook/assets/figure2.png" alt=""><figcaption><p>Working principal of a multi-chain ZK-Rollup</p></figcaption></figure>

In comparison to a classic ZK-Rollup architecture, zkLink proposes a new type of ZK-Rollup architecture which requires to deploy the rollup contracts on multiple L1 blockchains and L2 networks at the same time, thus is called a multi-chain ZK-Rollup.

In the architecture of a multi-chain ZK-Rollup, in order to optimize the verification cost, one chain among the connected chains will be designated as the **primary chain**, which is responsible for ZKP verification. The other connected chains will act as **secondary chains,** which receives transactions along with the primary chain.

To ensure security, all on-chain transactions on secondary chains, along with the transactions on primary chain and off-chain apps, are synchronized to the sequencer for execution and generating zk-proof. Through checking of the consistency of transaction sync hashes of different chains and the data  commitment by the sequencer, multi-chain state synchronization can be finalized.

The working principal of a classic ZK-Rollup settlement typically contains three stages: the Commit stage, Prove stage, and Execute stage. zkLink multi-chain ZK-Rollup architecture adds a Synchronize stage after the Prove stage.

The 4 stages are briefly described as below:

**1. Commit:** The sequencer submits all rollup data (excluding actual transaction data in Validium mode) to the primary chain, where the contract on the primary chain executes the complete zero knowledge proof verification process. For multi-chain ZK-Rollup, the sequencer will additionally commit block information to a specific secondary chain if a particular block contains transactions related to that secondary chain.

**2. Prove:** ZKPs are submitted to the primary chain and verified by the smart contract. This process occurs only on the primary chain (Please note the primary chain is required to have the capability to verify zk-SNARKs proofs). For multi-chain ZK-Rollup, the primary chain, during the commit stage, has received transaction information related to all secondary chains, which will be also used to verify a proof.

**3. Synchronize:** The purpose of the Synchronize stage is to achieve states synchronization across the connected chains.  The secondary chains use the cross-chain messaging mechanism to send their respective sync hashes to the primary chain. Once the primary chain receives all the sync hashes from the secondary chains, it compares them with its own calculated sync hashes from by. If ZKPs are verified and sync hashes are consistent, the primary chain notifies the secondary chains that they can proceed with transaction settlement and asset withdrawal.

**4.Execute:** This step aligns with the typical process found in a classic ZK-Rollup.  Based on the finality of multi-chain synchronization, requests for fund withdrawals would be approved.
