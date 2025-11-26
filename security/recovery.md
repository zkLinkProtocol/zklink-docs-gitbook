# zkLink Asset Emergency Withdrawal Guide in Case of DApp Shutdown / Exit Scam

**Applicable Scenarios:**
*   **Premise A:** The frontend website of an exchange or DApp integrated with zkLink (such as ApeX Pro, ZKEX, etc.) is inaccessible, and/or the team has disappeared or ceased operations.
*   **Premise B:** The zkLink X network (infrastructure layer) is still producing blocks normally, and the official explorer and core contracts are functioning as usual.
*   **Goal:** Bypass the DApp’s frontend and interact directly with the zkLink protocol to safely withdraw your assets back to your Layer 1 wallet.

## 1. How It Works: Why Are Your Assets Still Safe?

zkLink X is an application-centric rollup infrastructure.
*   **Custody of Funds:** Your funds are not stored on ApeX’s company servers. Instead, they are locked in **zkLink’s smart contracts on Layer1 (such as Ethereum, BNB Chain)**.
*   **Account Mapping:** The DApp (e.g., ApeX) is essentially a “frontend interface” and “matching engine” used to submit instructions (place orders, trade, etc.).
*   **Censorship Resistance:** Even if a DApp wanted to withhold your funds, as long as the zkLink protocol is still running, you can use the Layer1 **forced withdrawal** (Forced Withdrawal / Full Exit) mechanism to require the zkLink sequencer to process your withdrawal request.

---

## 2. Step-by-Step Guide: Two-Stage Strategy

### Strategy 1: Use zkLink’s Official General Interface (Easiest Option)

If it’s only the ApeX domain or frontend that’s down, zkLink will usually provide a generic explorer or asset portal.

1.  **Visit the zkLink Explorer (blockchain explorer):**
    *   Go to the official zkLink explorer (e.g., [scan.zk.link](https://scan.zk.link)).
    *   Search by entering your wallet address.
2.  **Check Your L2 Balance:**
    *   Confirm that your account balance on the zkLink network is displayed correctly.
3.  **Look for a General Withdrawal Interface:**
    *   Some zkLink ecosystem explorers or official bridge pages allow you to connect your wallet and initiate a **Withdraw** operation for specific tokens.
    *   *Note: If ApeX has configured special withdrawal restrictions, this simple method may fail, and you will need to proceed to Strategy 2.*

---

### Strategy 2: Layer1 Forced Withdrawal (Forced Withdrawal / Full Exit)

This is the core solution. Since you can’t click the “Withdraw” button on the DApp frontend, you will interact directly with the Layer1 contract to “slam the table” and force zkLink to process your request.

#### Preparation
*   **L1 Wallet:** A wallet that controls the private key of your account (e.g., MetaMask).
*   **Gas Fees:** Sufficient ETH (or the native token of the relevant L1) on Layer1 (e.g., Ethereum mainnet) to pay for gas.
*   **Key Parameters:** You need to know your `AccountId` and `SubAccountId` within the zkLink system (for ApeX, the main account is usually SubAccount 1 or 0).

#### Step 1: Obtain Your Account Information

Since the DApp is down, you will need to use the zkLink explorer to look up your account ID.

1.  Enter your 0x address in zkLink Explorer.
2.  Note down your **Account ID** (for example: 1054) and the **Sub Account ID** where your assets are stored (for example: 1).
3.  Confirm the **Token ID** of the asset you want to withdraw (for example: USDC might be ID 1; you need to check the mapping table).

#### Step 2: Call the Layer1 Smart Contract

You will need to call the `requestFullExit` function on zkLink’s main contract on Layer1.

1.  **Find the Contract Address:**
    *   Visit the zkLink official documentation or GitHub to find the `ZkLink Proxy` contract address for the relevant network (e.g., Ethereum Mainnet).
    *   *Tip: Always verify that the address comes from an official source to avoid phishing.*
2.  **Interact via Etherscan (or the explorer for that chain):**
    *   Open the contract address page, then click **“Contract” -> “Write Proxy”** (if it’s using a proxy pattern).
    *   Connect your wallet (Connect to Web3).
3.  **Fill in the `requestFullExit` function parameters:**
    *   `_accountId`: Enter the Account ID obtained in Step 1.
    *   `_subAccountId`: Enter the Sub Account ID.
    *   `_tokenId`: Enter the Token ID of the asset you want to withdraw.
    *   `_mapping`: (Usually set to `false`, unless there is a special mapping logic involved; this depends on the specific contract version.)
4.  **Send the Transaction:**
    *   Click “Write,” then confirm the transaction in your wallet and pay the gas fee.

#### Step 3: Wait for the zkLink Sequencer to Process

This is a transaction sent on L1 that the zkLink sequencer will listen for.

*   **Mechanism:** Under the zkLink protocol rules, the sequencer **must** include this request in a block and execute it on L2 within a specific time window (the Priority Queue).
*   **Result:** Your L2 assets will be burned, and an equivalent amount of funds will be unlocked from the L1 pool and sent to your L1 wallet address.

---

## 3. What If zkLink Refuses to Process It?

If ApeX has exited, and zkLink is still “alive” but does not process your `requestFullExit` request for an extended period (for example, more than 3–5 days), this escalates into a **“zkLink outage / malicious behavior”** scenario.

At that point, the system will automatically trigger **Exodus Mode**:
1.  The L1 contract detects that requests in the priority queue have expired.
2.  The contract is frozen and will no longer accept new L2 state updates.
3.  **At this point, you should switch to running `recover_state_server` locally** (the solution described in the previous document) to generate a Merkle proof and withdraw directly from the contract.

## 4. Summary & Recommendations

1.  **Don’t panic:** As long as zkLink is still producing blocks, ApeX’s frontend is just a “skin.” As long as you control your private key, you control your assets.
2.  **Preferred Strategy:** Try the L1 forced withdrawal (`requestFullExit`) first. This usually does not require deep technical knowledge and can be done directly through Etherscan.
3.  **Last Resort:** Only resort to Merkle-proof–based self-rescue methods when L1 forced withdrawal requests are ignored.

*Disclaimer: This process involves interacting directly with smart contracts. Always test with a small amount first, and carefully verify contract addresses and parameter descriptions against the official documentation before proceeding.*