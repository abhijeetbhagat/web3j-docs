## EIP-7702 Authorization List Transaction

Web3j 5.x supports EIP-7702 typed transactions through `RawTransaction` and the `Transaction7702` encoder.

This format extends the EIP-1559 transaction with an authorization list.

## Creating an EIP-7702 transaction

```java
List<AccessListObject> accessList = Collections.emptyList();
List<AuthorizationTuple> authorizationList =
        Collections.singletonList(
                new AuthorizationTuple(
                        BigInteger.ONE,
                        "0x1111111111111111111111111111111111111111",
                        BigInteger.ZERO,
                        BigInteger.ONE,
                        BigInteger.TWO,
                        BigInteger.TEN));

RawTransaction rawTransaction =
        RawTransaction.createTransaction(
                1L,
                BigInteger.ZERO,
                BigInteger.valueOf(1_500_000_000L),
                BigInteger.valueOf(3_000_000_000L),
                BigInteger.valueOf(21_000),
                "0x2222222222222222222222222222222222222222",
                BigInteger.ONE,
                "0x",
                accessList,
                authorizationList);
```

You can then sign and send this transaction in the same way as any other `RawTransaction`.

Web3j also exposes authorization tuples in transaction response types via `org.web3j.protocol.core.methods.response.AuthorizationTuple`.
