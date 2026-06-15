## Install Web3j

To start using Web3j you have two options:

### Use Web3j CLI

This is the recommended option for full project creation.

#### Install Web3j CLI

To install the command line tools, see [Command Line Tools](command_line_tools.md).

#### Create a new project

`$ web3j new`

This creates a sample project to get you started.

It includes a `HelloWorld` smart contract and boilerplate code that is easy to follow.

#### Run your project

Then run the project with the command `web3j run <network_url> <wallet_path> <wallet_password>`.
You can also provide additional runtime parameters, as described in [Command Line Tools](command_line_tools.md).

### Use Web3j plugins

To generate Java wrappers for Solidity contracts in your project, add one of the Web3j plugins available for Maven or Gradle.

#### Gradle plugin

To add the [Gradle](plugins/web3j_gradle_plugin.md) plugin to your project:

```groovy
plugins {
    id "org.web3j" version "<plugin-version>"
}
```

Once the plugin is downloaded you should see a new set of tasks under the Web3j label:

![](./general_media/web3j_plugin.png)

The simplest way is to create a new folder `src/main/solidity` and place your smart contracts there.
To generate the Java wrappers, run `generateContractWrappers` or `./gradlew generateContractWrappers`.
The wrappers are generated under `build/generated/source/web3j/main/java`.

#### Maven plugin

To add the [Maven](plugins/web3j_maven_plugin.md) plugin to your project:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.web3j</groupId>
            <artifactId>web3j-maven-plugin</artifactId>
            <version><!-- plugin version --></version>
            <configuration>
                <soliditySourceFiles/>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## Use Web3j

### Deploying a Smart Contract

To deploy the `HelloWorld` contract from the previous example:

```java
Web3j web3j = Web3j.build(new HttpService("<your_node_url>"));
HelloWorld helloWorld =
        HelloWorld.deploy(
                        web3j,
                        Credentials.create("your_private_key"),
                        new DefaultGasProvider(),
                        "Hello Blockchain World")
                .send();
String greeting = helloWorld.greeting().send();
web3j.shutdown();
```

There are many ways to create `Credentials` in Web3j. For more information see [Credentials](./transactions/credentials.md).

### Loading a Smart Contract

If you have already deployed a contract and would like to interact with it through Web3j, the generated wrappers expose a `load` method:

```java
Web3j web3j = Web3j.build(new HttpService("<your_node_url>"));
String greeting;
HelloWorld helloWorld =
        HelloWorld.load(
                "your_contract_address",
                web3j,
                Credentials.create("your_private_key"),
                new DefaultGasProvider());
if (helloWorld.isValid()) {
    greeting = helloWorld.greeting().send();
}
web3j.shutdown();
```

It is important that the loaded contract is checked using `isValid()`. This returns `false` if the wrapper bytecode does not match the deployed contract bytecode.
