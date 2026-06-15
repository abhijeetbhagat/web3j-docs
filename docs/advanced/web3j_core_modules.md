Web3j Core Modules
=======

To provide greater flexibility for developers wishing to work with Web3j, the project is made up of a number of modules.

In dependency order, they are as follows:

-   utils - minimal set of utility classes
-   rlp - Recursive Length Prefix (RLP) encoders
-   abi - Application Binary Interface (ABI) encoders
-   crypto - cryptographic library for transaction signing and key or wallet management
-   tuples - simple tuples library
-   core - core JSON-RPC, contracts, ENS, transaction management, and service abstractions
-   codegen - wrapper and ABI code generators

The below modules only depend on the core module.

-   geth - Geth specific JSON-RPC module
-   parity - Parity/OpenEthereum-compatible JSON-RPC module
-   hosted-providers - hosted provider HTTP helpers, including Infura support
-   contracts - support for specific EIPs
-   besu - support for private transactions on Hyperledger Besu
-   eea - support for EEA privacy extensions

For most use cases, the `core` module should be all you need. The lower-level modules are useful when your project is focused on a very specific interaction such as ABI encoding, RLP encoding, or signing without submission.

Further details
---------------

In the Java build:

-   Web3j provides type safe access to responses.
-   Asynchronous requests are wrapped in Java `CompletableFuture`s.
-   The Java 5.x artifacts are compiled for Java 21.

In both the Java and Android builds:

-   Quantity payload types are returned as `BigInteger`s.
-   It is possible to include the raw JSON payload in responses via the `includeRawResponse` parameter exposed by the service implementations.
