---
description: >-
  zkLink sdk now support all the functions in Golang, Javascript and Rust. You
  can use it to generate private key, build the transaction and generate
  signature. For the developer who use the other langu
---

# Transactions

## Private Key and Signature

After user create an account in zkLink, a layer3 private key need to be generated from the L1 wallet private key. User operations such as placing order, withdraw, transfer asset etc require this L3 private key to generate the signature. And zkLink server will verify the signature of the received transaction. This part will describe how to generate the L3 private key and encode the transaction to be signed. For the developers who use the SDK can ignore this part.

## Data Types

This part include all the transaction types and state update definition including the sdk example to sign the transactions.
