# Security Framework of zkLink X

From the perspective of fund security, zkLink X splits the system into three layers:  
**Application Layer (Matching / DEX) → Protocol Layer (zkLink X Layer2) → Settlement Layer (Layer1 + ZK Proofs)**.  
Even if an upper layer fails, as long as the lower layers remain secure, users can fully recover their funds.

---

## 1. Application Layer: Exchanges, External Matching / Frontend Downtime or Exit Scam ([Guide](refund.md))

This layer includes all applications built on zkLink X, such as ApeX Omni, ZkEX and other DEXs. They are responsible for:

- Providing the trading interface, order placement, and matching;
- Calling zkLink X APIs to execute trades on Layer2.

**Key point: the application layer does not custody funds.**

- After a user deposits, assets go directly into the zkLink contract on Layer1, not into any project’s own wallet.
- If an application’s frontend goes offline, the matching engine stops, or the team disappears:
    - Trading experience is interrupted;
    - But funds remain safely locked in the zkLink contract.
- Users can:
    - Connect to the same zkLink X account via the official zkLink frontend or community frontends to initiate withdrawals;
    - Or directly call the Full Exit / Forced Withdrawal function of the zkLink contract on Layer1 to withdraw assets back to Layer1.

> In other words: **a single application can fail, but it cannot “take away” user funds on zkLink X.**

---

## 2. Protocol Layer: zkLink X Layer2 Downtime or Team Disappearance ([Guide](recovery.md))

This layer is the zkLink X Rollup protocol and its infrastructure, including:

- The Sequencer, Prover, official APIs / frontend, etc.

**Even if zkLink X infrastructure goes down or the team disappears, funds can still be recovered:**

- All historical state updates (state roots, batch data) have already been posted on Layer1;
- When the Sequencer fails to process the priority queue for an extended period or stops producing blocks, the on-chain contract will enter **Exodus Mode (Emergency Exit Mode)**:
    - Freeze new Layer2 state updates;
    - Open a “permissionless exit” channel.
- In Exodus Mode, users or the community can use open-source tools (such as `recover_state_server`) to:
    1. Read historical data submitted by zkLink from Layer1 and **replay transactions locally to reconstruct the final state tree**;
    2. Generate the corresponding **Merkle proof** for each account;
    3. Directly call the Layer1 contract, submit the Merkle proof, and **forcibly withdraw assets from the zkLink contract**.

> Even if all official zkLink services are no longer maintained, as long as the underlying L1 chain is still running, users can rely on public data + mathematical proofs to complete “self-serve asset evacuation.”

---

## 3. Settlement Layer: Layer1 and the Mathematical Security of Zero-Knowledge Proofs ([Audits](../reference/Audits.md))

The lowest layer is the public-chain settlement layer (such as Ethereum) and the zkLink smart contracts and zero-knowledge proof system deployed on it, which together provide the **ultimate guarantee of fund security**:

- **Layer1 Custody of Funds**
    - After users deposit, funds are locked in zkLink’s Layer1 contract and protected by the L1 consensus;
    - Any asset movement must be executed via contract logic and cannot be bypassed by any single party.

- **Zero-Knowledge Validity Proofs (ZK Validity Proofs)**
    - Every state batch is accompanied by a ZK proof that is verified on Layer1;
    - Only state roots whose proofs pass verification can be written into the contract, **preventing forged balances, arbitrary minting, or incorrect settlements**.

- **Security from Mathematics, Not Trust**
    - Does not rely on a small multisig or a small validator set “voting” on fund ownership;
    - Fund security depends on public contract code and verifiable cryptographic proofs, rather than trust in any particular team or institution.

> Therefore, as long as Layer1 remains secure, the ultimate safety of zkLink X user funds is guaranteed by mathematics and cryptography, not by any centralized party’s credibility.

---

From top to bottom, the relationship between the three layers can be summarized as:

1. **The application layer can fail** — it affects the user experience, but not custody of funds;
2. **The protocol layer can be bypassed in extreme cases** — via Exodus Mode + Merkle proofs, users can exit directly from the contract;
3. **The settlement layer is secured by Layer1 + ZK mathematics** — no one can “lie” or move funds out of thin air at the mathematical level.