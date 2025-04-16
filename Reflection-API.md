# Class haqida ma'lumot olish
```java
public class Main {
    public static void main(String[] args) throws ClassNotFoundException {
        // Class obyektini olish
        Class<?> clazz = Class.forName("java.util.ArrayList");

        // Sinf haqida ma'lumot
        System.out.println("Sinf nomi: " + clazz.getName());
        System.out.println("Oddiy nomi: " + clazz.getSimpleName());
        System.out.println("Superklass: " + clazz.getSuperclass().getSimpleName());
        System.out.println("Interfeyslar: ");
        for (Class<?> iface : clazz.getInterfaces()) {
            System.out.println("- " + iface.getSimpleName());
        }
    }
}
```
#### Natija:
```text
Sinf nomi: java.util.ArrayList
Oddiy nomi: ArrayList
Superklass: AbstractList
Interfeyslar: 
- List
- RandomAccess
- Cloneable
- Serializable
```
---
