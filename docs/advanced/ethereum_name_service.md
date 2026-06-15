Ethereum Name Service
=====================

The [Ethereum Name Service (ENS)](https://ens.domains) provides human-readable names for addresses and onchain resources.

In practice, this means you can use a name such as `web3j.eth` anywhere Web3j expects an address.

Usage in web3j
--------------

You can use ENS names anywhere you wish to transact in Web3j. This includes smart contract wrappers:

```java
YourSmartContract contract = YourSmartContract.load(
        "0x<address>|<ensName>", web3j, credentials, contractGasProvider);
```

and Ether transfers:

```java
TransactionReceipt receipt =
        Transfer.sendFunds(
                        web3j,
                        credentials,
                        "nick.eth",
                        BigDecimal.ONE,
                        Convert.Unit.ETHER)
                .send();
```

ENS Features Supported with Code Examples
-----------------------------------------

#### ENS is supported for these chains

- Ethereum Mainnet
- Sepolia Testnet
- Holesky Testnet
- Linea Mainnet
- Linea Sepolia Testnet

#### Code Examples

- Forward resolution from ENS to address:

```java
Web3j web3j = Web3j.build(new HttpService("<rpc_endpoint_url>"));
EnsResolver ensResolver = new EnsResolver(web3j);

String ensName = NameHash.normalise("nick.eth");
System.out.println("ENS address = " + ensResolver.resolve(ensName));
```

- Reverse resolution from address to primary ENS:

```java
String ensPrimaryName =
        ensResolver.reverseResolve("0x225f137127d9067788314bc7fcc1f36746a3c3B5");
```

- Set primary ENS name:

```java
Credentials credentials = Credentials.create("<private_key>");
TransactionReceipt receipt = ensResolver.setReverseName("nick.eth", credentials);
```

- Get `nameHash`, `labelHash`, and DNS-encoded values:

```java
String nameHashString = NameHash.nameHash("luc.eth");
byte[] nameHash = NameHash.nameHashAsBytes("luc.eth");

String labelHashString = NameHash.labelHash("luc");
byte[] labelHash = NameHash.labelHashAsBytes("luc");

String dnsEncodedName = NameHash.dnsEncode("name.eth");
```

- Get and set ENS text records:

```java
String url = ensResolver.getEnsText("nick.eth", "url");
TransactionReceipt receipt =
        ensResolver.setEnsText("nick.eth", "url", "http://example.com", credentials);
```

Web3j implementation
--------------------

Whenever you use Web3j transaction managers, the [EnsResolver](https://github.com/LFDT-web3j/web3j/blob/main/core/src/main/java/org/web3j/ens/EnsResolver.java) is invoked to perform an ENS lookup if applicable.

The resolution process is as follows:

-   Check whether the node is synced
-   Fail if it is not
-   If it is synced, check the timestamp on the latest block
    - If it is older than the sync threshold, fail
    - Otherwise perform the lookup

As of Web3j 5.0.3, `EnsResolver.resolve()` routes address resolution through the Universal Resolver and falls back as needed for older deployments. The resolver also supports CCIP-read or offchain lookup flows when the target resolver exposes them.

If you need to change what constitutes a synced node, use `setSyncThreshold()` on `EnsResolver`.

Normalisation
-------------

ENSIP-15 is used to normalise input before resolution. For implementation details, refer to the [NameHash](https://github.com/LFDT-web3j/web3j/blob/main/core/src/main/java/org/web3j/ens/NameHash.java) class.

Registering domain names
------------------------

Web3j does not provide a high-level domain registration workflow. If you need to register or manage ENS names directly, use the ENS contracts or the official ENS tooling and documentation.
