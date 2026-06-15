## Transaction mechanisms

When you have a valid account created with some Ether, there are two mechanisms you can use to transact with Ethereum.

1.  [Transaction signing via an Ethereum client](#transaction-signing-via-an-ethereum-client)
2.  [Offline transaction signing](#offline-transaction-signing)

Both mechanisms are supported via Web3j.

## Transaction signing via an Ethereum client

In order to transact via an Ethereum client, you first need to ensure that the client you're transacting with knows about your wallet address. If the node is responsible for signing, the account must exist in the node keystore.

You can create a wallet via:

- The [Geth documentation](https://geth.ethereum.org/docs/interface/managing-your-accounts)
- A JSON-RPC admin command such as `personal_newAccount`

With your wallet file created, you can unlock your account via Web3j by creating an instance that supports admin commands:

```java
Admin web3j = Admin.build(new HttpService());
```

Then you can unlock the account and, provided this was successful, send a transaction:

```java
PersonalUnlockAccount personalUnlockAccount =
        web3j.personalUnlockAccount("0x000...", "a password").send();
if (personalUnlockAccount.accountUnlocked()) {
    // send a transaction
}
```

Further details of working with the different admin commands supported by Web3j are available in [Management APIs](../advanced/management_apis.md).

## Offline transaction signing

If you'd prefer not to manage your own Ethereum client, or do not want to provide wallet details such as your password to an Ethereum client, then offline transaction signing is the way to go.

Offline transaction signing allows you to sign a transaction within Web3j, giving you complete control over your private credentials. A transaction created offline can then be sent to any Ethereum client on the network, provided it is valid.

You can also perform out-of-process transaction signing if required by overriding the `sign` method in `ECKeyPair`, or by using the HSM-based signing flow described in [HSM Transaction Signing](../advanced/HSM_transaction_signing.md).
