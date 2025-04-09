# Method Reference Misollari

Bu bo‘limda Java’dagi **Method Reference** va xususan **Constructor as Method Reference** uchun amaliy misollar keltiriladi. Method Reference lambda ifodalarini soddalashtirish uchun ishlatiladi va `::` operatori yordamida amalga oshiriladi.

---

## 1. Statik metodga ishora (Reference to a Static Method)
Statik metodlarga class nomi orqali ishora qilinadi.

```java
import java.util.function.Function;

public class StaticMethodReference {
    public static void main(String[] args) {
        // Lambda bilan
        Function<String, Integer> lambda = s -> Integer.parseInt(s);
        // Method reference bilan
        Function<String, Integer> methodRef = Integer::parseInt;

        System.out.println(methodRef.apply("123")); // 123
    }
}
```
**Sharh**: `Integer::parseInt` statik metodni chaqiradi va `String`ni `Integer`ga aylantiradi.

---

## 2. Muayyan ob’ektning metodiga ishora (Reference to an Instance Method of a Particular Object)
Muayyan ob’ektning instans metodiga ishora qilinadi.

```java
import java.util.function.Consumer;

public class InstanceMethodReference {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();

        // Lambda bilan
        Consumer<String> lambda = s -> sb.append(s);
        // Method reference bilan
        Consumer<String> methodRef = sb::append;

        methodRef.accept("Hello"); // sb ga "Hello" qo‘shiladi
        System.out.println(sb); // Hello
    }
}
```
**Sharh**: `sb::append` faqat `sb` ob’ektining `append` metodiga ishora qiladi.

---

## 3. Turdagi obyektning metodiga ishora (Reference to an Instance Method of an Arbitrary Object)
Muayyan class turidagi har qanday ob’ektning metodiga ishora qilinadi.

```java
import java.util.function.Function;

public class ArbitraryInstanceMethodReference {
    public static void main(String[] args) {
        // Lambda bilan
        Function<String, Integer> lambda = s -> s.length();
        // Method reference bilan
        Function<String, Integer> methodRef = String::length;

        System.out.println(methodRef.apply("Java")); // 4
    }
}
```
**Sharh**: `String::length` har qanday `String` ob’ektining `length` metodini chaqiradi.

---

## 4. Konstruktorga ishora (Reference to a Constructor)
Konstruktorlarga `ClassName::new` sintaksisi bilan ishora qilinadi.

### 4.1. Parametrsiz konstruktor (`Supplier` bilan)
```java
import java.util.function.Supplier;

class Person {
    String name;

    Person() {
        this.name = "Default";
    }

    @Override
    public String toString() {
        return name;
    }
}

public class NoArgsConstructorReference {
    public static void main(String[] args) {
        // Lambda bilan
        Supplier<Person> lambda = () -> new Person();
        // Method reference bilan
        Supplier<Person> methodRef = Person::new;

        Person person = methodRef.get();
        System.out.println(person); // Default
    }
}
```
**Sharh**: `Person::new` parametrsiz konstruktor bilan yangi `Person` ob’ektini yaratadi.

---

### 4.2. Bitta parametrli konstruktor (`Function` bilan)
```java
import java.util.function.Function;

class Person {
    String name;

    Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return name;
    }
}

public class OneArgConstructorReference {
    public static void main(String[] args) {
        // Lambda bilan
        Function<String, Person> lambda = n -> new Person(n);
        // Method reference bilan
        Function<String, Person> methodRef = Person::new;

        Person person = methodRef.apply("Ali");
        System.out.println(person); // Ali
    }
}
```
**Sharh**: `Person::new` bitta `String` parametri bilan konstruktor chaqiradi.

---

### 4.3. Ikkita parametrli konstruktor (`BiFunction` bilan)
```java
import java.util.function.BiFunction;

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class TwoArgsConstructorReference {
    public static void main(String[] args) {
        // Lambda bilan
        BiFunction<String, Integer, Person> lambda = (n, a) -> new Person(n, a);
        // Method reference bilan
        BiFunction<String, Integer, Person> methodRef = Person::new;

        Person person = methodRef.apply("Bob", 25);
        System.out.println(person); // Bob (25)
    }
}
```
**Sharh**: `Person::new` ikkita parametrli konstruktor bilan `Person` ob’ektini yaratadi.

---

### 4.4. Stream bilan konstruktor ishorasi
```java
import java.util.List;
import java.util.stream.Collectors;

class Person {
    String name;

    Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return name;
    }
}

public class StreamConstructorReference {
    public static void main(String[] args) {
        List<String> names = List.of("Ali", "Bob", "Charlie");

        List<Person> people = names.stream()
                                  .map(Person::new)
                                  .collect(Collectors.toList());

        people.forEach(System.out::println);
        // Natija:
        // Ali
        // Bob
        // Charlie
    }
}
```
**Sharh**: `map` har bir `String` uchun `Person::new` konstruktorini chaqirib, `Person` ob’ektlar ro‘yxatini hosil qiladi.

---

### 4.5. Massiv konstruktorlari
```java
import java.util.function.IntFunction;
import java.util.Arrays;

public class ArrayConstructorReference {
    public static void main(String[] args) {
        // Lambda bilan
        IntFunction<String[]> lambda = size -> new String[size];
        // Method reference bilan
        IntFunction<String[]> methodRef = String[]::new;

        String[] array = methodRef.apply(3);
        array[0] = "A";
        array[1] = "B";
        array[2] = "C";
        System.out.println(Arrays.toString(array)); // [A, B, C]
    }
}
```
**Sharh**: `String[]::new` berilgan uzunlikdagi `String` massivini yaratadi.
