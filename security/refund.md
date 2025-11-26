# Guide to Forced Withdrawal of Assets on zkLink When a DApp Shuts Down or Rugs

**Applicable scenarios:**
*   **Premise A:** The front-end website of an exchange or DApp integrated with zkLink (such as ApeX Pro, ZKEX, etc.) is inaccessible, and the team is unreachable or has ceased operations.
*   **Premise B:** The zkLink X network (infrastructure layer) is still producing blocks normally, and the official explorer and core contracts are functioning as usual.
*   **Goal:** Bypass the DApp’s front-end and interact directly with the zkLink protocol to safely withdraw your assets back to your Layer 1 wallet.

## 1. How It Works: Why Are Your Assets Still Safe?

zkLink X is an application-centric rollup infrastructure.
*   **Custody of funds:** Your funds are not stored on ApeX’s company servers; they are locked in **zkLink smart contracts on Layer 1 (such as Ethereum, BNB Chain)**.
*   **Account mapping:** The DApp (e.g., ApeX) is only a “front-end interface” and “matching engine” that issues commands (place orders, trade).
*   **Censorship resistance:** Even if the DApp tries to withhold your funds, as long as the zkLink protocol is still running, you can use the Layer 1 “forced exit” mechanism (Forced Withdrawal) to require the zkLink sequencer to process your withdrawal request.

---

## 2. Step-by-Step Guide: Two-Phase Strategy

### Strategy 1: Use zkLink’s Official General Access (Easiest)

If only ApeX’s domain or front-end is down, zkLink will usually provide a general explorer or asset portal.

1.  **Visit the zkLink Explorer (blockchain explorer):**
    *   Go to the official zkLink explorer (e.g., [scan.zk.link](https://scan.zk.link)).
    *   Enter your wallet address to search.
2.  **Check your L2 balance:**
    *   Confirm that your account balance on the zkLink network is displayed correctly.
3.  **Look for a generic withdrawal entry:**
    *   Some zkLink ecosystem explorers or official bridge interfaces allow you to connect your wallet and initiate a "Withdraw" operation for specific tokens.
    *   *Note: If ApeX has set withdrawal restrictions, this simple method may fail and you will need to move on to Strategy 2.*

---

### Strategy 2: Layer 1 Forced Withdrawal (Forced Withdrawal / Full Exit)

This is the core solution. Since the DApp’s front-end no longer lets you click the “Withdraw” button, you’ll go directly to the Layer 1 contract to “slam the table” and force zkLink to process your request.

#### Preparation
*   **L1 wallet:** A wallet that controls the private key for the account (e.g., MetaMask).
*   **Gas fees:** Sufficient ETH (or native token) on Layer 1 (e.g., Ethereum mainnet) to pay gas.
*   **Key parameters:** You need to know your `AccountId` and `SubAccountId` in the zkLink system (for ApeX, the main account is usually SubAccount 1 or 0).

#### Step 1: Obtain Your Account Information
Since the DApp is down, you need to use the blockchain explorer to look up your account ID.
1.  Enter your 0x address in the zkLink Explorer.
2.  Record your **Account ID** (e.g., 1054) and the **Sub Account ID** that holds your assets (e.g., 1).
3.  Confirm the **Token ID** you want to withdraw (for example, USDC’s ID might be 1; you’ll need to check the mapping table).

#### Step 2: Call the Layer 1 Smart Contract
You’ll need to call the zkLink main contract function on Layer 1: `requestFullExit`.

1.  **Find the contract address:**
    *   Go to the official zkLink documentation or GitHub and find the `ZkLink Proxy` contract address for the relevant network (such as Ethereum Mainnet).
    *   *Tip: Always verify that the address comes from an official source to avoid phishing.*
2.  **Interact via Etherscan (or the explorer of the corresponding chain):**
    *   Open the contract address page and click **"Contract" -> "Write Proxy"** (if it uses a proxy contract pattern).
    *   Connect your wallet (Connect to Web3).
3.  **Fill in the `requestFullExit` function parameters:**
    *   `_accountId`: Enter the ID obtained in Step 1.
    *   `_subAccountId`: Enter the sub-account ID.
    *   `_tokenId`: Enter the ID of the token you want to withdraw.
    *   `_mapping`: (Typically set to `false`, unless special mapping logic is involved; this depends on the specific contract version).
4.  **Send the transaction:**
    *   Click "Write", then confirm the transaction in your wallet and pay the gas fee.

#### Step 3: Wait for the zkLink Sequencer to Process
This is a transaction sent to Layer 1. The zkLink sequencer will listen for this event.
*   **Mechanism:** According to zkLink protocol rules, the sequencer **must** include this request in the priority queue and execute it on L2 within a specified time window.
*   **Result:** Your L2 assets will be burned, and an equivalent amount of funds will be unlocked from the L1 pool and sent to your L1 wallet address.

---

## 3. What If zkLink Refuses to Process It?

If ApeX has rug-pulled, and although zkLink is still “alive”, it fails to process the `requestFullExit` you submitted in Strategy 2 for a prolonged period (for example, more than 3–5 days), the situation escalates to a **“zkLink downtime / malicious behavior”** scenario.

At this point, the system will automatically enter **Exodus Mode**:
1.  The L1 contract detects that requests in the priority queue have expired.
2.  The contract freezes and stops accepting new L2 state updates.
3.  **You must then run `recover_state_server` locally** (as described in the previous document), generate a Merkle proof, and withdraw directly from the contract.

## 4. Summary and Recommendations

1.  **Do not panic:** As long as zkLink is still producing blocks, ApeX’s front-end is just a “skin”. As long as you hold your private key, you control your assets.
2.  **Preferred strategy:** First attempt a Layer 1 forced withdrawal (`requestFullExit`). This usually does not require deep technical expertise and can be done via Etherscan.
3.  **Last resort:** Only when Layer 1 forced withdrawals are ignored do you need to resort to the more complex self-rescue method using Merkle proofs.

*Disclaimer: This process involves interacting directly with smart contracts. Always test with a small amount first and carefully verify contract addresses and parameter descriptions against the official documentation before proceeding.*