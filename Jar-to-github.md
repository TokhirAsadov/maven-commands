# Jar File to Github

#### maven project `pom.xml`
```java
  <distributionManagement>
    <repository>
      <id>github</id>
      <name>Jar file -> GitHub Packages</name>
      <url>https://maven.pkg.github.com/TokhirAsadov/module-5</url>
    </repository>
  </distributionManagement>
```

---

#### Adding to `pom.xml`
```java
<repositories>
    <repository>
      <id>github</id>
      <url>https://maven.pkg.github.com/TokhirAsadov/module-5</url>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
    </repository>
  </repositories>

  <dependencies>
    <dependency>
      <groupId>uz.javatuz</groupId>
      <artifactId>lesson_3_faker</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
  </dependencies>
```
