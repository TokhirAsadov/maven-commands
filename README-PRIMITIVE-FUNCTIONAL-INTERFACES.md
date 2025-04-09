# Primitiv Funktsional Interfeyslar (Primitive Functional Interfaces)

Java’da `java.util.function` paketida primitiv turlar (`int`, `long`, `double`) bilan ishlash uchun maxsus funktsional interfeyslar mavjud. Ular umumiy interfeyslarning optimallashtirilgan versiyalari bo‘lib, boxing va unboxing jarayonlarini oldini olish orqali samaradorlikni oshiradi.

Quyida har bir primitiv funktsional interfeys turi, ularning abstrakt metodlari va qisqacha maqsadlari keltirilgan.

---

## 1. Primitiv `Predicate`lar
Shart tekshirish uchun ishlatiladi.

| Interfeys         | Metod                     | Maqsad                          |
|-------------------|---------------------------|---------------------------------|
| `IntPredicate`    | `boolean test(int value)` | `int` qiymat uchun shart tekshiradi |
| `LongPredicate`   | `boolean test(long value)`| `long` qiymat uchun shart tekshiradi |
| `DoublePredicate` | `boolean test(double value)` | `double` qiymat uchun shart tekshiradi |

### Misol:
```java
IntPredicate isPositive = x -> x > 0;
System.out.println(isPositive.test(5)); // true
```

---

## 2. Primitiv `Function`lar
Primitiv va umumiy turlar o‘rtasida transformatsiya uchun ishlatiladi.

| Interfeys            | Metod                        | Maqsad                              |
|----------------------|------------------------------|-------------------------------------|
| `IntFunction<R>`     | `R apply(int value)`         | `int`dan umumiy turga (`R`)         |
| `LongFunction<R>`    | `R apply(long value)`        | `long`dan umumiy turga (`R`)        |
| `DoubleFunction<R>`  | `R apply(double value)`      | `double`dan umumiy turga (`R`)      |
| `ToIntFunction<T>`   | `int applyAsInt(T value)`    | Umumiy turdan (`T`) `int`ga         |
| `ToLongFunction<T>`  | `long applyAsLong(T value)`  | Umumiy turdan (`T`) `long`ga        |
| `ToDoubleFunction<T>`| `double applyAsDouble(T value)` | Umumiy turdan (`T`) `double`ga   |
| `IntToLongFunction`  | `long applyAsLong(int value)`| `int`dan `long`ga                  |
| `IntToDoubleFunction`| `double applyAsDouble(int value)` | `int`dan `double`ga           |
| `LongToIntFunction`  | `int applyAsInt(long value)` | `long`dan `int`ga                  |
| `LongToDoubleFunction`| `double applyAsDouble(long value)` | `long`dan `double`ga        |
| `DoubleToIntFunction`| `int applyAsInt(double value)` | `double`dan `int`ga             |
| `DoubleToLongFunction`| `long applyAsLong(double value)` | `double`dan `long`ga          |

### Misol:
```java
ToIntFunction<String> length = s -> s.length();
System.out.println(length.applyAsInt("Java")); // 4
```

---

## 3. Primitiv `Consumer`lar
Primitiv qiymatlarni qabul qilib, natija qaytarmasdan amal bajaradi.

| Interfeys       | Metod                  | Maqsad                          |
|-----------------|------------------------|---------------------------------|
| `IntConsumer`   | `void accept(int value)` | `int` qiymatni qayta ishlaydi |
| `LongConsumer`  | `void accept(long value)`| `long` qiymatni qayta ishlaydi |
| `DoubleConsumer`| `void accept(double value)` | `double` qiymatni qayta ishlaydi |

### Misol:
```java
IntConsumer printer = x -> System.out.println(x);
printer.accept(10); // 10
```

---

## 4. Primitiv `Supplier`lar
Primitiv qiymat qaytaradi, kirish qabul qilmaydi.

| Interfeys       | Metod                  | Maqsad                          |
|-----------------|------------------------|---------------------------------|
| `IntSupplier`   | `int getAsInt()`       | `int` qiymat qaytaradi         |
| `LongSupplier`  | `long getAsLong()`     | `long` qiymat qaytaradi        |
| `DoubleSupplier`| `double getAsDouble()` | `double` qiymat qaytaradi      |

### Misol:
```java
DoubleSupplier random = () -> Math.random();
System.out.println(random.getAsDouble()); // Tasodifiy double (masalan, 0.73)
```

---

## 5. Primitiv `Operator`lar
Primitiv qiymatlarga unary (bitta operand) yoki binary (ikkita operand) operatsiyalar qo‘llaydi.

| Interfeys            | Metod                           | Maqsad                              |
|----------------------|---------------------------------|-------------------------------------|
| `IntUnaryOperator`   | `int applyAsInt(int operand)`   | `int`ga unary operatsiya            |
| `LongUnaryOperator`  | `long applyAsLong(long operand)`| `long`ga unary operatsiya           |
| `DoubleUnaryOperator`| `double applyAsDouble(double operand)` | `double`ga unary operatsiya  |
| `IntBinaryOperator`  | `int applyAsInt(int left, int right)` | `int`ga binary operatsiya     |
| `LongBinaryOperator` | `long applyAsLong(long left, long right)` | `long`ga binary operatsiya |
| `DoubleBinaryOperator`| `double applyAsDouble(double left, double right)` | `double`ga binary operatsiya |

### Misol:
```java
IntBinaryOperator adder = (a, b) -> a + b;
System.out.println(adder.applyAsInt(3, 5)); // 8
```

---

## Foydalanish
- **Stream API**: `mapToInt`, `mapToLong`, `mapToDouble` kabi metodlar bilan.
- **Massivlar bilan**: Primitiv massiv elementlarini qayta ishlashda.
- **Performans**: Boxing/unboxingni oldini olish uchun.

## Afzalliklari
- Boxing va unboxing xarajatlari yo‘q.
- Primitiv turlar bilan to‘g‘ridan-to‘g‘ri ishlash.
- Stream API bilan yuqori darajada moslashuvchanlik.

## Kamchiliklari
- Faqat `int`, `long`, `double` turlari bilan cheklangan.
- Har bir tur uchun alohida interfeyslar mavjudligi chalkashlik keltirishi mumkin.

---
