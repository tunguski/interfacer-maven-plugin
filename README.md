# Interfacer maven plugin

Add interfaces to classes generated by other plugins.

## Why?

Let's suppose that you generate classes from a meta model. It may be avro or
protobuf schemas. Let's suppose that many generated classes have common api
that you would like to use in a generic way. What would be helpful here is
if all of these classes would implement same interface. If a tool that
generates classes does not have the capability, this plugin may help you.

**Simplified example:**

```java
// src/main/java manually defined interface
public interface HasName {
  String getName();
}

// target/generated-sources/avro
public class Person {

  String name;

  public String getName() {
    return name;
  }
  // [...]
}

public class Company {

  String name;

  public String getName() {
    return name;
  }
  // [...]
}

// after this plugin run

// target/generated-sources/avro
public class Person implements HasName {

  String name;

  public String getName() {
    return name;
  }
  // [...]
}

public class Company implements HasName {

  String name;

  public String getName() {
    return name;
  }
  // [...]
}
```

## Maven Usage

```
<plugin>
    <groupId>pl.matsuo.interfacer</groupId>
    <artifactId>interfacer-maven-plugin</artifactId>
    <version>0.0.6</version>
    <executions>
        <execution>
            <configuration>
                <interfacesDirectory>${project.basedir}/src/main/java</interfacesDirectory>
                <interfacePackage>pl.matsuo.interfacer.showcase</interfacePackage>
            </configuration>
            <goals>
                <goal>add-interfaces</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## Gradle Usage

```
plugins {
    id 'pl.matsuo.interfacer'
}

interfacer {
    interfacePackage = 'pl.matsuo.interfacer.showcase'
    interfacesDirectory = file(path_to_custom_source_dir)
    scanDirectory = file(path_to_generated_classes_dir)
}
```

## Development and publishing

**Maven**

```
mvn clean
mvn -P release release:prepare
mvn -P release release:perform
```

**Gradle**

```
cd interfacer-gradle-plugin/
./gradlew publishPlugins
```
