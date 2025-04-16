# Class haqida ma'lumot olish
```java
public class Main {
    public static void main(String[] args) throws ClassNotFoundException {
        // Class obyektini olish
        Class<?> clazz = Class.forName("java.util.ArrayList");

        // Sinf haqida ma'lumot
        System.out.println("Class nomi: " + clazz.getName());
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
Class nomi: java.util.ArrayList
Oddiy nomi: ArrayList
Superklass: AbstractList
Interfeyslar: 
- List
- RandomAccess
- Cloneable
- Serializable
```
---
# Fields'ga kirish
```java
import java.lang.reflect.Field;

public class Main {
    public static void main(String[] args) throws Exception {
        // Person sinfi
        Class<?> clazz = Person.class;
        Person person = new Person("Ali", 25);

        // Maydonlarni olish
        Field nameField = clazz.getDeclaredField("name");
        Field ageField = clazz.getDeclaredField("age");

        // Private maydonlarga kirish uchun setAccessible(true)
        nameField.setAccessible(true);
        ageField.setAccessible(true);

        // Maydon qiymatlarini o‘qish
        System.out.println("Name: " + nameField.get(person));
        System.out.println("Age: " + ageField.get(person));

        // Maydon qiymatlarini o‘zgartirish
        nameField.set(person, "Vali");
        ageField.set(person, 30);

        System.out.println("Yangilangan: " + person);
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}
```
#### Natija:
```text
Name: Ali
Age: 25
Yangilangan: Person{name='Vali', age=30}
```
Tushuntirish:
- `getDeclaredField()` faqat ko‘rsatilgan maydonni qaytaradi.
- `setAccessible(true)` `private` field'larga kirish uchun ishlatiladi.
- `get()` va `set()` metodlari maydon qiymatlarini o‘qish va o‘zgartirish uchun ishlatiladi.
---
