# Url class `java.net.URL`

```java
import java.net.URL;

public class URLExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("http://example.com:80/index.html?param=value");
        System.out.println("Protocol: " + url.getProtocol()); // http
        System.out.println("Host: " + url.getHost()); // example.com
        System.out.println("Port: " + url.getPort()); // 80
        System.out.println("Path: " + url.getPath()); // /index.html
        System.out.println("Query: " + url.getQuery()); // param=value
    }
}
```
---

# URLConnection class `java.net.URLConnection`
#### Misol (`HTTP GET` request):
```java
import java.net.URL;
import java.net.URLConnection;
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class URLConnectionExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("http://example.com");
        URLConnection conn = url.openConnection();
        
        // Ma'lumotlarni o‘qish
        BufferedReader reader = new BufferedReader(
            new InputStreamReader(conn.getInputStream())
        );
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }
        reader.close();
    }
}
```
***
#### Misol (`HTTP POST` request):
```java
import java.net.URL;
import java.net.URLConnection;
import java.io.OutputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class URLConnectionPostExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("http://example.com/api");
        URLConnection conn = url.openConnection();
        
        // POST so‘rovi uchun sozlamalar
        conn.setDoOutput(true);
        conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        
        // Ma'lumot yuborish
        String data = "param1=value1&param2=value2";
        try (OutputStream os = conn.getOutputStream()) {
            os.write(data.getBytes());
            os.flush();
        }
        
        // Javobni o‘qish
        BufferedReader reader = new BufferedReader(
            new InputStreamReader(conn.getInputStream())
        );
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }
        reader.close();
    }
}
```
---
#### `HttpURLConnection`
```java
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpURLConnectionExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("http://example.com");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        
        conn.setRequestMethod("GET");
        conn.setRequestProperty("User-Agent", "Mozilla/5.0");
        
        int responseCode = conn.getResponseCode();
        System.out.println("Response Code: " + responseCode);
        
        if (responseCode == HttpURLConnection.HTTP_OK) {
            BufferedReader reader = new BufferedReader(
                new InputStreamReader(conn.getInputStream())
            );
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
            reader.close();
        }
        
        conn.disconnect();
    }
}
```
