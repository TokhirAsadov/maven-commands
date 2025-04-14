# Jar File to Github

#### maven project `pom.xml`
```xml
  <distributionManagement>
    <repository>
      <id>github</id>
      <name>Jar file -> GitHub Packages</name>
      <url>https://maven.pkg.github.com/TokhirAsadov/module-5</url>
    </repository>
  </distributionManagement>
```

---
#### GitHub Personal Access Token yaratish:
- [] GitHub'da "Settings" → "Developer settings" → "Personal access tokens" → "Tokens (classic)" bo'limiga o'ting.
- [] Yangi token yarating va write:packages va read:packages ruxsatlarini tanlang.
- [] Tokenni saqlang (masalan, ghp_xxxxxxxxxxxxxxxx).

---


#### `~/.m2/settings.xml`
```xml
<settings>
    <servers>
        <server>
            <id>github</id>
            <username>YOUR_GITHUB_USERNAME</username>
            <password>YOUR_PERSONAL_ACCESS_TOKEN</password>
        </server>
    </servers>
</settings>
```
---
#### `.jar` faylini GitHub Packages'ga nashr qilish:
```bash
mvn deploy
```

#### Adding to `pom.xml`
```xml
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
