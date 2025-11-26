# Layer1 & Zero‑Knowledge Security Guarantees

This page explains how zkLink X leverages Layer1 settlement and zero‑knowledge proofs  
to provide **mathematical** security for user funds, independent of any operator or team.

---

## Layer1: Custody and Final Settlement

When using zkLink X:

- **Deposits** move assets from user wallets into zkLink’s smart contracts on Layer1  
  (e.g. Ethereum, or other supported chains).
- These contracts:
    - Hold user funds under the rules of immutable, audited code;
    - Are protected by the consensus of the underlying blockchain;
    - Do not give any project team unilateral control over balances.

All critical actions—such as finalizing rollup batches and releasing withdrawals—  
are ultimately authorized by Layer1 consensus and contract logic, not by a trusted committee.

---

## Zero‑Knowledge Validity Proofs

zkLink X is a **validity rollup** (zkRollup):  
every state transition must be accompanied by a zero‑knowledge proof (ZKP) that:

- The batch of transactions follows the protocol rules;
- No balance goes negative;
- No tokens are created from nothing;
- All per‑account and global invariants are respected.

Only if the proof is successfully verified by the Layer1 contract:

- The new state root is accepted and stored on‑chain;
- Subsequent withdrawals can reference this root as the source of truth.

This gives **cryptographic assurance** that:

- If you see a balance recorded in the accepted state root,
- Then, mathematically, that balance must be the result of a valid execution trace.

---

## Math vs. Trust: Why This Matters

Many bridge or sidechain designs rely on:

- A small multi‑sig or validator set signing withdrawals;
- Social trust that the operators will not collude.

In those models:

- If enough validators are compromised or collude,
- They can sign arbitrary withdrawals and drain shared liquidity pools.

zkLink X instead relies on:

- **Public smart contracts** enforcing strict rules;
- **Zero‑knowledge proofs** checked on Layer1.

No group of operators can “vote” to bypass these rules.  
They cannot fabricate a valid proof for an invalid state without breaking  
the underlying cryptography, which is assumed to be computationally infeasible.

In short:

- Funds are not safe because a team promises to behave well;
- Funds are safe because **they cannot be moved without satisfying the math.**

---

## What This Does and Does Not Protect

zkLink X’s Layer1 + ZK security model protects against:

- Operator misbehavior or collusion;
- Invalid state updates or hidden balance manipulation;
- Theft of pooled funds by compromising a small validator set.

It does **not** protect against:

- Users losing control of their own private keys;
- Signing malicious transactions to phishing contracts or fake UIs;
- Vulnerabilities in assets themselves (e.g. buggy ERC‑20 tokens).

---

## Key Takeaways

- zkLink X inherits the security of the underlying Layer1 (e.g. Ethereum).
- All state transitions are validated by zero‑knowledge proofs on‑chain.
- No centralized party can bypass the Layer1 contracts or the ZK proofs  
  to arbitrarily move user funds.
- Ultimately, **your assets are secured by public code and mathematics, not by trust.**