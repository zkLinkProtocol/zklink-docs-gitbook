# Table of contents

## 🙂 Welcome

* [Introduction](README.md)

## ⚖️ Architecture

* [Overview](architecture/architecture.md)
* [Settlement Layer](architecture/settlement-layer/README.md)
  * [Working Principal of A Multi-Chain ZK-Rollup](architecture/settlement-layer/working-principal-of-a-multi-chain-zk-rollup.md)
  * [Nexus: Settlement on ETH L2s](architecture/settlement-layer/nexus-settlement-on-eth-l2s.md)
  * [Origin: Settlement on ETH and Alt-L1s](architecture/settlement-layer/origin-settlement-on-eth-and-alt-l1s.md)
  * [Multi-Chain State Synchronization](architecture/settlement-layer/multi-chain-state-synchronization/README.md)
    * [In-Detail: Nexus Multi-Chain State Synchronization](architecture/settlement-layer/multi-chain-state-synchronization/in-detail-nexus-multi-chain-state-synchronization.md)
  * [Supported Networks of zkLink Nexus and Origin](architecture/settlement-layer/supported-networks-of-zklink-nexus-and-origin.md)
  * [Security Assumptions of zkLink Nexus and Origin](architecture/settlement-layer/security-assumptions-of-zklink-nexus-and-origin.md)
* [Execution Layer](architecture/execution-layer/README.md)
  * [TS-zkVM for App Rollup](architecture/execution-layer/ts-zkvm-for-app-rollup.md)
* [Sequencing Layer](architecture/sequencing-layer.md)
* [DA Layer](architecture/da-layer.md)

## 🛠️ Developer

* [Developer Overview](developer/overview.md)
* [Get Started](developer/quick-start/README.md)
* [Examples](developer/examples/README.md)
  * [Base Demo](developer/examples/base-demo.md)
* [JSON RPC & Websocket & Kafka](developer/json-rpc-and-websocket-and-kafka/README.md)
  * [JSON-RPC API](developer/api-and-sdk/json-rpc/json-rpc-api.md)
  * [JSON-RPC Errors](developer/api-and-sdk/json-rpc/json-rpc-errors.md)
  * [Websocket](developer/api-and-sdk/websocket/README.md)
  * [Kafka](developer/api-and-sdk/kafka/README.md)
* [Transactions](developer/api-and-sdk/README.md)
  * [Basic Types](developer/api-and-sdk/data-types/basic-types.md)
  * [State Update](developer/api-and-sdk/data-types/state-update.md)
  * [Transaction](developer/api-and-sdk/data-types/transaction/README.md)
    * [Deposit](developer/api-and-sdk/data-types/transaction/deposit.md)
    * [FullExit](developer/api-and-sdk/data-types/transaction/full\_exit.md)
    * [ChangePubKey](developer/api-and-sdk/data-types/transaction/change\_pubkey.md)
    * [Withdraw](developer/api-and-sdk/data-types/transaction/withdraw.md)
    * [Transfer](developer/api-and-sdk/data-types/transaction/transfer.md)
    * [ForcedExit](developer/api-and-sdk/data-types/transaction/forced\_exit.md)
    * [OrderMatching](developer/api-and-sdk/data-types/transaction/order\_matching.md)
    * [AutoDeleveraging](developer/api-and-sdk/data-types/transaction/auto\_deleveraging.md)
    * [ContractMatching](developer/api-and-sdk/data-types/transaction/contract\_matching.md)
    * [Funding](developer/api-and-sdk/data-types/transaction/funding.md)
    * [Liquidation](developer/api-and-sdk/data-types/transaction/liquidation.md)
    * [UpdateGlobalVar](developer/api-and-sdk/data-types/transaction/update\_global\_var.md)
  * [Private Key & Signature](developer/api-and-sdk/private-key-and-signature/private\_key.md)
    * [Algorithm](developer/api-and-sdk/private-key-and-signature/encode/algorithm.md)
    * [ChangePubKey](developer/api-and-sdk/private-key-and-signature/encode/chaneg\_pubkey.md)
    * [Withdraw](developer/api-and-sdk/private-key-and-signature/encode/withdraw.md)
    * [Transfer](developer/api-and-sdk/private-key-and-signature/encode/transfer.md)
    * [ForcedExit](developer/api-and-sdk/private-key-and-signature/encode/forced\_exit.md)
    * [OrderMatching](developer/api-and-sdk/private-key-and-signature/encode/order\_matching.md)
    * [ContractMatching](developer/api-and-sdk/private-key-and-signature/encode/contract\_matching.md)
    * [Funding](developer/api-and-sdk/private-key-and-signature/encode/funding.md)
    * [Liquidation](developer/api-and-sdk/private-key-and-signature/encode/liquidation.md)
    * [AutoDeleveraging](developer/api-and-sdk/private-key-and-signature/encode/auto\_deleveraging.md)
    * [UpdateGlobalVar](developer/api-and-sdk/private-key-and-signature/encode/update\_global\_var.md)
* [SDK](developer/sdk/sdk.md)
  * [Go](developer/sdk/go/changelog.md)
    * [Types](developer/sdk/go/basic\_types.md)
    * [Signature](developer/sdk/go/signer.md)
    * [Utils](developer/sdk/go/utils.md)
    * [Transactions](developer/sdk/go/transactions/README.md)
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
    * [Signature](developer/sdk/js/signer.md)
    * [Utils](developer/sdk/js/utils.md)
    * [Transactions](developer/sdk/js/transactions/README.md)
      * [ChangePubKey](developer/sdk/js/transactions/1-change-pubkey.md)
      * [Withdraw](developer/sdk/js/transactions/2-withdraw.md)
      * [Transfer](developer/sdk/js/transactions/3-transfer.md)
      * [ForcedExit](developer/sdk/js/transactions/4-forced-exit.md)
      * [OrderMatching](developer/sdk/js/transactions/5-order-matching.md)
      * [ContractMatching](developer/sdk/js/transactions/6-contract-matching.md)
      * [AutoDeleveraging](developer/sdk/js/transactions/7-auto-deleveraging.md)
      * [Funding](developer/sdk/js/transactions/8-funding.md)
      * [Liquidation](developer/sdk/js/transactions/9-liquidation.md)
      * [UpdateGlobalVar](developer/sdk/js/transactions/10-update-global-var.md)
  * [Dart](developer/sdk/dart/changelog.md)
    * [Signature](developer/sdk/dart/signer.md)
    * [Utils](developer/sdk/dart/utils.md)
    * [Transactions](developer/sdk/dart/transactions/README.md)
      * [ChangePubKey](developer/sdk/dart/transactions/1-change-pubkey.md)
      * [Withdraw](developer/sdk/dart/transactions/2-withdraw.md)
      * [Transfer](developer/sdk/dart/transactions/3-transfer.md)
      * [ForcedExit](developer/sdk/dart/transactions/4-forced-exit.md)
      * [OrderMatching](developer/sdk/dart/transactions/5-order-matching.md)
      * [ContractMatching](developer/sdk/dart/transactions/6-contract-matching.md)
      * [AutoDeleveraging](developer/sdk/dart/transactions/7-auto-deleveraging.md)
      * [Funding](developer/sdk/dart/transactions/8-funding.md)
      * [Liquidation](developer/sdk/dart/transactions/9-liquidation.md)
      * [UpdateGlobalVar](developer/sdk/dart/transactions/10-update-global-var.md)

## ⚙️ Network Information

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

## Appendix

* [Audits](reference/Audits.md)
* [FAQ](appendix/faq.md)
* [glossary](appendix/glossary.md)
