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
