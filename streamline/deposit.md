---
sidebar_position: 6
title: ' '

---

# Deposit

---
zkLink supports account abstraction (AA) wallets and externally owned account (EOA) wallets, with either EVM addresses (20 bytes) or nonEVM addresses (currently Starknet address only, with 32 bytes).

---
**Users can deposit to zkLink from the following address:**

1. EOA wallet on EVM
2. EOA wallet on nonEVM
3. Abstraction wallet on EVM
4. Abstraction wallet on nonEVM
5. CEX address on EVM (custodial, and usually only supports standard transfer contract calls)
6. CEX address on nonEVM (custodial, and usually only supports standard transfer contract calls)

---
**The scenarios of receiver address on zkLink are:**

1. EOA on EVM
2. EOA on nonEVM
3. AA wallet on EVM
4. AA wallet on nonEVM

---
**Thus the accordingly call functions are as followed:**

<span className="highlight">A: directly call zkLink deposit function</span>

Note: supports both AA wallets and EOA wallets; it is required that the from_address can directly sign zkLink deposit transactions.

<span className="highlight">B: call transfer to user AA wallet</span>

Note: supports AA wallets that adopt the zkLink auto transfer function only, such as UniPass.

<span className="highlight">C: call transfer to proxy deposit contract, and then automatically deposit to zkLink contract</span>

Example scenarios:
1. The from_address is not authorized to define the signature message. For example, a CEX user intends to withdraw directly from Binance to ZKEX.
2. The address format of the to_address on zkLink and the from_address is different. For example, a user intends to deposit from a Starknet address to a EVM compatible AA wallet address.

![Proxy Deposit Flow](../../static/img/tech/proxy_deposit_flow.png)

---
## Exhaustive Scenarios

<table>
    <tr>
        <td></td><td></td><td colspan="4" align="center">Receiver Address</td>
    </tr>  
    <tr>
        <th></th><th></th><th align="center">AA Wallet - EVM (20 bytes)</th><th align="center">AA Wallet - nonEVM (Starknet 32 bytes)</th><th align="center">EOA EVM (20 bytes)</th><th align="center">EOA nonEVM (32 bytes)</th>
    </tr>
    <tr>
        <td rowspan="6">Sender Address</td><td align="center">EOA - EVM</td><td align="center">ABC</td><td align="center">AC</td><td align="center">AC</td><td align="center">AC</td>
    </tr>
    <tr>
        <td align="center">EOA - nonEVM</td><td align="center">AC</td><td align="center">ABC</td><td align="center">AC</td><td align="center">AC</td>
    </tr>
    <tr>
        <td align="center">owned AA Wallet - EVM</td><td align="center">ABC</td><td align="center">AC</td><td align="center">AC</td><td align="center">AC</td>
    </tr>
    <tr>
        <td align="center">owned AA Wallet - nonEVM</td><td align="center">AC</td><td align="center">ABC</td><td align="center">AC</td><td align="center">AC</td>
    </tr>
    <tr>
        <td align="center">CEX - EVM</td><td align="center">BC</td><td align="center">C</td><td align="center">C</td><td align="center">C</td>
    </tr>
    <tr>
        <td align="center">CEX - nonEVM</td><td align="center">C</td><td align="center">BC</td><td align="center">C</td><td align="center">C</td>
    </tr>
</table>


---
Below are 2 typical operational processes using [ZKEX.com](http://zkex.com/) as an example (the first dApp using zkLink infrastructure):

## Example 1

A Unipass AA wallet user intends to deposit a Token (ABC) deployed on Ethereum from a CEX (such as Binance):

Step 1: On the zkex deposit page, locate ABC with the deposit address (0xabc...);

Step 2: Copy the address to the Binance withdrawal page and await for approval from Binance; 

Step 3: Wait for confirmation from zkex.

Note:

1. Since the Unipass AA wallet is compatible with proxy deposit and is deployed on the EVM (the same network with ABC), the deposit address copied to Binance is actually the Unipass AA wallet address.
2. If the token ABC is deployed on StarkNet, which differs from the network of the user's AA wallet, then a proxy deposit contract address would be generated on Starknet to assist the ABC deposit.
3. Why not directly call deposit on zkLink? This is because in many cases, the withdrawal address does not support user-defined contract calls (zklink deposit function), especially on CEXs, who generally only support the most basic transfer calls.

## Example 2

A MetaMask user intends to deposit the token ABC on EVM from its MetaMask address:

Step 1: On the zkex deposit page, locate ABC and select "Personal Wallet" as the fund source (it may vary if it is a deposit from CEX). Enter the amount, sign in MetaMask, and call zkLink deposit function;

Step 2: Only wait for the deposit transaction to be submitted and confirmed on-chain (it required 36 block on Ethereum).

Note:

1. A user may deposit to ZKEX from another MetaMask address. In this case, ZKEX can provide a separate deposit page to assist in signing the zkLink deposit with a third address.
2. If the token is deployed on a non EVM-compatible network, the deposit will need to go through a proxy deposit contract.