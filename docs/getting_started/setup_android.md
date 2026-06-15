Setting up Project for Android
--------------------------------------------------------

In this article, we’ll walk through the steps to set up Web3j for Android development.

> Web3j 5.x Java artifacts are compiled for Java 21. For Android, use the latest Android-tagged artifact published by the project: `{{ android.version }}`.

## Step 1: Add Web3j Dependency

### Using Maven

Add the following dependency to your `pom.xml` file:

```xml
<dependency>
    <groupId>org.web3j</groupId>
    <artifactId>core</artifactId>
    <version>{{ android.version }}</version>
</dependency>
```

### Using Gradle (Kotlin)

Add the Web3j dependency to your `build.gradle.kts` file:

```kotlin
dependencies {
    implementation("org.web3j:core:{{ android.version }}")
}
```

## Step 2: Update Packaging Options

To avoid conflicts with certain files included in the Web3j library, exclude the additional resources shown below:

```kotlin
android {
    packagingOptions {
        resources {
            excludes += "/META-INF/DISCLAIMER"
        }
    }
}
```

If you are starting a new JVM project rather than an Android project, use the Java 5.x coordinates shown in [Manual Configuration](manual_configuration.md).
