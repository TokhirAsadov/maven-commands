# Java Stream Operatsiyalari va Collectors Misollari

Bu `Readme.md` fileda Java’dagi Stream operatsiyalari va `Collectors` class'ining asosiy funksiyalari misollar bilan tushuntiriladi. Har bir misol qisqa va amaliy bo‘lib, kodning maqsadini aniq ko‘rsatadi.

## Stateless va Stateful Operatsiyalar

### Stateless: `filter`
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);
numbers.stream()
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println);
// Natija: 2, 4
```

### Stateful: `sorted`
```java
List<Integer> numbers = List.of(3, 1, 4, 1, 5);
numbers.stream()
       .sorted()
       .forEach(System.out::println);
// Natija: 1, 1, 3, 4, 5
```

## Truncating Stream

### `limit`
```java
Stream.iterate(0, n -> n + 1)
      .limit(5)
      .forEach(System.out::println);
// Natija: 0, 1, 2, 3, 4
```

### `skip`
```java
List<String> names = List.of("Ali", "Bob", "Charlie", "Dave");
names.stream()
     .skip(2)
     .forEach(System.out::println);
// Natija: Charlie, Dave
```

## Consuming Stream

### `forEach`
```java
List<String> names = List.of("Ali", "Bob", "Charlie");
names.stream()
     .forEach(System.out::println);
// Natija: Ali, Bob, Charlie
```

### `collect`
```java
List<Integer> numbers = List.of(1, 2, 3, 4);
List<Integer> evens = numbers.stream()
                            .filter(n -> n % 2 == 0)
                            .collect(Collectors.toList());
// Natija: [2, 4]
```

## `peek` va `forEach`

### `peek`
```java
List<Integer> numbers = List.of(1, 2, 3);
numbers.stream()
       .peek(n -> System.out.println("Oldin: " + n))
       .filter(n -> n > 1)
       .forEach(n -> System.out.println("Keyin: " + n));
// Natija:
// Oldin: 1
// Oldin: 2
// Keyin: 2
// Oldin: 3
// Keyin: 3
```

### `forEach`
```java
List<String> names = List.of("Ali", "Bob", "Charlie");
names.stream()
     .filter(n -> n.length() > 3)
     .forEach(System.out::println);
// Natija: Charlie
```

## Matching Operations

### `anyMatch`
```java
List<Integer> numbers = List.of(1, 3, 5, 6);
boolean hasEven = numbers.stream()
                        .anyMatch(n -> n % 2 == 0);
// Natija: true
```

### `allMatch`
```java
List<String> names = List.of("Ali", "Bob", "Cat");
boolean allShort = names.stream()
                       .allMatch(n -> n.length() <= 3);
// Natija: true
```

### `noneMatch`
```java
List<Integer> numbers = List.of(1, 3, 5);
boolean noEven = numbers.stream()
                       .noneMatch(n -> n % 2 == 0);
// Natija: true
```

## Mapping Operations

### `map`
```java
List<String> names = List.of("Ali", "Bob");
names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println);
// Natija: ALI, BOB
```

### `mapToInt`
```java
List<String> words = List.of("one", "two");
int sum = words.stream()
               .mapToInt(String::length)
               .sum();
// Natija: 6
```

### `flatMap`
```java
List<List<String>> nested = List.of(List.of("Ali"), List.of("Bob"));
nested.stream()
      .flatMap(List::stream)
      .forEach(System.out::println);
// Natija: Ali, Bob
```

## Finding Element

### `findFirst`
```java
List<Integer> numbers = List.of(1, 3, 5, 6);
Integer firstEven = numbers.stream()
                          .filter(n -> n % 2 == 0)
                          .findFirst()
                          .orElse(null);
// Natija: 6
```

### `findAny`
```java
List<String> names = List.of("Ali", "Bob", "Charlie");
String anyLong = names.stream()
                     .filter(n -> n.length() > 3)
                     .findAny()
                     .orElse(null);
// Natija: Charlie
```

## Parallel Stream

### Parallel `map`
```java
List<Integer> numbers = List.of(1, 2, 3, 4);
numbers.parallelStream()
       .map(n -> n * n)
       .forEach(System.out::println);
// Natija: 1, 4, 9, 16 (tartib noaniq)
```

## Stream Reduction

### `reduce` (boshlang‘ich qiymat bilan)
```java
List<Integer> numbers = List.of(1, 2, 3);
int sum = numbers.stream()
                .reduce(0, Integer::sum);
// Natija: 6
```

### `reduce` (boshlang‘ich qiymatsiz)
```java
List<Integer> numbers = List.of(1, 2, 3);
int max = numbers.stream()
                .reduce(Integer::max)
                .orElse(0);
// Natija: 3
```

## `toArray`

### `toArray()`
```java
List<String> names = List.of("Ali", "Bob");
Object[] array = names.stream()
                     .toArray();
// Natija: [Ali, Bob]
```

### `toArray(generator)`
```java
List<String> names = List.of("Ali", "Bob");
String[] array = names.stream()
                     .toArray(String[]::new);
// Natija: [Ali, Bob]
```

## Infinite Streams

### `iterate`
```java
Stream.iterate(0, n -> n + 2)
      .limit(4)
      .forEach(System.out::println);
// Natija: 0, 2, 4, 6
```

### `generate`
```java
Stream.generate(() -> "Hello")
      .limit(3)
      .forEach(System.out::println);
// Natija: Hello, Hello, Hello
```

## `Comparator`

### `comparing`
```java
List<String> names = List.of("Charlie", "Ali", "Bob");
names.stream()
     .sorted(Comparator.comparing(String::length))
     .forEach(System.out::println);
// Natija: Ali, Bob, Charlie
```

### `thenComparing`
```java
List<String> names = List.of("Bob", "Ali", "Ann");
names.stream()
     .sorted(Comparator.comparing(String::length).thenComparing(Comparator.naturalOrder()))
     .forEach(System.out::println);
// Natija: Ali, Ann, Bob
```

## `Collectors`

### `toList`
```java
List<String> names = List.of("Ali", "Bob");
List<String> result = names.stream()
                          .collect(Collectors.toList());
// Natija: [Ali, Bob]
```

### `toSet`
```java
List<String> names = List.of("Ali", "Ali", "Bob");
Set<String> result = names.stream()
                         .collect(Collectors.toSet());
// Natija: [Ali, Bob]
```

### `toMap`
```java
List<String> names = List.of("Ali", "Bob");
Map<String, Integer> result = names.stream()
                                  .collect(Collectors.toMap(n -> n, String::length));
// Natija: {Ali=3, Bob=3}
```

### `joining`
```java
List<String> words = List.of("Hello", "World");
String result = words.stream()
                    .collect(Collectors.joining(" "));
// Natija: Hello World
```

### `groupingBy`
```java
List<String> names = List.of("Ali", "Bob", "Charlie");
Map<Integer, List<String>> result = names.stream()
                                        .collect(Collectors.groupingBy(String::length));
// Natija: {3=[Ali, Bob], 7=[Charlie]}
```

### `partitioningBy`
```java
List<String> names = List.of("Ali", "Charlie");
Map<Boolean, List<String>> result = names.stream()
                                        .collect(Collectors.partitioningBy(n -> n.length() > 3));
// Natija: {false=[Ali], true=[Charlie]}
```

### `counting`
```java
List<String> names = List.of("Ali", "Bob");
long count = names.stream()
                 .collect(Collectors.counting());
// Natija: 2
```

### `summingInt`
```java
List<String> words = List.of("one", "two");
int sum = words.stream()
               .collect(Collectors.summingInt(String::length));
// Natija: 6
```

### `averagingDouble`
```java
List<Integer> numbers = List.of(1, 2, 3);
double avg = numbers.stream()
                   .collect(Collectors.averagingDouble(n -> n));
// Natija: 2.0
```

### `maxBy`
```java
List<String> names = List.of("Ali", "Charlie");
String longest = names.stream()
                     .collect(Collectors.maxBy(Comparator.comparing(String::length)))
                     .orElse(null);
// Natija: Charlie
```

### `minBy`
```java
List<String> names = List.of("Ali", "Charlie");
String shortest = names.stream()
                      .collect(Collectors.minBy(Comparator.comparing(String::length)))
                      .orElse(null);
// Natija: Ali
```

### `mapping`
```java
List<String> names = List.of("Ali", "Bob");
List<Integer> lengths = names.stream()
                            .collect(Collectors.mapping(String::length, Collectors.toList()));
// Natija: [3, 3]
```

### `reducing`
```java
List<Integer> numbers = List.of(1, 2, 3);
int sum = numbers.stream()
                .collect(Collectors.reducing(0, Integer::sum));
// Natija: 6
```

### `toCollection`
```java
List<String> names = List.of("Ali", "Bob");
TreeSet<String> result = names.stream()
                             .collect(Collectors.toCollection(TreeSet::new));
// Natija: [Ali, Bob] (tartiblangan)
```

---

## Qo‘shimcha eslatmalar
- **Parallelizm**: Parallel Stream’da tartib muhim bo‘lmasa, `forEach` ishlatilishi mumkin, aks holda `forEachOrdered` tanlanadi.
- **Infinite Streams**: Cheklov (`limit`, `takeWhile`) qo‘yilmasa, cheksiz ishlaydi.
- **`toMap`da takrorlanish**: Kalitlar takrorlansa, `mergeFunction` qo‘shish kerak:
  ```java
  Map<String, Integer> result = names.stream()
                                    .collect(Collectors.toMap(n -> n, String::length, (v1, v2) -> v1));
  ```
---

# Collector interface
### User class
```java
package uz.javatuz.entity;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private Integer age;
    private String email;

    public User(String name) {
        this.name = name;
    }
    public User(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
}

```

### ToXmlCollector
```java
import java.util.Set;
import java.util.StringJoiner;
import java.util.function.BiConsumer;
import java.util.function.BinaryOperator;
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.stream.Collector;

public class ToXmlCollector implements Collector<User, StringBuilder, String> {

    @Override
    public Supplier<StringBuilder> supplier() {
        return StringBuilder::new; // Bo‘sh StringBuilder yaratadi
    }

    @Override
    public BiConsumer<StringBuilder, User> accumulator() {
        return (sb, user) -> { // Har bir User’ni XML qatoriga qo‘shadi
            sb.append("<user>")
              .append("<name>").append(user.getName()).append("</name>")
              .append("<age>").append(user.getAge()).append("</age>")
              .append("</user>");
        };
    }

    @Override
    public BinaryOperator<StringBuilder> combiner() {
        return (sb1, sb2) -> { // Ikkita StringBuilder’ni birlashtiradi
            sb1.append(sb2);
            return sb1;
        };
    }

    @Override
    public Function<StringBuilder, String> finisher() {
        return sb -> "<users>" + sb.toString() + "</users>"; // Yakuniy XML hosil qiladi
    }

    @Override
    public Set<Characteristics> characteristics() {
        return Set.of(); // Hech qanday maxsus xususiyat yo‘q
    }

    public static void main(String[] args) {
        List<User> users = List.of(
            new User("Ali", 25),
            new User("Bob", 30)
        );

        String xml = users.stream()
                         .collect(new ToXmlCollector());
        System.out.println(xml);
        // Natija:
        // <users><user><name>Ali</name><age>25</age></user><user><name>Bob</name><age>30</age></user></users>
    }
}
```
