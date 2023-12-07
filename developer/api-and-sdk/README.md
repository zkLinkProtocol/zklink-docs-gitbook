# API & SDK
## Change Log

**2023-12-07**
* Initialize SDK documentation

## Introduction
Welcome to the zkLink SDK documentation. This document is a guide to help developers access the services offered by zkLink protocol via SDK.

[zkLink sdk](https://github.com/zkLinkProtocol/zklink_sdk) now support all the functions in Golang, Javascript and Rust. You can use it to generate private key, build the transaction and generate signature. For the developer who use the other language please refer to the [Private Key and Signature](./private-key-and-signature) part.

## Json Rpc and Websocket
Developers can send all the layer3 transactions to the zkLink and get the associated information from zkLink. zkLink also push the transaction [state update](./data-types/state-update.md) via websocket to dApp.

## Private Key and Signature
After user create an account in zkLink, a layer3 private key need to be generated from the L1 wallet private key. User operations such as placing order, withdraw, transfer asset etc require this L3 private key to generate the signature. And zkLink server will verify the signature of the received transaction. This part will describe how to generate the L3 private key and encode the transaction to be signed.
For the developers who use the SDK can ignore this part.

## Data Types
This part include all the transaction types and state update definition including the sdk example to sign the transactions.

