# Adding Dependencies

## Learning Goals

- Add dependencies to a Maven project.
- Explain dependency scope.

## Introduction

We will adding the JUnit library to our project so we can write and run tests in
later lessons. Let’s look at what a dependency is before we add one to our
project.

## What are Dependencies?

Any non-trivial program uses external drivers, libraries, and frameworks. These
external resources are called dependencies because your program depends on them
to run properly.

Maven has a declarative dependency management system. We can define which
dependency we want in the `pom.xml` file and Maven will download them
automatically. Maven downloads these dependencies to the
[local repository](https://maven.apache.org/guides/introduction/introduction-to-repositories.html#artifact-repositories)
and references them in the project by automatically setting the correct the
[Java Classpath](https://docs.oracle.com/javase/tutorial/essential/environment/paths.html).

## Add a Test

We’ll be adding a unit test to our project and then we’ll learn how to add the
dependencies to make it work. Create an `AppTest.java` file in the
`src/test/java/org/example` directory.

```bash
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── org
    │   │       └── example
    │   │           └── App.java
    └── test
        └── java
            └── org
                └── example
                    └── AppTest.java
```

Write the following code in the file:

```java
package org.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class AppTest
{
    @Test
    public void greetShoudReturnCorrectString() {
        String expected = "Hello, John!";
        String actual = App.greet("John");
        assertEquals(expected, actual);
    }
}
```

If you try to run the test, you’ll get an error saying that the `junit` library
can’t be found.

## Add a Dependency

We can add dependencies using coordinates of the external resource. Here’s what
the `pom.xml` file would look like if we add JUnit:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>
  <artifactId>example-app</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>example-app</name>
  <url>http://www.example.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
  </properties>

  <dependencies>
		<!-- START NEW SECTION -->
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.8.2</version>
      <scope>test</scope>
    </dependency>
		<!-- END NEW SECTION -->
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.10.1</version>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>3.2.2</version>
        <configuration>
          <archive>
            <manifest>
              <mainClass>org.example.App</mainClass>
            </manifest>
          </archive>
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.0.0-M7</version>
      </plugin>
    </plugins>
  </build>
</project>
```

We’ve added the following lines to the POM file:

```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-api</artifactId>
  <version>5.8.2</version>
  <scope>test</scope>
</dependency>
```

Notice that we’re using the groupId, artifactId, and version tags to uniquely
identify which external resource we want to add to our project. Maven will
automatically download and set up JUnit and we’ll have access to all JUnit
methods in our test file.

We do have to add another plugin called the `maven-surefire-plugin` in order to
run the tests but the library can still be referenced in our code with just the
dependency added.

## Scope

You may have noticed the new `<scope>test</scope>` tag in the previous section.
The scope tag defines at what points in the program’s execution they’ll be
available. For example, the `test` scope means that the dependencies will only
be used for testing and not for compilation or during runtime.

By default, dependencies have a `compile` scope. This means that the
dependencies are available at compile time, for testing, and at runtime. You can
check out the available scopes in the
[official documentation](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Scope).

## Conclusion

We’ve learned how to add dependencies to our project. Maven makes it incredibly
easy to add dependencies since it uses a declarative dependency management
system which can automatically download and set up dependencies.
