# Quick Start
In this quick start, you'll lean how to create a DEX and take a short tour through the zkLink sdk code.
The following topics are intended to be read in order as a fast path to learning to create a simple DEX.
Before reading the following chapters, it is strongly recommended to read the overview first to understand the architecture of zkLink.
And in order to get the notification from zkLink, you need to make [Websocket](../api-and-sdk/websocket) prepared. 

Here we assume there are two trader Alic and Bob:

* Alice and Bob deposit and create Account
* Alice use USDT to buy the BTC from Bob
* Alice withdraw the half amount of BTC she just brought
* Alice transfer the left BTC to another subaccount

The function of DEX are:

* 
* handle the withdraw request from Alice
* create order list
* make Alice and Bob's order matched and build transaction
