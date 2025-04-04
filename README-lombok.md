# Project Lombok Annotatsiyalari Ro‘yxati

Quyida Project Lombokning barcha asosiy annotatsiyalari jadval ko‘rinishida keltirilgan. Har bir annotatsiyaning qisqacha tavsifi va qo‘llanilishi haqida ma’lumot olishingiz mumkin.

| **Annotatsiya**            | **Tavsif**                                                                                   |
|----------------------------|---------------------------------------------------------------------------------------------|
| `@Getter`                  | Har bir maydon (field) uchun `getXxx()` metodini generatsiya qiladi.                         |
| `@Setter`                  | Har bir maydon uchun `setXxx()` metodini generatsiya qiladi (faqat `final` bo‘lmaganlar uchun). |
| `@ToString`                | `toString()` metodini generatsiya qiladi, sinfning barcha maydonlarini qaytaradi.           |
| `@EqualsAndHashCode`       | `equals()` va `hashCode()` metodlarini generatsiya qiladi, maydonlar asosida solishtiradi.  |
| `@NoArgsConstructor`       | Parametrsiz (bo‘sh) konstruktor generatsiya qiladi.                                          |
| `@AllArgsConstructor`      | Barcha maydonlarni parametr sifatida qabul qiluvchi konstruktor generatsiya qiladi.          |
| `@RequiredArgsConstructor` | `final` yoki `@NonNull` bilan belgilangan maydonlar uchun konstruktor generatsiya qiladi.    |
| `@Data`                    | `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode` va `@RequiredArgsConstructor` ni birlashtiradi. |
| `@Value`                   | Sinfni immutable (o‘zgarmas) qiladi: `@Getter`, `@ToString`, `@EqualsAndHashCode` va `@AllArgsConstructor` qo‘shadi, lekin setter’lar yo‘q. |
| `@Builder`                 | Builder pattern’ni qo‘llab-quvvatlaydi, ob’ektlarni qadam-baqadam yaratish imkonini beradi.  |
| `@Singular`                | `@Builder` bilan birga ishlatiladi, kolleksiyalarni (List, Set) qulay boshqarish uchun.       |
| `@NonNull`                 | Parametr yoki maydon null bo‘lmasligini tekshiradi, agar null bo‘lsa, `NullPointerException` chiqaradi. |
| `@Accessors`               | Getter va setter’larning xatti-harakatlarini sozlaydi (masalan, fluent API uchun).           |
| `@Synchronized`            | Metodni sinxronlashtiradi, `synchronized` kalit so‘zi kabi ishlaydi, lekin xavfsizroq.       |
| `@With`                    | Immutable sinflar uchun maydonlarni o‘zgartirib yangi nusxa qaytaruvchi metodlar yaratadi.   |
| `@Cleanup`                 | Resurslarni (masalan, `InputStream`) avtomatik yopadi, `try-with-resources` kabi ishlaydi.   |
| `@SneakyThrows`            | `throws` deklaratsiyasisiz istisnolarni yashirincha tashlashga imkon beradi.                 |
| `@Log`                     | Logging uchun o‘zgaruvchi yaratadi (masalan, `java.util.logging`).                          |
| `@Slf4j`                   | SLF4J logger’ini avtomatik qo‘shadi (`log` nomli o‘zgaruvchi bilan).                        |
| `@CommonsLog`              | Apache Commons Logging uchun logger qo‘shadi.                                               |
| `@JBossLog`                | JBoss Logging uchun logger qo‘shadi.                                                        |
| `@Log4j`                   | Log4j uchun logger qo‘shadi.                                                                |
| `@Log4j2`                  | Log4j2 uchun logger qo‘shadi.                                                               |
| `@XSlf4j`                  | XSLF4J uchun logger qo‘shadi.                                                               |
| `@FieldDefaults`           | Maydonlar uchun standart access modifier’larini belgilaydi (masalan, `private final`).      |
| `@UtilityClass`            | Sinfni faqat statik metodlar uchun ishlatiladigan utilitaga aylantiradi (konstruktor yo‘q).  |
| `@ExtensionMethod`         | Statik metodlarni extension metod sifatida ishlatishga imkon beradi (eksperimental).        |
| `@Delegate`                | Maydonning metodlarini sinfga delegatsiya qiladi (masalan, interfeysni amalga oshirish).     |
| `@FieldNameConstants`      | Maydon nomlarini `static final` konstantalar sifatida generatsiya qiladi (eksperimental).   |

## Foydalanish Misollari

### 1. Oddiy POJO class
```java
import lombok.Data;

@Data
public class User {
    private String name;
    private int age;
}

***

# Lompok dependency
```java
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.32</version>
    <scope>provided</scope>
</dependency>
```
