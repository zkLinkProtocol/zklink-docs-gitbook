# Sequencing Layer

The sequencing layer is a pivotal component in rollup systems, primarily responsible for receiving user transactions, sequencing the transactions and bundling them into batches. These batches are then committed to the settlement layer. Additionally, in scenarios where the system employs an external DA layer, the sequencer also ensures efficient transmission of transaction data to the DA layer.

Similar to other rollups, App Rollups will start with a centralized sequencer model. While this approach offered certain development efficiencies, it also presents challenges and risks, such as potential single point of failure, transaction censorship and issues around miner extractable value (MEV), affecting network fairness and transparency.

To address these concerns, zkLink X aims to incorporate decentralized sequencer solutions. These solutions, including platforms like Espresso, Astria, and Fairblock, aim to mitigate centralization risks by processing and validating transactions across a distributed node network. This strategy will not only boost network security and transparency but also strive to offer a more secure, equitable, and efficient rollup solution to its users.
