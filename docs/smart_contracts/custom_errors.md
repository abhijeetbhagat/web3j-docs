Solidity Custom Errors
======================

Web3j 5.x supports Solidity custom error definitions in both the ABI layer and wrapper generation.

This is useful when working with contracts compiled from Solidity versions that emit `error` definitions.

## Encoding a custom error signature

Use `CustomError` together with `CustomErrorEncoder` to build the error selector:

```java
CustomError error =
        new CustomError(
                "InvalidAccess",
                Arrays.<TypeReference<?>>asList(
                        new TypeReference<Address>() {},
                        new TypeReference<Utf8String>() {},
                        new TypeReference<Uint256>() {}));

String signatureHash = CustomErrorEncoder.encode(error);
```

## Generated wrappers

When wrapper generation encounters Solidity custom errors, Web3j generates the corresponding error definitions as part of the wrapper output.

Generated wrappers also include generated-code annotations where supported by the current JDK and generator configuration. Treat generated classes as derived artifacts and re-run code generation instead of editing them manually.
