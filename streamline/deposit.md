# Deposit

zkLink supports account abstraction (AA) wallets and externally owned account (EOA) wallets, with either EVM addresses (20 bytes) or nonEVM addresses (currently Starknet address only, with 32 bytes).

### **Users can deposit to zkLink from the following address:**

1. EOA wallet on EVM
2. EOA wallet on nonEVM
3. Abstraction wallet on EVM
4. Abstraction wallet on nonEVM
5. CEX address on EVM (custodial, and usually only supports standard transfer contract calls)
6. CEX address on nonEVM (custodial, and usually only supports standard transfer contract calls)

### **The scenarios of receiver address on zkLink are:**

1. EOA on EVM
2. EOA on nonEVM
3. AA wallet on EVM
4. AA wallet on nonEVM

## **Deposit Functions**

### **The accordingly call functions are as followed:**

`A: directly call zkLink deposit function`

Note: supports both AA wallets and EOA wallets; it is required that the from\_address can directly sign zkLink deposit transactions.

`B: call transfer to user AA wallet`

Note: supports AA wallets that adopt the zkLink auto transfer function only, such as UniPass.

`C: call transfer to proxy deposit contract, and then automatically deposit to zkLink contract`

Example scenarios:

1. The from\_address is not authorized to define the signature message. For example, a CEX user intends to withdraw directly from Binance to ZKEX.
2. The address format of the to\_address on zkLink and the from\_address is different. For example, a user intends to deposit from a Starknet address to a EVM compatible AA wallet address.

![Proxy Deposit Flow](../img/proxy\_deposit\_flow.png)

## Exhaustive Scenarios

<table data-header-hidden><thead><tr><th width="256"></th><th width="113"></th><th width="149" align="center"></th><th width="100"></th><th></th></tr></thead><tbody><tr><td></td><td>AA Wallet - EVM (20 bytes)</td><td align="center">AA Wallet - nonEVM (Starknet 32 bytes)</td><td>EOA EVM (20 bytes)</td><td>EOA nonEVM (32 bytes)</td></tr><tr><td>EOA - EVM</td><td>ABC</td><td align="center">AC</td><td>AC</td><td>AC</td></tr><tr><td>EOA - nonEVM</td><td>AC</td><td align="center">ABC</td><td>AC</td><td>AC</td></tr><tr><td>owned AA Wallet - EVM</td><td>ABC</td><td align="center">AC</td><td>AC</td><td>AC</td></tr><tr><td>owned AA Wallet - nonEVM</td><td>AC</td><td align="center">ABC</td><td>AC</td><td>AC</td></tr><tr><td>CEX - EVM</td><td>BC</td><td align="center">C</td><td>C</td><td>C</td></tr><tr><td>CEX - nonEVM</td><td>C</td><td align="center">BC</td><td>C</td><td>C</td></tr></tbody></table>

Below are 2 typical operational processes using [ZKEX.com](http://zkex.com/) as an example (the first dApp using zkLink infrastructure):

### Example 1

A Unipass AA wallet user intends to deposit a Token (ABC) deployed on Ethereum from a CEX (such as Binance):

Step 1: On the zkex deposit page, locate ABC with the deposit address (0xabc...);

Step 2: Copy the address to the Binance withdrawal page and await for approval from Binance;

Step 3: Wait for confirmation from zkex.

{% hint style="info" %}
1. Since the Unipass AA wallet is compatible with proxy deposit and is deployed on the EVM (the same network with ABC), the deposit address copied to Binance is actually the Unipass AA wallet address.
2. If the token ABC is deployed on StarkNet, which differs from the network of the user's AA wallet, then a proxy deposit contract address would be generated on Starknet to assist the ABC deposit.
3. Why not directly call deposit on zkLink? This is because in many cases, the withdrawal address does not support user-defined contract calls (zklink deposit function), especially on CEXs, who generally only support the most basic transfer calls.
{% endhint %}

### Example 2

A MetaMask user intends to deposit the token ABC on EVM from its MetaMask address:

Step 1: On the zkex deposit page, locate ABC and select "Personal Wallet" as the fund source (it may vary if it is a deposit from CEX). Enter the amount, sign in MetaMask, and call zkLink deposit function;

Step 2: Only wait for the deposit transaction to be submitted and confirmed on-chain (it required 36 block on Ethereum).

{% hint style="info" %}
1. A user may deposit to ZKEX from another MetaMask address. In this case, ZKEX can provide a separate deposit page to assist in signing the zkLink deposit with a third address.
2. If the token is deployed on a non EVM-compatible network, the deposit will need to go through a proxy deposit contract.
{% endhint %}

