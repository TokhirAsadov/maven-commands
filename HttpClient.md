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
# `HttpResponse` qayta ishlash
#### Example (Sinxron `GET` request):
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class HttpClientSyncExample {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newBuilder().build();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://example.com"))
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

        System.out.println("Status Code: " + response.statusCode());
        System.out.println("Body: " + response.body());
    }
}
```
#### Example (Asinxron `GET` request):
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class HttpClientAsyncExample {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newBuilder().build();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://example.com"))
                .build();

        client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
                .thenApply(HttpResponse::body)
                .thenAccept(System.out::println)
                .join(); // Natijani kutish
    }
}
```
##### `BodyHandlers`:
- `ofString()`: Javobni `String` sifatida qaytaradi.
- `ofByteArray()`: Javobni `byte[]` sifatida qaytaradi.
- `ofFile(Path)`: Javobni `fayl`ga saqlaydi.
- `ofInputStream()`: Javobni `InputStream` sifatida qaytaradi.
- `discarding()`: Javob tanasini e’tiborsiz qoldiradi.
- `ofLines()`: Javobni satrlar oqimi (`Stream<String>`) sifatida qaytaradi.
##### `HttpResponse` metodlari:
- `statusCode()`: HTTP status kodini qaytaradi (`200`, `404` va `h.k`.).
- `body()`: Javob tanasini qaytaradi.
- `headers()`: Javob `header`’larini qaytaradi.
- `uri()`: So‘rov yuborilgan `URI` ni qaytaradi.
- `version()`: Ishlatilgan `HTTP` versiyasini qaytaradi (HTTP/1.1 yoki HTTP/2).
