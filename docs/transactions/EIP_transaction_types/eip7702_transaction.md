## EIP-7702 Transaction

EIP-7702 introduces a new typed transaction for delegated smart account execution. In addition to the EIP-1559 style fee fields and access list support, it includes an `authorizationList` containing signed authorization tuples.

### Spec

The new EIP-2718 `TransactionType = 4(0x04)` and TransactionPayload for this transaction is `rlp([chainId, nonce, maxPriorityFeePerGas, maxFeePerGas, gasLimit, to, value, data, accessList, authorizationList, signatureYParity, signatureR, signatureS])`.

In Web3j this transaction type is represented by `TransactionType.EIP7702`, and raw transaction creation accepts both an `accessList` and an `authorizationList`.

### Example

```java
public void signedEip7702() throws SignatureException {
        Credentials credentials = Credentials.create("<privateKey>");
        Web3j web3j = Web3j.build(new HttpService("<nodeUrl>"));

        final RawTransaction rawTransaction = createEip7702RawTransaction();

        final byte[] signedMessage =
                TransactionEncoder.signMessage(rawTransaction, credentials);
        final String signedHexMessage = Numeric.toHexString(signedMessage);

        EthSendTransaction ethSendTransaction =
                web3j.ethSendRawTransaction(signedHexMessage).send();

        System.out.println("Transaction hash: " + ethSendTransaction.getTransactionHash());
    }

private static RawTransaction createEip7702RawTransaction() {
        List<AccessListObject> accessList = Collections.emptyList();
        List<AuthorizationTuple> authorizationList =
                Collections.singletonList(
                        new AuthorizationTuple(
                                BigInteger.valueOf(1),
                                "0x7b73644935b8e68019ac6356c40661e1bc315860",
                                BigInteger.ZERO,
                                BigInteger.ONE,
                                new BigInteger("1234"),
                                new BigInteger("5678")));

        return RawTransaction.createTransaction(
                1L,
                BigInteger.valueOf(1),
                BigInteger.valueOf(2_000_000_000L),
                BigInteger.valueOf(20_000_000_000L),
                BigInteger.valueOf(100000),
                "0x627306090abab3a6e1400e9345bc60c78a8bef57",
                BigInteger.ZERO,
                "0x",
                accessList,
                authorizationList);
    }
```

Transactions returned from JSON-RPC methods can also include EIP-7702 authorization data. In Web3j this is exposed via `Transaction.getAuthorizationList()`.
