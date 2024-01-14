# Detailed Explanation of Nexus Multi-Chain State Synchronization

In the zkLink Nexus mode, the synchronization of multi-chain states is carried out through the transmission of sync hashes. This transmission is executed by the official message bridge(canonical message service) deployed by layer2 team. Once the sync hashes from different chains have reached the Ethereum mainnet, a consistency check is then performed. After passing the check, the confirmation information is sent back to the Layer2 network using the official message bridge.

<figure><img src="../../../.gitbook/assets/photo_2024-01-14 15.01.23.jpeg" alt=""><figcaption><p>Nexus Multi-Chain State Synchronization</p></figcaption></figure>

{% hint style="warning" %}
For simplicity and ease of understanding, the above diagram uses only two zkRollup Layer2s as examples and does not represent the actual network deployment scenario.
{% endhint %}

### Contracts deployed by zkLink Protocol

In the image above, the contracts deployed by the zkLink Protocol include:

* **zkLink Main:** The main contract of zkLink, which includes user deposit, zk verify, and sync hash functions.
* **Linea L1 Gateway, zkSync Era L1 Gateway :** Gateway contracts deployed on the Ethereum Mainnet are responsible for comparing the sync hash and transmitting the results to the zkLink Main Contract deployed on Layer2 through the official message bridge.
* **Linea L2 Gateway, zkSync Era L2 Gateway:** Gateway contracts deployed on the Layer2 Network are responsible for sending the sync hash to the Arbitrator contract deployed on Ethereum through the official message bridge.
* **Arbitrator:** The arbitrator contract receives and stores the sync hashes of zkLink from various zk Layer2 Networks and then performs consistency verification. And then transmits the comparison results to the zkLink Main contract on each Layer2 Network using the `confirmBlock` method.

### Contracts deployed by Layer2 Network

* **Linea Canonical Message Service**
* **zkSync Era Canonical Message Service**

In the above image, the **zkSync Canonical Message Service** and **Linea Canonical Message Service** are the official message bridges deployed by zkSync Team and Linea Team, respectively, responsible for message transmission between Layer2 and Ethereum.

### zkLink Sequencer

The zkLink Sequencer is responsible for executing transactions, submitting blocks, and zk proofs. During the multi-chain state synchronization phase, it is in charge of invoking contracts on different chains to drive state synchronization.

### Sync Process

The multi-chain state synchronization is divided into two steps:

1. Send sync hash: The zkLink Sequencer calls the zkLink main contracts on zkSync Era and Linea, respectively, to send their respective chain's sync hash to the **Arbitrator**.
2. Confirm block: After the **Arbitrator** receives the Sync hashes from different chains, the zkLink Sequencer invokes the Arbitrator's `ConfirmBlock` method to perform the consistency verification of the sync hashes. If the consistency verification is successful, the Arbitrator will send the confirmation message through the L1 gateway to the zkLink contracts on various zk Rollups.



{% hint style="info" %}
The table below is the Nexus Beta Network Information.
{% endhint %}

| Deployment Chain   | Module Name           | Contract Address                           |
| ------------------ | --------------------- | ------------------------------------------ |
| Ethereum Mainnet   | Arbitrator            | 0x683669E5B6cDc6636673a5f7ddB68E20812216F5 |
| Ethereum Mainnet   | Linea L1 Gateway      | 0xaD5d729291C0d6A299E370814CA6Ce1c8C25b51c |
| Ethereum Mainnet   | zkSync Era L1 Gateway | 0x98CEDA04E4a1FDc0fd025FB73e48e609AD00673B |
| Linea Mainnet      | zkLink Main           | 0xdE1Ce751405Fe6D836349226EEdCDFFE1C3BE269 |
| Linea Mainnet      | Linea L2 Gateway      | 0xb6B96964633F558980e454953474cc7435c3D78B |
| zkSync Era Mainnet | zkLink Main           | 0x0669ef7718376591AE0756A56255c75D2e712d87 |
| zkSync Era Mainnet | zkSync L2 Gateway     | 0xF180DD47ad7681335c82a592EA62Fdb92446F300 |

