Account Abstraction
===================

Web3j supports EIP-4337 user operation RPC methods for account abstraction flows.

The supported methods are:

-   `ethSendUserOperation`
-   `ethEstimateUserOperationGas`
-   `ethGetUserOperationByHash`
-   `ethGetUserOperationReceipt`
-   `ethSupportedEntryPoints`

### Example

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

EthSendUserOperation response =
        web3j.ethSendUserOperation(
                        userOperation,
                        "0xa70e8dd61c5d32be8058bb8eb970870f07233156")
                .send();

System.out.println(response.getUserOperationHash());
```

For a walkthrough of ERC-4337 smart accounts with Web3j, refer to the [smart account tutorial](https://www.lfdecentralizedtrust.org/blog/erc-4337-smart-account-tutorial-with-web3j).
