## Privacy Support in Web3j

As mentioned earlier, the Besu module in Web3j can be used to create privacy groups and send private transactions in Besu. The Besu privacy quickstart typically starts three nodes which we will call Alice, Bob and Charlie for ease of use. The keys associated with each can be represented in Java as follows:

```java
private static final Credentials ALICE =
        Credentials.create("8f2a55949038a9610f50fb23b5883af3b4ecb3c3bb792cbcefbd1542c692be63");
private static final Credentials BOB =
        Credentials.create("c87509a1c067bbde78beb793e6fa76530b6382a4c0241e5e4a9ec0a0f44dc0d3");
private static final Credentials CHARLIE =
        Credentials.create("ae6ae8e5ccbfb04590405997ee2d52d2b330726137b875053c36d94e974d162f");

private static final Base64String ENCLAVE_KEY_ALICE =
        Base64String.wrap("A1aVtMxLCUHmBVHXoZzzBgPbW/wj5axDpW9X8l91SGo=");
private static final Base64String ENCLAVE_KEY_BOB =
        Base64String.wrap("Ko2bVqD+nNlNYL5EE7y3IdOnviftjiizpjRt+HTuFBs=");
private static final Base64String ENCLAVE_KEY_CHARLIE =
        Base64String.wrap("k2zXEin4Ip/qBGlRkJejnGWdP9cjkK+DAvKNW31L2C8=");
```

The instances of `Credentials` above are the private keys associated with the default account of each Besu node in the network. The `Base64String` values are the enclave public keys associated with each member. If you create your own network, these values will differ from the quickstart defaults.

Web3j provides the class `org.web3j.protocol.besu.Besu` which encapsulates Besu-specific methods together with the usual Web3j commands. We can use several instances of these to talk to the running quickstart network:

```java
private static Besu nodeAlice = Besu.build(new HttpService("http://localhost:20000"));
private static Besu nodeBob = Besu.build(new HttpService("http://localhost:20002"));
private static Besu nodeCharlie = Besu.build(new HttpService("http://localhost:20004"));
private static PollingPrivateTransactionReceiptProcessor processor =
        new PollingPrivateTransactionReceiptProcessor(nodeAlice, 1000, 15);
```

### Creating a privacy group

In order to create a new privacy group, first a random 32 byte base64 string is generated to form the group ID. Then the create group function is called, and we wait for the transaction receipt to be returned. Finally, we can call `privOnChainFindPrivacyGroup`, which will find the privacy group using the enclave keys of the group members.

```java
Base64String privacyGroupId = Base64String.wrap(generateRandomBytes(32));

final String txHash =
        nodeAlice
                .privOnChainCreatePrivacyGroup(
                        privacyGroupId,
                        ALICE,
                        ENCLAVE_KEY_ALICE,
                        Collections.singletonList(ENCLAVE_KEY_BOB))
                .send()
                .getTransactionHash();

TransactionReceipt receipt = processor.waitForTransactionReceipt(txHash);

Optional<PrivacyGroup> group =
        nodeAlice
                .privOnChainFindPrivacyGroup(
                        Arrays.asList(ENCLAVE_KEY_ALICE, ENCLAVE_KEY_BOB))
                .send()
                .getGroups().stream()
                .filter(x -> x.getPrivacyGroupId().equals(privacyGroupId))
                .findFirst();
```

### Creating and interacting with a smart contract

Once a privacy group has been created, smart contracts can be deployed and executed within the privacy group. The state of the contract and all associated private transactions are only visible to the group members.

```java
final PrivateTransactionManager tmAlice =
        new BesuPrivateTransactionManager(
                nodeAlice,
                ZERO_GAS_PROVIDER,
                ALICE,
                2018,
                ENCLAVE_KEY_ALICE,
                aliceBobGroup);

final HumanStandardToken tokenAlice =
        HumanStandardToken.deploy(
                        nodeAlice,
                        tmAlice,
                        ZERO_GAS_PROVIDER,
                        BigInteger.TEN,
                        "eea_token",
                        BigInteger.TEN,
                        "EEATKN")
                .send();
```
