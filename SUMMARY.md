# Table of contents

## 🙂 Welcome

* [Introduction](README.md)

## ⚖ Protocol

* [Protocol Overview](welcome/Architecture.md)
* [Settlement Layer](protocol/settlement-layer/README.md)
  * [Working Principal of A Multi-Chain ZK-Rollup](protocol/settlement-layer/working-principal-of-a-multi-chain-zk-rollup.md)
  * [Nexus: Settlement on ETH ZK-L2s](protocol/settlement-layer/nexus-settlement-on-eth-zk-l2s.md)
  * [Origin: Settlement on ETH and Alt-L1s](protocol/settlement-layer/origin-settlement-on-eth-and-alt-l1s.md)
  * [Multi-Chain State Synchronization](protocol/settlement-layer/multi-chain-state-synchronization.md)
  * [Supported Networks of zkLink Nexus and Origin](protocol/settlement-layer/supported-networks-of-zklink-nexus-and-origin.md)
  * [Security Assumptions of zkLink Nexus and Origin](protocol/settlement-layer/security-assumptions-of-zklink-nexus-and-origin.md)
* [Execution Layer](protocol/execution-layer/README.md)
  * [TS-zkVM for App Rollup](protocol/execution-layer/ts-zkvm-for-app-rollup.md)
* [Sequencing Layer](protocol/sequencing-layer.md)
* [DA Layer](protocol/da-layer.md)

## 🛠 Developer

* [Developer Overview](developer/overview.md)
* [Get Started](developer/quick-start/README.md)
* [JSON RPC & Websocket](developer/json-rpc-and-websocket/README.md)
  * [JSON-RPC API](developer/api-and-sdk/json-rpc/json-rpc-api.md)
  * [JSON-RPC Errors](developer/api-and-sdk/json-rpc/json-rpc-errors.md)
  * [Websocket](developer/api-and-sdk/websocket/README.md)
* [Transactions](developer/api-and-sdk/README.md)
  * [Basic Types](developer/api-and-sdk/data-types/basic-types.md)
  * [State Update](developer/api-and-sdk/data-types/state-update.md)
  * [Transaction](developer/api-and-sdk/data-types/transaction/README.md)
    * [Deposit](developer/api-and-sdk/data-types/transaction/deposit.md)
    * [FullExit](developer/api-and-sdk/data-types/transaction/full\_exit.md)
    * [ChangePubKey](developer/api-and-sdk/data-types/transaction/change\_pubkey.md)
    * [Withdraw](developer/api-and-sdk/data-types/transaction/withdraw.md)
    * [ForcedExit](developer/api-and-sdk/data-types/transaction/forced\_exit.md)
    * [OrderMatching](developer/api-and-sdk/data-types/transaction/order\_matching.md)
    * [AutoDeleveraging](developer/api-and-sdk/data-types/transaction/auto\_deleveraging.md)
    * [ContractMatching](developer/api-and-sdk/data-types/transaction/contract\_matching.md)
    * [Funding](developer/api-and-sdk/data-types/transaction/funding.md)
    * [Liquidation](developer/api-and-sdk/data-types/transaction/liquidation.md)
    * [UpdateGlobalVar](developer/api-and-sdk/data-types/transaction/update\_global\_var.md)
  * [Private Key & Signature](developer/api-and-sdk/private-key-and-signature/private\_key.md)
    * [ChangePubKey](developer/api-and-sdk/private-key-and-signature/encode/chaneg\_pubkey.md)
    * [Withdraw](developer/api-and-sdk/private-key-and-signature/encode/withdraw.md)
    * [ForcedExit](developer/api-and-sdk/private-key-and-signature/encode/forced\_exit.md)
    * [OrderMatching](developer/api-and-sdk/private-key-and-signature/encode/order\_matching.md)
    * [ContractMatching](developer/api-and-sdk/private-key-and-signature/encode/contract\_matching.md)
    * [Funding](developer/api-and-sdk/private-key-and-signature/encode/funding.md)
    * [Liquidation](developer/api-and-sdk/private-key-and-signature/encode/liquidation.md)
    * [UpdateGlobalVar](developer/api-and-sdk/private-key-and-signature/encode/update\_global\_var.md)
* [SDK](developer/sdk/sdk.md)
  * [Go](developer/sdk/go/changelog.md)
    * [Types](developer/sdk/go/basic\_types.md)
    * [Signature](developer/sdk/go/signer.md)
    * [Transactions](developer/sdk/changelog/transactions/README.md)
      * [ChangePubKey](developer/sdk/go/transactions/1-change-pubkey.md)
      * [Withdraw](developer/sdk/go/transactions/2-withdraw.md)
      * [Transfer](developer/sdk/go/transactions/3-transfer.md)
      * [ForcedExit](developer/sdk/go/transactions/4-forced-exit.md)
      * [OrderMatching](developer/sdk/go/transactions/5-order-matching.md)
      * [ContractMatching](developer/sdk/go/transactions/6-contract-matching.md)
      * [AutoDeleveraging](developer/sdk/go/transactions/7-auto-deleveraging.md)
      * [Funding](developer/sdk/go/transactions/8-funding.md)
      * [Liquidation](developer/sdk/go/transactions/9-liquidation.md)
      * [UpdateGlobalVar](developer/sdk/go/transactions/10-update-global-var.md)
  * [Js](developer/sdk/js/changelog.md)

## ⚙ Network Information

* [Connected Networks](network-information/connected-networks/README.md)
  * [Mainnet](networks/mainnet\_networks.md)
  * [Testnet](networks/testnet\_networks.md)
* [DApps & Deployment Addresses](network-information/dapps-and-deployment-addresses/README.md)
  * [Mainnet](networks/mainnet\_addresses.md)
  * [Testnet](networks/testnet\_addresses.md)

## Wallet & User Fund Streamline

* [Withdraw](streamline/withdraw.md)
* [Wallet Integration & AA Wallet](streamline/wallet.md)
* [Deposit](streamline/deposit.md)

## Integration Cases

* [Heavyweight Integration (Multi-Chain Derivatives & Spot Exchange)](IntegrationArchitecture/Derivatives.md)
* [Simple Integration (Multi-Chain Spot Exchange)](IntegrationArchitecture/spot.md)

## Tokenomics

* [Tokenomics](tokenomics.md)

## Appendix

* [Audits](reference/Audits.md)
* [FAQ](appendix/faq.md)
* [glossary](appendix/glossary.md)
