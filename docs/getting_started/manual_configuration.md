Manual Configuration
===============

The simplest way to get started with Web3j is via the [Web3j CLI](../command_line_tools.md).

However, if you wish to configure your project manually, you can follow the steps outlined here.

Add the latest Web3j version to your project build configuration.

## Maven

Java:

```xml
<dependency>
  <groupId>org.web3j</groupId>
  <artifactId>core</artifactId>
  <version>{{ web3j.version }}</version>
</dependency>
```

Android:

```xml
<dependency>
  <groupId>org.web3j</groupId>
  <artifactId>core</artifactId>
  <version>{{ android.version }}</version>
</dependency>
```

## Gradle

Java:

```groovy
implementation("org.web3j:core:{{ web3j.version }}")
```

Android:

```groovy
implementation("org.web3j:core:{{ android.version }}")
```

The Java 5.x artifacts are compiled for Java 21.

The latest Android-tagged release visible from the project tags is `{{ android.version }}`. If you are building an Android app, prefer that artifact instead of the Java 5.x line.

## Plugins

There are also Gradle and Maven plugins to help you generate Web3j Java wrappers for your Solidity smart contracts, allowing you to integrate wrapper generation into your build lifecycle.

Take a look at the project homepage for the [web3j-gradle-plugin](https://github.com/web3j/web3j-gradle-plugin) and [web3j-maven-plugin](https://github.com/web3j/web3j-maven-plugin) for usage details.
