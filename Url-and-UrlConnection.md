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
