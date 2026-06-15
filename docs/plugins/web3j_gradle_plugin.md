Web3j Gradle Plugin
===================

Gradle plugin that generates Web3j Java wrappers from Solidity smart contracts.
It integrates with your project's build lifecycle by adding dedicated wrapper-generation tasks.

## Plugin configuration

To configure the Web3j Gradle Plugin using the plugins DSL or the legacy plugin application,
check the [plugin page](https://plugins.gradle.org/plugin/org.web3j).
The minimum Gradle version to run the plugin is `5.+`.

Then run your project containing Solidity contracts:

```
./gradlew build
```

After applying the plugin, the base directory for generated code (by default `$buildDir/generated/sources/web3j`) contains a directory for each source set, usually `main` and `test`.

## Code generation

The `web3j` DSL allows you to configure the generated code:

```groovy
web3j {
    generatedPackageName = 'com.mycompany.{0}'
    generatedFilesBaseDir = "$buildDir/custom/destination"
    excludedContracts = ['Ownable']
    useNativeJavaTypes = false
}
```

The properties accepted by the DSL are listed in the following table:

| Name | Type | Default value | Description |
| --- | :---: | :---: | --- |
| `generatedPackageName` | `String` | `${group}.web3j` or `org.web3j.{0}` | Generated contract wrappers package |
| `generatedFilesBaseDir` | `String` | `$buildDir/generated/sources/web3j` | Generated Java code output directory |
| `excludedContracts` | `String[]` | `[]` | Excluded contract names from wrapper generation |
| `includedContracts` | `String[]` | `[]` | Included contract names from wrapper generation |
| `useNativeJavaTypes` | `Boolean` | `true` | Generate wrappers using native Java types |
| `addressBitLength` | `int` | `160` | Supported address length in bits |

By default, all `.sol` files in `$projectDir/src/main/solidity` are processed by the plugin.

## Plugin tasks

The Solidity plugin adds the `generateContractWrappers` task for the `main` source set, and a `generate[SourceSet]ContractWrappers` task for each remaining source set.

To obtain a list and description of all added tasks, run:

```
./gradlew tasks --all
```

Generated wrappers follow the same conventions as the CLI-based generators, including generated-code annotations where supported by the current codegen module.
