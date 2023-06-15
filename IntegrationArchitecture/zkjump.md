---
sidebar_position: 6
title: ' '
---

# Lightweight Integration

> 💡 A case study of zkJump.io to showcase lightweight integration

![zkJump Integration](../img/zkjump\_integration.png)

## zkJump Server

zkJump Server monitors related on-chain events, offers broker services for cross-chain transfers, and provides backend service for zkJump.io.

In practice, the zkJump Server does not need to maintain a complete set of user nonce or account balance information as [ZKEX.com](http://zkex.com) does. User balance can be directly fetched from zkLink L2 infra, reducing the development and operational costs of zkJump considerably.

The fee for `Proxy Withdraw Operation` is covered by zkJump operators.

## zkJump Bridge Contract

The contract is developed and maintained by zkJump, with the following functions:

* Collect cross-chain transfer fees
* Integrate to on-chain DEXs. For example, tokens swapped in Uniswap could be directly transferred to the destined chain via zkJump integrated on Uniswap

