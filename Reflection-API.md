# 1.Class haqida ma'lumot olish
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
# 2.Fields'ga kirish
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
# 3.Metodlarni chaqirish
```java
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        // Person sinfi
        Class<?> clazz = Person.class;
        Person person = new Person("Ali", 25);

        // Metodni olish
        Method setNameMethod = clazz.getDeclaredMethod("setName", String.class);
        setNameMethod.setAccessible(true); // Private metod uchun

        // Metodni chaqirish
        setNameMethod.invoke(person, "Vali");

        // Getter metodini chaqirish
        Method getNameMethod = clazz.getDeclaredMethod("getName");
        getNameMethod.setAccessible(true);
        String name = (String) getNameMethod.invoke(person);

        System.out.println("Yangilangan ism: " + name);
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    private String getName() {
        return name;
    }

    private void setName(String name) {
        this.name = name;
    }
}
```
#### Natija:
```text
Yangilangan ism: Vali
```
Tushuntirish:
- `getDeclaredMethod()` metodi aniq metodni (nomi va parametr turlari bo‘yicha) qaytaradi.
- `invoke()` metodi metodni chaqiradi va kerakli argumentlarni qabul qiladi.
---
# 4.Obyekt yaratish (Konstruktorlar)
```java
import java.lang.reflect.Constructor;

public class Main {
    public static void main(String[] args) throws Exception {
        // Person sinfi
        Class<?> clazz = Person.class;

        // Konstruktor olish
        Constructor<?> constructor = clazz.getDeclaredConstructor(String.class, int.class);

        // Obyekt yaratish
        Person person = (Person) constructor.newInstance("Ali", 25);

        System.out.println("Yaratilgan obyekt: " + person);
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
Yaratilgan obyekt: Person{name='Ali', age=25}
```
Tushuntirish:
- `getDeclaredConstructor()` konstruktor topadi.
- `newInstance()` konstruktor orqali yangi obyekt yaratadi.
---
