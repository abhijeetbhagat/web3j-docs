## Dynamic Gas Providers

Web3j 5.x provides dynamic gas providers for applications that want to fetch current gas values from the connected network instead of hardcoding them.

## Legacy-style gas pricing

Use `DynamicGasProvider` when you want Web3j to read the current gas price from `eth_gasPrice`.

```java
DynamicGasProvider gasProvider = new DynamicGasProvider(web3j);

MyContract contract =
        MyContract.deploy(web3j, credentials, gasProvider, constructorArg).send();
```

`DynamicGasProvider` also supports priorities:

```java
DynamicGasProvider normal = new DynamicGasProvider(web3j);
DynamicGasProvider fast = new DynamicGasProvider(web3j, PriorityGasProvider.Priority.FAST);
DynamicGasProvider slow = new DynamicGasProvider(web3j, PriorityGasProvider.Priority.SLOW);
DynamicGasProvider custom =
        new DynamicGasProvider(
                web3j,
                PriorityGasProvider.Priority.CUSTOM,
                BigDecimal.valueOf(1.5));
```

## EIP-1559 gas pricing

Use `DynamicEIP1559GasProvider` when your target network expects EIP-1559 fee fields.

```java
DynamicEIP1559GasProvider gasProvider =
        new DynamicEIP1559GasProvider(web3j, 1L);

MyContract contract =
        MyContract.deploy(web3j, credentials, gasProvider, constructorArg).send();
```

Both dynamic providers can estimate gas limits for a transaction object:

```java
Transaction tx =
        Transaction.createEthCallTransaction(
                credentials.getAddress(),
                contractAddress,
                encodedFunction);

BigInteger gasLimit = gasProvider.getGasLimit(tx);
```
