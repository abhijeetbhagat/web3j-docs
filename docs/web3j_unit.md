Web3j Unit
===========

Web3j Unit is an extension to [JUnit 5](https://junit.org/junit5/docs/current/user-guide/) which enables you to test Solidity contracts like any other Java code.

It supports embedded and dockerized Ethereum nodes, with out-of-the-box support for Geth and Besu. A Docker Compose network can also be configured for more complex setups.

## Usage

First, add the Gradle dependency.

```groovy
repositories {
    mavenCentral()
}
implementation "org.web3j:core:{{ web3j.version }}"
testImplementation "org.web3j:web3j-unit:<web3j-unit-version>"
```

### Using EVMTest annotation

Create a new test with the `@EVMTest` annotation. An embedded EVM is used by default. To use Geth or Besu, pass the node type into the annotation: `@EVMTest(type = NodeType.GETH)` or `@EVMTest(type = NodeType.BESU)`.

```java
@EVMTest
public class GreeterTest {
}
```

Inject instances of `Web3j`, `TransactionManager` and `ContractGasProvider` in your test method.

```java
@EVMTest
public class GreeterTest {

    @Test
    public void greeterDeploys(
            Web3j web3j,
            TransactionManager transactionManager,
            ContractGasProvider gasProvider) {
    }
}
```

Deploy your contract in the test:

```java
@EVMTest
public class GreeterTest {

    @Test
    public void greeterDeploys(
            Web3j web3j,
            TransactionManager transactionManager,
            ContractGasProvider gasProvider) throws Exception {
        Greeter greeter =
                Greeter.deploy(web3j, transactionManager, gasProvider, "Hello EVM").send();
        String greeting = greeter.greet().send();
        assertEquals("Hello EVM", greeting);
    }
}
```

### Using EVMComposeTest annotation

Create a new test with the `@EVMComposeTest` annotation.

By default it uses `test.yml` in the project root, and runs Web3j against a service named `node1` exposing port `8545`.

```java
@EVMComposeTest("src/test/resources/geth.yml", "ethnode1", 8080)
public class GreeterTest {
}
```
