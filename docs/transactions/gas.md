## Gas

When a transaction takes place on Ethereum, a transaction cost must be paid to the client that executes the transaction on your behalf and commits the result to the blockchain.

This cost is measured in gas, which represents the work required to execute a transaction in the Ethereum Virtual Machine.

When working with Web3j, the important parameters are:

- `gasLimit`
- either `gasPrice` for legacy or EIP-2930 transactions
- or `maxPriorityFeePerGas` and `maxFeePerGas` for EIP-1559 style transactions

These parameters taken together dictate the maximum amount of Ether you are willing to spend on transaction costs.

### Picking Gas Providers

Web3j exposes multiple gas provider implementations for contracts:

- `DefaultGasProvider` for fixed default values
- `StaticGasProvider` when you want to provide explicit legacy values
- `StaticEIP1559GasProvider` when you want to provide explicit EIP-1559 values
- `DynamicGasProvider` when you want Web3j to fetch the current gas price
- `DynamicEIP1559GasProvider` when you want Web3j to derive EIP-1559 values from the network

The static providers are useful when your application has its own gas policy. The dynamic providers are useful when you want the client to adapt to current network conditions.

For the dynamic providers, see [Dynamic Gas Providers](dynamic_gas_providers.md).

### Contract deployment example

```java
ContractGasProvider gasProvider =
        new StaticGasProvider(
                BigInteger.valueOf(5_000_000_000L),
                BigInteger.valueOf(6_000_000L));

MyContract contract =
        MyContract.deploy(web3j, credentials, gasProvider, constructorArg).send();
```
