Management APIs
===============

In addition to the standard [JSON-RPC](https://ethereum.org/en/developers/docs/apis/json-rpc/) API, some Ethereum clients expose additional management APIs over JSON-RPC.

One of the most common management tasks is creating and unlocking accounts for node-managed signing.

-   [Geth personal namespace](https://geth.ethereum.org/docs/rpc/ns-personal)

Support for these APIs is available in Web3j. The methods that live in the shared admin layer are exposed by the [Admin](https://github.com/LFDT-web3j/web3j/blob/main/core/src/main/java/org/web3j/protocol/admin/Admin.java) module.

You can initialise a connector that supports this module using:

```java
Admin web3j = Admin.build(new HttpService());  // defaults to http://localhost:8545/
PersonalUnlockAccount personalUnlockAccount =
        web3j.personalUnlockAccount("0x000...", "a password").send();
if (personalUnlockAccount.accountUnlocked()) {
    // send a transaction
}
```

For Geth specific methods, you can use the [Geth](https://github.com/LFDT-web3j/web3j/blob/main/geth/src/main/java/org/web3j/protocol/geth/Geth.java) connector.

For legacy Parity/OpenEthereum-compatible APIs, you can use the [Parity](https://github.com/LFDT-web3j/web3j/blob/main/parity/src/main/java/org/web3j/protocol/parity/Parity.java) connector. The `Parity` connector also provides access to trace-style RPC calls for networks that still expose them.

For most new applications, local signing with `Credentials` or `TransactionManager` is preferred. Use management APIs only when the connected node is expected to manage accounts on your behalf.
