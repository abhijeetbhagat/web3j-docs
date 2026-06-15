## Account Abstraction (EIP-4337)

Web3j 5.x includes support for the main EIP-4337 bundler JSON-RPC methods.

These methods operate on `UserOperationStruct` rather than `RawTransaction`.

## Creating a user operation

```java
UserOperationStruct userOperation =
        UserOperationStruct.createEthSendUserOperationTransaction(
                "0xa70e8dd61c5d32be8058bb8eb970870f07233155",
                BigInteger.ONE,
                "0x0",
                "0x0",
                "0x0",
                "0x0",
                "0x0",
                "0x0",
                "0x0",
                "0x0",
                "0x0");
```

## Sending a user operation

```java
EthSendUserOperation response =
        web3j.ethSendUserOperation(
                        userOperation,
                        "0xa70e8dd61c5d32be8058bb8eb970870f07233156")
                .send();

String userOperationHash = response.getUserOperationHash();
```

## Estimating gas

```java
EthEstimateUserOperationGas estimate =
        web3j.ethEstimateUserOperationGas(
                        userOperation,
                        "0xa70e8dd61c5d32be8058bb8eb970870f07233156")
                .send();
```

## Bundler discovery and lookup

```java
EthSupportedEntryPoints entryPoints = web3j.ethSupportedEntryPoints().send();
EthGetUserOperationByHash operation = web3j.ethGetUserOperationByHash(userOperationHash).send();
EthGetUserOperationReceipt receipt =
        web3j.ethGetUserOperationReceipt(userOperationHash).send();
```

These RPC methods require a node or bundler endpoint that actually implements the EIP-4337 API surface.
