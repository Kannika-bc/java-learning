# Apache Maven – Terminologies

Apache Maven is a build automation and project management tool mainly used in Java projects.

In simple words, Maven helps you:

- Manage external libraries (dependencies).
- Compile Java code.
- Run tests.
- Package your application.
- Manage project versions.
- Build projects without manually handling everything.

Let's understand the important terminologies one by one.

## 1. POM (Project Object Model)

Most important

- POM stands for Project Object Model.
- It is an XML file named `pom.xml`.
- It contains information about your project and how Maven should build it.

For example:

```
<project>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>myapp</artifactId>
    <version>1.0.0</version>

</project>
```

Think of `pom.xml` as the configuration file of your Java project.

It tells Maven:

- Project name and version.
- Required dependencies.
- Plugins.
- Build configuration.

## 2. Dependency

A dependency is an external library that your project needs to work.

For example, suppose you want to build a Spring Boot application. You might need:

- Spring Web
- Spring Data JPA
- MySQL Connector
- Lombok

Instead of downloading JAR files manually, Maven downloads them for you.

Example:

```
<dependencies>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>3.5.0</version>
    </dependency>

</dependencies>
```

Remember: Dependencies are libraries your application uses.

## 3. Repository

A repository is a storage location where Maven keeps dependencies and plugins.

There are three main types.

A. Local Repository

- Located on your computer.
- Stores dependencies already downloaded.
- Default location: `~/.m2/repository`

B. Central Repository

- A public online repository.
- Maven downloads dependencies from here when needed.
- Website: [Maven Central](https://central.sonatype.com/?utm_source=chatgpt.com)

C. Remote Repository

- An additional repository hosted on a remote server.
- Companies may maintain private repositories for internal libraries.

[Maven Basics](https://images.openai.com/static-rsc-4/V2rp35zGO4_DyXXqqhIReFonuhMO7FZUPQgjXOL3x8js3e9734u4ISR2Y76SKJCv4DakHVfpDJSBrsN8eV2adLJBP4dYStbqzWuw4DQFOlweUXHaGBPQvC9HsTHOiXFp3zvU4QDrg-4CXYtq11_VYfvJ6LdZ7xHnAKwvC-MHHWE?purpose=inline)

[datadisk.co.uk](https://www.datadisk.co.uk/html_docs/maven/maven_2.html?utm_source=chatgpt.com)

## 4. GroupId

Identifies the organisation or group that created the project.

Example:

```
<groupId>com.google</groupId>
```

Think of it as the organisation's identity.

## 5. ArtifactId

Identifies the actual project or library.

Example:

```
<artifactId>payment-service</artifactId>
```

Think of it as the project name.

## 6. Version

Specifies which version of the project or dependency you are using.

```
<version>1.0.0</version>
```

Why is it important?

Different versions may have different features, bug fixes or compatibility.

## 7. Artifact

An artifact is the output produced by a Maven build.

For example:

- JAR file
- WAR file

If Maven builds your project, it might generate:

`payment-service-1.0.0.jar`

This is an artifact.

## 8. Lifecycle

Important for interviews

A lifecycle is a sequence of stages Maven follows to build your project.

The most commonly used lifecycle phases:

| Phase    | What it does                              |
| -------- | ----------------------------------------- |
| validate | Checks whether the project is valid       |
| compile  | Compiles Java source code                 |
| test     | Runs unit tests                           |
| package  | Creates JAR or WAR                        |
| verify   | Performs additional checks                |
| install  | Stores the artifact in local repository   |
| deploy   | Publishes artifact to a remote repository |

Important: These phases execute in order.

For example:

```
mvn package
```

Maven executes all the preceding phases, including validate, compile and test, before packaging.

## 9. Plugin

A plugin is a tool that helps Maven perform a particular task.

Examples:

- Compiler Plugin – Compiles Java code.
- Surefire Plugin – Runs unit tests.
- Spring Boot Maven Plugin – Packages and runs Spring Boot applications.

Think of plugins as tools Maven uses to perform different jobs.

## 10. Goal

A goal is a specific task performed by a plugin.

Example:

```
mvn compiler:compile
```

Here:

- `compiler` = plugin.
- `compile` = goal.

Difference:

- Plugin = Collection of related tasks.
- Goal = One specific task.

## 11. Build

Build means converting your source code into an executable or distributable output.

For example:

```
mvn clean package
```

This cleans old build files and creates a new JAR/WAR package.

## 12. Clean

Clean removes previously generated build files.

```
mvn clean
```

It removes the `target` directory by default.

## 13. Snapshot

A snapshot is a version that is still under development.

Example:

```
<version>1.0-SNAPSHOT</version>
```

It indicates that the version is not considered a final release.

## 14. Transitive Dependencies

Suppose:

- Your project uses Library A.
- Library A internally requires Library B.

You only declare Library A, but Maven also downloads Library B.

This is called a transitive dependency.

[java - IntelliJ: How to make Maven project use only the dependencies show in pom.xml file - Stack Overflow](https://images.openai.com/static-rsc-4/Y2PKueiDpcPoF-8gdc5Vi_imqonW_-PojYDTXwuXxkcwqqwbSAQnijrVbEwWWPUvKe6I2oiVn6jz8bpHj0UmEJw8vxSefLdQmFTI2ZZwh6ADAN3s441Np7QruheWvqEtRdlRGjihujeLl1tOhlJeY5igzcTiCYYDNFPmM8gprG0?purpose=inline)

[stackoverflow.com](https://stackoverflow.com/questions/41632198/intellij-how-to-make-maven-project-use-only-the-dependencies-show-in-pom-xml-fi?utm_source=chatgpt.com)

## Important Maven commands for revision

| Command               | Purpose                            |
| --------------------- | ---------------------------------- |
| `mvn clean`           | Removes old build files            |
| `mvn compile`         | Compiles Java code                 |
| `mvn test`            | Runs tests                         |
| `mvn package`         | Creates JAR/WAR                    |
| `mvn install`         | Saves artifact in local repository |
| `mvn deploy`          | Publishes artifact remotely        |
| `mvn clean install`   | Cleans, builds, tests and installs |
| `mvn dependency:tree` | Displays dependency hierarchy      |
