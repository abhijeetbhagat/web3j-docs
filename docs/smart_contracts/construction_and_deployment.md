Construction and deployment
---------------------------

Construction and deployment of smart contracts happens with the `deploy` method:

```java
YourSmartContract contract = YourSmartContract.deploy(
        <web3j>, <credentials>, <contractGasProvider>,
        [<initialValue>,]
        <param1>, ..., <paramN>).send();
```

This creates a new instance of the smart contract on the Ethereum blockchain using the supplied credentials and constructor parameters.

The `<initialValue>` parameter is only required if your smart contract accepts Ether on construction.

If you wish to construct an instance of a wrapper for an existing smart contract, pass in its address:

```java
YourSmartContract contract = YourSmartContract.load(
        "0x<address>|<ensName>", web3j, credentials, contractGasProvider);
```

Solidity smart contract wrappers
--------------------------------

Web3j supports the auto-generation of smart contract function wrappers in Java from Solidity ABI files.

The Web3j [Command Line Tools](../command_line_tools.md) ship with a command line utility for generating smart contract function wrappers:

```bash
$ web3j generate solidity [-hV] [-jt] [-st] -a=<abiFile> [-b=<binFile>] -o=<destinationFileDir> -p=<packageName>
```

`<binFile>` is required for [Contract validity](contract_validity.md).

The generator supports modern ABI features including:

- ABIv2 structs
- custom Solidity error definitions
- generated-code annotations where supported by the active JDK and generator configuration

You can also generate wrappers by calling the Java class directly:

```bash
org.web3j.codegen.SolidityFunctionWrapperGenerator \
  -b /path/to/<smart-contract>.bin \
  -a /path/to/<smart-contract>.abi \
  -o /path/to/src/main/java \
  -p com.your.organisation.name
```

The native Java to Solidity type conversions used are detailed in [Application Binary Interface](application_binary_interface.md).

The generated wrappers support common operations for working with smart contracts:

- [Construction and deployment](construction_and_deployment.md)
- [Invoking transactions and events](interacting_with_smart_contract.md#invoking-transactions-and-events)
- [Calling constant methods](interacting_with_smart_contract.md#calling-constant-methods)
- [Contract validity](contract_validity.md)
