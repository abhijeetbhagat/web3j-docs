Linea RPC Support
=================

Web3j 5.x includes typed response classes and convenience methods for several Linea-specific RPC endpoints.

## `linea_estimateGas`

```java
LineaEstimateGas response =
        web3j.lineaEstimateGas(
                        Transaction.createEthCallTransaction(
                                "0xa70e8dd61c5d32be8058bb8eb970870f07233155",
                                "0x52b93c80364dc2dd4444c146d73b9836bbbb2b3f",
                                "0x0"))
                .send();
```

## `linea_getProof`

```java
LineaGetProof response =
        web3j.lineaGetProof(
                        "0x7F0d15C7FAae65896648C8273B6d7E43f58Fa842",
                        Arrays.asList(
                                "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"),
                        "latest")
                .send();
```

## `linea_getTransactionExclusionStatusV1`

```java
LineaGetTransactionExclusionStatusV1 response =
        web3j.lineaGetTransactionExclusionStatusV1(
                        "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238")
                .send();
```

These methods are only useful when the connected endpoint supports the corresponding Linea extensions.
