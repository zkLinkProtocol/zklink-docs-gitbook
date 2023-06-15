---
sidebar_position: 6
title: ' '

---

# Wallet Integration & AA Wallet

---
> 💡Account abstraction unifies contract accounts and EOAs. Integrating AA wallets brings better user experience to dApps built on top of zkLink. Smart contract wallets are much more flexible than EOAs, allowing users to control their accounts by smart contract rather than simply a private key, and to define their account logic such as permission controls, transaction limits, wallet recovery, etc,.

> Taking UniPass as an example, UniPass offers a user-friendly experience for managing smart contract wallets using email and password. We believe this is crucial for users who are not yet familiar with crypto.

## Required Conditions

- supports EIP712 signing
- supports generation of zkLink_key
    - zkLink_key must be generated and kept secret by user clients;
    - zkLink_key is the private key to generation EdDSA signature, and every single Layer2 transactions needs to be signed by zkLink_key;
    - In the example of Unipass integration, a zkLink_key is generated when the user signs from the browser client with the [client slice of MPC master key](https://docs.wallet.unipass.id/architecture/master-key) (`user key`). This zkLink_key will not be exported to Unipass server.



## Optional Condition

- compatible to proxy deposit contract
    - we suggest AA wallets to support zkLink proxy deposit to improve UX in many cases;
    - AA wallets that adopt the zkLink auto transfer function can [directly call transfer to AA wallet](/docs/streamline/deposit#example-1).


![AA wallet flow](../../static/img/tech/AA_flow.png)

## AA Wallet Security

AA wallets integrating to zkLink must meet Dunkirk Test requirement, i.e., assets security is not dependent to agency, and users can still retrieve assets even when the wallet service provider goes down.

In the [case study of Unipass](https://docs.wallet.unipass.id/architecture/email-on-chain-verification), users can download an open-source front-end or use a web form with IPFS hosting to recover their account and migrate to EOAs if the Unipass server fails.