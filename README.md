# Maven Buyruqlari Ro‘yxati

Quyida Apache Mavenning asosiy va foydali buyruqlari jadval ko‘rinishida keltirilgan. Har bir buyruqning qisqacha tushuntirishi bilan tanishishingiz mumkin.

| **Buyruq**                  | **Tavsif**                                                                 |
|-----------------------------|---------------------------------------------------------------------------|
| `mvn clean`                 | Loyihaning avvalgi qurilish natijalarini (`target` papkasini) tozalaydi.   |
| `mvn compile`               | Java kodini kompilyatsiya qiladi (`src/main/java` ichidagi fayllar).       |
| `mvn test`                  | Unit testlarni ishga tushiradi (`src/test/java` ichidagi fayllar).         |
| `mvn package`               | Loyihani kompilyatsiya qiladi, testlardan o‘tkazadi va paketlaydi (JAR/WAR). |
| `mvn install`               | Paketlangan loyihani mahalliy repozitoriyga (`.m2`) o‘rnatadi.             |
| `mvn deploy`                | Paketlangan loyihani tashqi repozitoriyga yuklaydi.                        |
| `mvn validate`              | POM faylining to‘g‘riligini tekshiradi.                                   |
| `mvn verify`                | Testlardan o‘tgan qadoqlangan faylning to‘g‘riligini tekshiradi.           |
| `mvn site`                  | Loyiha uchun hujjatlar saytini yaratadi.                                  |
| `mvn dependency:tree`       | Bog‘liqliklar daraxtini ko‘rsatadi.                                       |
| `mvn dependency:resolve`    | Bog‘liqliklarni yuklaydi, lekin qurilishni amalga oshirmaydi.              |
| `mvn dependency:analyze`    | Bog‘liqliklarda muammolarni tahlil qiladi.                                |
| `mvn archetype:generate`    | Yangi loyihani shablon asosida yaratadi.                                  |
| `mvn help:describe`         | Buyruq yoki plagin haqida ma’lumot beradi.                                |
| `mvn help:effective-pom`    | POM faylining to‘liq versiyasini ko‘rsatadi.                              |
| `mvn exec:java`             | Java sinfini ishga tushiradi (main metod bilan).                          |
| `mvn versions:set`          | Loyiha versiyasini yangilaydi.                                            |
| `mvn test-compile`          | Faqat test kodlarini kompilyatsiya qiladi.                                |
| `mvn -X`                    | Debug rejimida ishlaydi, batafsil loglarni chiqaradi.                     |
| `mvn -DskipTests`           | Testlarni o‘tkazib yuborib, qurilishni amalga oshiradi.                   |

## Foydalanish Misollari

- Yangi loyiha yaratish:
  ```bash
  mvn archetype:generate -DgroupId=com.example -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart
