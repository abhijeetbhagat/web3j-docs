Web3j
=====

Web3j is a highly modular, reactive, type safe Java and Android library for working with smart contracts and integrating with Ethereum clients:

![image](img/web3j_network.png)

This allows you to work with the [Ethereum](https://www.ethereum.org/) blockchain without writing your own low-level integration code.

The [Java and the Blockchain](https://www.youtube.com/watch?v=ea3miXs_P6Y) talk provides an overview of blockchain, Ethereum and Web3j.

Features
========

-   Complete implementation of Ethereum's [JSON-RPC](https://ethereum.org/en/developers/docs/apis/json-rpc/) client API over HTTP, WebSockets and IPC
-   Ethereum wallet support
-   Auto-generation of Java smart contract wrappers to create, deploy, transact with and call smart contracts from native Java code
-   Reactive-functional API for working with filters
-   [Ethereum Name Service (ENS)](https://ens.domains/) support
-   Support for typed transactions including EIP-2930, EIP-1559, EIP-4844 and EIP-7702
-   Support for Account Abstraction JSON-RPC methods such as `eth_sendUserOperation`
-   Support for Geth, Besu, legacy Parity/OpenEthereum-compatible RPC modules, hosted providers, and Besu privacy extensions
-   Support for [Alchemy](https://alchemyapi.io/) and [Infura](https://infura.io/)
-   Support for ERC20 and ERC721 token standards
-   Support for HSM-based signing, AWS KMS integration, and JWK generation
-   Comprehensive integration tests demonstrating a number of the above scenarios
-   Command line tools
-   Android-compatible artifacts via the latest Android-tagged release
-   Support for [EEA privacy features](https://entethalliance.org/technical-documents/) as implemented by [Hyperledger Besu](https://besu.hyperledger.org/)

Dependencies
============

The Java 5.x artifacts are compiled for Java 21.

If you are targeting Android, use the latest Android-tagged Web3j artifact published by the project. At the time of this docs refresh that is `{{ android.version }}`.

For Maven and Gradle snippets, see [Manual Configuration](getting_started/manual_configuration.md).
