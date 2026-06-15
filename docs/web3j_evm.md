Web3j EVM
==========================================

Web3j EVM is an embedded freestanding Ethereum EVM and ledger running within a Java process, which can be used for unit and integration testing your Web3j projects.

As everything is local and in-process, there is no need to start external Ethereum nodes, which helps with ease of use and performance.

## Getting started

Often you'd use this together with the [Web3j Unit](https://github.com/web3j/web3j-unit) project, allowing you to run tests without the need to start an Ethereum node.

If you want to use this directly in your own project, you need the EVM dependency and a few external libraries. Have a look at the [example project](https://github.com/web3j/web3j-evmexample) for a complete setup.

```groovy
repositories {
    mavenCentral()
}

dependencies {
    implementation "org.web3j:core:{{ web3j.version }}"
    implementation "org.web3j:web3j-evm:{{ web3j.version }}"
}
```

Below is a simple demonstration of ETH transactions, contract deployment and contract interactions. Using the `ConsoleDebugTracer`, you can step through the EVM bytecode, inspect the stack and see where in the related Solidity code you currently are.
