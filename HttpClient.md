# `HttpClient` creating
```java
import java.net.http.HttpClient;
import java.time.Duration;

public class HttpClientCreation {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newBuilder()
                .version(HttpClient.Version.HTTP_2) // HTTP/2 ni afzal ko‘rish
                .connectTimeout(Duration.ofSeconds(10)) // Ulanish timeout
                .followRedirects(HttpClient.Redirect.NORMAL) // Redirect’larni boshqarish
                .build();
    }
}
```
##### Sozlash parametrlari:
- `version(HttpClient.Version)`: HTTP/1.1 yoki HTTP/2 ni tanlash (default: HTTP/2, agar server qo‘llab-quvvatlasa).
- `connectTimeout(Duration)`: Serverga ulanish uchun maksimal vaqt.
- `followRedirects(Redirect)`: Redirect’larni avtomatik kuzatish (NEVER, ALWAYS, NORMAL).
- `proxy(ProxySelector)`: Proxy sozlamalari.
- `authenticator(Authenticator)`: Autentifikatsiya (masalan, Basic Auth).
- `executor(Executor)`: Asinxron so‘rovlar uchun maxsus thread pool.

---
# `HttpRequest` creating
#### Example (`GET` request)
```java
import java.net.URI;
import java.net.http.HttpRequest;

public class HttpRequestExample {
    public static void main(String[] args) {
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://example.com"))
                .GET() // GET metodi
                .header("User-Agent", "Java HttpClient")
                .build();
    }
}
```
#### Example (`POST` request)
```java
import java.net.URI;
import java.net.http.HttpRequest;

public class HttpRequestPostExample {
    public static void main(String[] args) {
        String json = "{\"key\":\"value\"}";
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://example.com/api"))
                .POST(HttpRequest.BodyPublishers.ofString(json)) // JSON body
                .header("Content-Type", "application/json")
                .build();
    }
}
```

##### Asosiy metodlar:
- `uri(URI)`: So‘rov manzilini belgilaydi.
- `GET()`, `POST(BodyPublisher)`, `PUT(BodyPublisher)`, `DELETE()`: HTTP metodini tanlaydi.
- `header(String key, String value)`: So‘rov header’larini qo‘shadi.
- `timeout(Duration)`: So‘rovning javob vaqtini cheklaydi.
- `BodyPublishers`: Body ma'lumotlarini uzatish uchun:
    - `ofString(String)`: Oddiy matn yoki JSON.
    - `ofByteArray(byte[])`: Binary ma'lumotlar.
    - `ofFile(Path)`: Fayldan ma'lumot.
    - `noBody()`: Body bo‘lmagan so‘rovlar uchun.
---
