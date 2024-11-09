# Conventional Release Gradle Plugin

[![Maven Central](https://maven-badges.herokuapp.com/maven-central/se.bjurr.gradle.conventional-release/se.bjurr.gradle.conventional-release.gradle.plugin/badge.svg)](https://search.maven.org/artifact/se.bjurr.gradle.conventional-release/se.bjurr.gradle.conventional-release.gradle.plugin)

Bundle of plugins and some Gradle DSL that can:

- Get version from [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/)
- Tag commit
- Update `CHANGELOG.md`
- Publish JAR:s to Maven Central
- Publish command line tools to Maven Central and NPM
- Publish Gradle plugins to Gradle Plugin Portal or Central

## Usage

Apply it with:

```groovy
plugins {
 id "se.bjurr.gradle.conventional-release" version "X"
}
```

Run it with:

```groovy
./gradlew release
```

I also have other similar plugins:

- <https://github.com/tomasbjerre/java-convention-gradle-plugin>
- <https://github.com/tomasbjerre/update-versions-gradle-plugin>

I usually do something like this in a Java project:

`build.gradle`:

```groovy
plugins {
 id "se.bjurr.gradle.conventional-release" version "0.+"
 id "se.bjurr.gradle.java-convention" version "0.+"
 id "se.bjurr.gradle.update-versions" version "0.+"
}
```

`settings.gradle`:

```groovy
pluginManagement {
 repositories {
  mavenLocal()
  mavenCentral()
  gradlePluginPortal()
 }
}
```

## Publish JAR:s

The default setting, no need to change any properties.

Will publish the JAR to [Central](https://central.sonatype.com/).

## Publish command line tools

In `gradle.properties`:

```properties
repoType=COMMAND
```

Will publish the JAR to [Central](https://central.sonatype.com/).

Will package the JAR inside an NPM package and publish that to [NPM](https://plugins.gradle.org/).

## Publish Gradle plugins

In `gradle.properties`:

```properties
repoType=GRADLE
tags=tag1,tag2,tag3
implementationClass=a.b.c.ImplClass
```

Will publish the plugin to [Plugin Portal](https://plugins.gradle.org/).

## Properties

It can be tweaked with `conventionalRelease { /* config here*/ }`:

```groovy
// ---- default config ----

// ---- default config ----
```

## Requirements

Gradle wrapper can be downloaded with:

```sh
cat > gradle/wrapper/gradle-wrapper.properties << EOL
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-8.10.2-bin.zip
networkTimeout=10000
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
EOL

./gradlew wrapper \
 --gradle-version=8.10.2 \
 --distribution-type=bin
```

In many cases you can just run the `gradlew wrapper` task. But there are cases where this does not work. On such case is if you are using Java version X and the current wrapper only support version Y, the current wrapper cannot run. Se example error below:

```sh
$ java -version

openjdk version "21.0.2" 2024-01-16
OpenJDK Runtime Environment (build 21.0.2+13-Ubuntu-122.04.1)
OpenJDK 64-Bit Server VM (build 21.0.2+13-Ubuntu-122.04.1, mixed mode, sharing)
```

```sh
$ ./gradlew wrapper --gradle-version=8.6 --distribution-type=bin

Downloading https://X/distributions/gradle-6.8.3-bin.zip

...

FAILURE: Build failed with an exception.

* Where:
Build file 'X/build.gradle'

* What went wrong:
Could not compile build file 'X/build.gradle'.
> startup failed:
  General error during semantic analysis: Unsupported class file major version 65

  java.lang.IllegalArgumentException: Unsupported class file major version 65
        at groovyjarjarasm.asm.ClassReader.<init>(ClassReader.java:196)
```
