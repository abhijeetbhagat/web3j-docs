Start sending requests
----------------------

To send synchronous requests:

```java
Web3j web3j = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Web3ClientVersion web3ClientVersion = web3j.web3ClientVersion().send();
String clientVersion = web3ClientVersion.getWeb3ClientVersion();
```

To send asynchronous requests using a `CompletableFuture`:

```java
Web3j web3j = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Web3ClientVersion web3ClientVersion = web3j.web3ClientVersion().sendAsync().get();
String clientVersion = web3ClientVersion.getWeb3ClientVersion();
```

To use an RxJava `Flowable`:

```java
Web3j web3j = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
web3j.web3ClientVersion().flowable().subscribe(x -> {
    String clientVersion = x.getWeb3ClientVersion();
    // ...
});
```

IPC
---

Web3j also supports inter-process communication (IPC) via file sockets to clients running on the same host as Web3j.

```java
// OS X/Linux/Unix:
Web3j web3j = Web3j.build(new UnixIpcService("/path/to/socketfile"));

// Windows:
Web3j web3j = Web3j.build(new WindowsIpcService("/path/to/namedpipefile"));
```

**Note:** IPC is not available on `web3j-android`.

Transactions
------------

Web3j provides support for both Ethereum wallet files and Ethereum client admin commands for sending transactions.

## Send Ether

To send Ether to another party using your Ethereum wallet file:

```java
Web3j web3j = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");
TransactionReceipt transactionReceipt =
        Transfer.sendFunds(
                        web3j,
                        credentials,
                        "0x<address>|<ensName>",
                        BigDecimal.valueOf(1.0),
                        Convert.Unit.ETHER)
                .send();
```

## Custom transaction

If you wish to create your own custom transaction:

```java
Web3j web3j = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");

EthGetTransactionCount ethGetTransactionCount =
        web3j.ethGetTransactionCount(
                        credentials.getAddress(),
                        DefaultBlockParameterName.LATEST)
                .send();
BigInteger nonce = ethGetTransactionCount.getTransactionCount();

RawTransaction rawTransaction =
        RawTransaction.createEtherTransaction(
                nonce,
                BigInteger.valueOf(20_000_000_000L),
                BigInteger.valueOf(21_000),
                "<toAddress>",
                BigInteger.valueOf(1_000_000_000_000_000_000L));

byte[] signedMessage = TransactionEncoder.signMessage(rawTransaction, credentials);
String hexValue = Numeric.toHexString(signedMessage);
EthSendTransaction ethSendTransaction = web3j.ethSendRawTransaction(hexValue).send();
```

Although it's far simpler to use Web3j's `Transfer` helper for sending Ether.

Using an Ethereum client's admin commands (make sure you have your wallet in the client's keystore):

```java
Admin web3j = Admin.build(new HttpService());  // defaults to http://localhost:8545/
PersonalUnlockAccount personalUnlockAccount =
        web3j.personalUnlockAccount("0x000...", "a password").sendAsync().get();
if (personalUnlockAccount.accountUnlocked()) {
    // send a transaction
}
```

If you want to make use of Geth-specific personal APIs or legacy Parity/OpenEthereum-compatible APIs, you can use the `org.web3j:geth` and `org.web3j:parity` modules respectively.
