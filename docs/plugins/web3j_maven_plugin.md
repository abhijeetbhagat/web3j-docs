# Web3j-maven-plugin

The Web3j Maven plugin is used to generate Java classes from Solidity contract files.

## Usage

The base configuration takes Solidity files from `src/main/resources` and generates Java classes into `src/main/java`.

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

To run the plugin, execute the goal `generate-sources`:

```bash
mvn web3j:generate-sources
```

## Configuration

There are several variables to select the Solidity source files, define an output path, or change the package name.

| Name | Format | Default value |
| --- | --- | --- |
| `<packageName/>` | valid Java package name | `org.web3j.model` |
| `<outputDirectory><java/></outputDirectory>` | relative or absolute path for generated Java files | value in `<sourceDestination/>` |
| `<outputDirectory><bin/></outputDirectory>` | relative or absolute path for generated BIN files | value in `<sourceDestination/>` |
| `<outputDirectory><abi/></outputDirectory>` | relative or absolute path for generated ABI files | value in `<sourceDestination/>` |
| `<sourceDestination/>` | relative or absolute path of the generated files | `src/main/java` |
| `<outputFormat/>` | `java`, `abi`, and or `bin` | `java` |
| `<nativeJavaType/>` | generates Java native types instead of Solidity types | `true` |
| `<soliditySourceFiles>` | standard Maven fileset | `src/main/resources/**/*.sol` |
| `<contract>` | include or exclude contracts based on the name | none |
| `<pathPrefixes>` | dependency path replacements inside Solidity imports | none |

Configuration of `outputDirectory` has priority over `sourceDestination`.

## Getting Started

Create a standard Java Maven project. Add the following plugin configuration into `pom.xml`:

```xml
<plugin>
    <groupId>org.web3j</groupId>
    <artifactId>web3j-maven-plugin</artifactId>
    <version><!-- plugin version --></version>
    <configuration>
        <packageName>com.example.generated</packageName>
        <sourceDestination>src/main/java/generated</sourceDestination>
        <nativeJavaType>true</nativeJavaType>
        <outputFormat>java,bin</outputFormat>
        <soliditySourceFiles>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.sol</include>
            </includes>
        </soliditySourceFiles>
    </configuration>
</plugin>
```

Add your Solidity contract files into `src/main/resources`. Make sure the files end with `.sol`.

Run:

```bash
mvn web3j:generate-sources
```

You will find the generated Java classes inside `src/main/java/generated/`.

Next, interact with the generated wrappers as described in [Construction and Deployment](../smart_contracts/construction_and_deployment.md).
