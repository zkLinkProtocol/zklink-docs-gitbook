# In-Detail: Nexus Multi-Chain State Synchronization

In zkLink's Nexus mode, multi-chain state synchronization is achieved by transmitting sync hashes. This process is facilitated by an official message bridge, also known as a canonical message service, which is deployed by the Layer2 Network team. Once these sync hashes from various chains are received on the Ethereum mainnet, they undergo a consistency check. Upon successful verification, the confirmation information is relayed back to the Layer2 network via the same official message bridge.

<figure><img src="../../../.gitbook/assets/photo_2024-01-14 15.01.23.jpeg" alt=""><figcaption><p>Nexus Multi-Chain State Synchronization</p></figcaption></figure>

{% hint style="warning" %}
For simplicity and ease of understanding, the above diagram uses only two zkRollup Layer2s as examples and does not represent the actual network deployment scenario.
{% endhint %}

### Contracts deployed by zkLink Protocol

The zkLink Protocol deploys several contracts, as featured in the image above, which include:

* **zkLink Main Contract:** This is the principal contract of zkLink, encompassing functions for user deposits, zk verification, and sync hash.
* **Linea L1 Gateway, zkSync Era L1 Gateway :** These gateway contracts which are deployed on the Ethereum Mainnet, are tasked with comparing the sync hash and forwarding the results to the zkLink Main Contract on Layer2 Network via the official message bridge.
* **Linea L2 Gateway, zkSync Era L2 Gateway:** These gateway contracts which are deployed on the Layer2 Network are responsible for sending the sync hashes to the Arbitrator contract on Ethereum through the official message bridge.
* **Arbitrator:** This contract receives and stores zkLink's sync hashes from various zk Layer2 Networks, performs consistency verification, and then uses the `confirmBlock` method to transmit the comparison results to each Layer2 Network's zkLink Main contract."

### Contracts deployed by Layer2 Network

* **Linea Canonical Message Service**
* **zkSync Era Canonical Message Service**

These are the official message bridges, deployed respectively by the zkSync Team and Linea Team, that are responsible for facilitating message transmission between the Layer2 and Ethereum.

### zkLink Sequencer

The zkLink Sequencer is responsible for executing transactions, submitting blocks, and zk proofs. During the multi-chain state synchronization phase, it is in charge of invoking contracts on different chains to drive state synchronization.

### Sync Process

The process of multi-chain state synchronization is divided into two steps:

1. **Sending Sync Hash:** The zkLink Sequencer triggers the zkLink main contracts on both zkSync Era and Linea to dispatch their respective chain's sync hash to the Arbitrator.
2. **Confirming Blocks:** After receiving the sync hashes from the different chains, the zkLink Sequencer uses the Arbitrator's ConfirmBlock method to perform consistency verification of the sync hashes. If this verification is successful, the Arbitrator transmits a confirmation message via the L1 gateway to the zkLink contracts on the various zk Rollups.



{% hint style="info" %}
The table below is the Nexus Beta Network Information.
{% endhint %}

| Deployment Chain   | Module Name           | Contract Address                           |
| ------------------ | --------------------- | ------------------------------------------ |
| Ethereum Mainnet   | Arbitrator            | 0x683669E5B6cDc6636673a5f7ddB68E20812216F5 |
| Ethereum Mainnet   | Linea L1 Gateway      | 0xaD5d729291C0d6A299E370814CA6Ce1c8C25b51c |
| Ethereum Mainnet   | zkSync Era L1 Gateway | 0x98CEDA04E4a1FDc0fd025FB73e48e609AD00673B |
| Linea Mainnet      | zkLink Main Contract  | 0xdE1Ce751405Fe6D836349226EEdCDFFE1C3BE269 |
| Linea Mainnet      | Linea L2 Gateway      | 0xb6B96964633F558980e454953474cc7435c3D78B |
| zkSync Era Mainnet | zkLink Main Contract  | 0x0669ef7718376591AE0756A56255c75D2e712d87 |
| zkSync Era Mainnet | zkSync L2 Gateway     | 0xF180DD47ad7681335c82a592EA62Fdb92446F300 |

