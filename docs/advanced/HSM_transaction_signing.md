HSM Transaction Signing
==================

Hardware Security Modules (HSMs) protect signing keys and keep private key material outside the application process. In blockchain systems they are commonly used to sign transactions without ever loading the raw private key into application memory.

Web3j provides HSM signing support through `TxHSMSignService` and pluggable request processors.

Web3j implementation
-----------------------------

To make this possible, Web3j provides [TxHSMSignService](https://github.com/LFDT-web3j/web3j/blob/main/core/src/main/java/org/web3j/service/TxHSMSignService.java). This implements `TxSignService` so it can be injected into a transaction manager.

![image](../img/HSM/HSM_sign_service.png)

`TxHSMSignService` is built around two main abstractions:

- `HSMPass`, which holds the public key information required to derive the Ethereum address
- `HSMRequestProcessor`, which submits signing requests to the HSM and returns `Sign.SignatureData`

![image](../img/HSM/HSM_interfaces.png)

Usage in web3j
-----------------------------

To sign transactions with an HSM using Web3j:

- Implement `HSMRequestProcessor` for your HSM backend. Web3j includes HTTP-oriented helpers via `HSMHTTPRequestProcessor` and also ships an AWS KMS implementation via `HSMAwsKMSRequestProcessor`.

![image](../img/HSM/HSM_http_approach.png)

`HSMHTTPRequestProcessor` is abstract, so you need to implement `createRequest()` and `readResponse()`. The [test implementation](https://github.com/LFDT-web3j/web3j/blob/main/core/src/test/java/org/web3j/tx/HSMHTTPRequestProcessorTestImpl.java) is a good starting point.

- Instantiate a new `HSMPass` object:

```java
HSMHTTPPass hsmhttpPass = new HSMHTTPPass(
        <account_address>,
        <account_public_key>,
        <hsm_url>);
```

- Instantiate your request processor:

```java
HSMHTTPRequestProcessor hsmRequestProcessor =
        new HSMHTTPRequestProcessorTestImpl<>(<http_client>);
```

- Create the signing service:

```java
TxSignService txHSMSignService =
        new TxHSMSignService<>(hsmRequestProcessor, hsmhttpPass);
```

- Submit the signing service to a transaction manager:

```java
RawTransactionManager transactionManager =
        new RawTransactionManager(web3j, txHSMSignService, <chain_id>);
```

AWS KMS
-------

Web3j 5.x also provides `HSMAwsKMSRequestProcessor` for AWS KMS keys of type `ECC_SECG_P256K1`.

```java
KmsClient kmsClient = KmsClient.create();
HSMPass pass = new HSMPass("<address>", <publicKeyBigInteger>);
TxSignService txSignService =
        new TxHSMSignService(new HSMAwsKMSRequestProcessor(kmsClient, "<kms-key-id>"), pass);
RawTransactionManager transactionManager =
        new RawTransactionManager(web3j, txSignService, <chainId>);
```

After wiring the signing service into your transaction manager, transactions are signed by the HSM-backed service instead of a local private key.
