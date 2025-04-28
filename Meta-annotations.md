# Java Meta-Annotatsiyalari: Batafsil Tushuntirish va Misollar

Meta-annotatsiyalar Java'da maxsus annotatsiyalarni aniqlashda ularning xatti-harakatlarini, qo'llanilish doirasini va saqlanish muddatini belgilash uchun ishlatiladi. Ular `java.lang.annotation` paketida joylashgan bo'lib, quyidagilarni o'z ichiga oladi: `@Retention`, `@Target`, `@Documented`, `@Inherited`, va `@Repeatable`. Quyida har bir meta-annotatsiya, uning har bir holati va misollari batafsil tushuntiriladi.

## 1. `@Retention`
`@Retention` annotatsiyasi maxsus annotatsiyaning qaysi bosqichda saqlanishini belgilaydi. U `RetentionPolicy` enum qiymatlarini qabul qiladi:
- `SOURCE`: Annotatsiya faqat manba kodida saqlanadi, kompilyatsiya vaqtida olib tashlanadi.
- `CLASS`: Annotatsiya bytecode'da saqlanadi, lekin runtime'da odatda o'qilmaydi (standart qiymat).
- `RUNTIME`: Annotatsiya runtime'da reflection API orqali o'qilishi mumkin.

### Misollar
#### `RetentionPolicy.SOURCE`
Annotatsiya faqat manba kodida ishlatiladi, masalan, kodni tahlil qilish yoki hujjatlash uchun.
```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.SOURCE)
public @interface SourceOnly {
    String description();
}

@SourceOnly(description = "Bu class faqat hujjatlash uchun")
class MyClass {
    public void doSomething() {
        System.out.println("Ish bajarildi!");
    }
}
```
**Tushuntirish**: `@SourceOnly` annotatsiyasi kompilyatsiya vaqtida olib tashlanadi, shuning uchun reflection orqali o'qib bo'lmaydi. Bu, masalan, kod generatsiyasi yoki statik tahlil vositalari uchun ishlatiladi (Lombok'dagi `@Getter` kabi).

#### `RetentionPolicy.CLASS`
Annotatsiya bytecode'da saqlanadi, lekin runtime'da odatda mavjud emas.
```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.CLASS)
public @interface ClassOnly {
    String value();
}

@ClassOnly(value = "Bytecode uchun annotatsiya")
class MyClass {
    public void doSomething() {
        System.out.println("Ish bajarildi!");
    }
}
```
**Tushuntirish**: `@ClassOnly` bytecode'da saqlanadi, lekin odatda reflection orqali o'qilmaydi, agar maxsus vositalar (masalan, bytecode manipulyatsiyasi) ishlatilmasa. Bu kamdan-kam ishlatiladi.

#### `RetentionPolicy.RUNTIME`
Annotatsiya runtime'da reflection orqali o'qilishi mumkin.
```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
public @interface RuntimeAnnotation {
    String value();
}

@RuntimeAnnotation(value = "Runtime'da o'qiladi")
class MyClass {
    public void doSomething() {
        System.out.println("Ish bajarildi!");
    }
}

class AnnotationReader {
    public static void main(String[] args) throws Exception {
        Class<?> clazz = MyClass.class;
        RuntimeAnnotation annotation = clazz.getAnnotation(RuntimeAnnotation.class);
        System.out.println("Value: " + annotation.value());
    }
}
```
**Natija**:
```
Value: Runtime'da o'qiladi
```
**Tushuntirish**: `@RuntimeAnnotation` reflection API yordamida runtime'da o'qiladi. Bu eng ko'p ishlatiladigan holat, chunki u frameworklar (Spring, JUnit) va dinamik logikalar uchun mos keladi.

## 2. `@Target`
`@Target` annotatsiyasi maxsus annotatsiyaning qaysi kod elementlariga qo'llanilishi mumkinligini belgilaydi. U `ElementType` enum qiymatlarini qabul qiladi:
- `TYPE`: class, interfeys, enum yoki annotatsiyalarga.
- `FIELD`: O'zgaruvchilarga.
- `METHOD`: Metodlarga.
- `PARAMETER`: Metod parametrlarga.
- `CONSTRUCTOR`: Konstruktorlarga.
- `LOCAL_VARIABLE`: Mahalliy o'zgaruvchilarga.
- `ANNOTATION_TYPE`: Boshqa annotatsiyalarga.
- `PACKAGE`: Paketlarga.
- `TYPE_PARAMETER`: Generik tur parametrlarga (Java 8+).
- `TYPE_USE`: Tur ishlatiladigan joylarda (Java 8+).
- `MODULE`: Modullarga (Java 9+).
- `RECORD_COMPONENT`: Record komponentlariga (Java 14+).

### Misollar
#### `ElementType.TYPE`
class, interfeys yoki enum'larga qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.TYPE)
public @interface TypeAnnotation {
    String value();
}

@TypeAnnotation("class annotatsiyasi")
class MyClass {
    public void doSomething() {
        System.out.println("Ish bajarildi!");
    }
}
```
**Tushuntirish**: `@TypeAnnotation` faqat classlarga qo'llaniladi. Agar uni metod yoki o'zgaruvchiga qo'llashga urinilsa, kompilyatsiya xatosi yuzaga keladi.

#### `ElementType.FIELD`
O'zgaruvchilarga qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.FIELD)
public @interface FieldAnnotation {
    String description();
}

class MyClass {
    @FieldAnnotation(description = "Bu ism maydoni")
    private String name;

    public void doSomething() {
        System.out.println("Ish bajarildi!");
    }
}
```
**Tushuntirish**: `@FieldAnnotation` faqat o'zgaruvchilarga qo'llaniladi, masalan, validatsiya yoki serialization uchun.

#### `ElementType.METHOD`
Metodlarga qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
public @interface MethodAnnotation {
    String priority();
}

class MyClass {
    @MethodAnnotation(priority = "Yuqori")
    public void doSomething() {
        System.out.println("Ish bajarildi!");
    }
}
```
**Tushuntirish**: `@MethodAnnotation` faqat metodlarga qo'llaniladi, masalan, logging yoki testlash uchun.

#### `ElementType.PARAMETER`
Metod parametrlarga qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.PARAMETER)
public @interface ParameterAnnotation {
    String value();
}

class MyClass {
    public void doSomething(@ParameterAnnotation(value = "Mu him parametr") String param) {
        System.out.println("Parametr: " + param);
    }
}
```
**Tushuntirish**: `@ParameterAnnotation` metod parametrlarni validatsiya qilish yoki dokumentatsiya uchun ishlatiladi.

#### `ElementType.CONSTRUCTOR`
Konstruktorlarga qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.CONSTRUCTOR)
public @interface ConstructorAnnotation {
    String value();
}

class MyClass {
    @ConstructorAnnotation("Maxsus konstruktor")
    public MyClass() {
        System.out.println("Obyekt yaratildi!");
    }
}
```
**Tushuntirish**: `@ConstructorAnnotation` konstruktorlarni belgilash uchun ishlatiladi, masalan, dependency injection uchun.

#### `ElementType.LOCAL_VARIABLE`
Mahalliy o'zgaruvchilarga qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.LOCAL_VARIABLE)
public @interface LocalVariableAnnotation {
    String value();
}

class MyClass {
    public void doSomething() {
        @LocalVariableAnnotation(value = "Mahalliy o'zgaruvchi")
        int count = 10;
        System.out.println("Count: " + count);
    }
}
```
**Tushuntirish**: `@LocalVariableAnnotation` mahalliy o'zgaruvchilarni belgilash uchun ishlatiladi, lekin bu kamdan-kam qo'llaniladi.

#### `ElementType.ANNOTATION_TYPE`
Boshqa annotatsiyalarga qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.ANNOTATION_TYPE)
public @interface MetaAnnotation {
    String value();
}

@MetaAnnotation("Bu meta-annotatsiya")
public @interface MyAnnotation {
    String description();
}
```
**Tushuntirish**: `@MetaAnnotation` boshqa annotatsiyalarni belgilash uchun ishlatiladi, masalan, annotatsiyalarni guruhlash yoki maxsus xatti-harakat qo'shish uchun.

#### `ElementType.PACKAGE`
Paketlarga qo'llaniladi.
```java
// package-info.java faylida
@PackageAnnotation("Bu paket annotatsiyasi")
package com.example;

import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.PACKAGE)
public @interface PackageAnnotation {
    String value();
}
```
**Tushuntirish**: `@PackageAnnotation` paketlarni hujjatlash yoki modullar bilan ishlash uchun ishlatiladi.

#### `ElementType.TYPE_PARAMETER`
Generik tur parametrlarga qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.TYPE_PARAMETER)
public @interface TypeParameterAnnotation {
    String value();
}

class MyClass<@TypeParameterAnnotation("Generik tur") T> {
    public void doSomething(T param) {
        System.out.println("Parametr: " + param);
    }
}
```
**Tushuntirish**: `@TypeParameterAnnotation` generik turlarni belgilash uchun ishlatiladi, masalan, tur xavfsizligini ta'minlash uchun.

#### `ElementType.TYPE_USE`
Tur ishlatiladigan joylarda qo'llaniladi.
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.TYPE_USE)
public @interface TypeUseAnnotation {
    String value();
}

class MyClass {
    public void doSomething(@TypeUseAnnotation("Tur ishlatilishi") String param) {
        System.out.println("Parametr: " + param);
    }
}
```
**Tushuntirish**: `@TypeUseAnnotation` tur ishlatiladigan har qanday joyda (masalan, o'zgaruvchilar, qaytarish turlari) qo'llaniladi.

#### `ElementType.MODULE`
Modullarga qo'llaniladi (Java 9+).
```java
// module-info.java faylida
@ModuleAnnotation("Bu modul annotatsiyasi")
module my.module {
}

import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.MODULE)
public @interface ModuleAnnotation {
    String value();
}
```
**Tushuntirish**: `@ModuleAnnotation` modul tizimida modullarni belgilash uchun ishlatiladi.

#### `ElementType.RECORD_COMPONENT`
Record komponentlariga qo'llaniladi (Java 14+).
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.RECORD_COMPONENT)
public @interface RecordComponentAnnotation {
    String value();
}

record Person(@RecordComponentAnnotation("Ism komponenti") String name, int age) {}
```
**Tushuntirish**: `@RecordComponentAnnotation` record classlarining komponentlarini belgilash uchun ishlatiladi.

## 3. `@Documented`
`@Documented` annotatsiyasi maxsus annotatsiyaning Javadoc hujjatlarida ko'rinishini ta'minlaydi. Agar qo'llanilmasa, annotatsiya Javadoc'da ko'rinmaydi.

### Misol
#### `@Documented` bilan
```java
import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Documented
@Retention(RetentionPolicy.RUNTIME)
public @interface DocumentedAnnotation {
    String value();
}

@DocumentedAnnotation("Bu hujjatlashtirilgan")
class MyClass {
    public void doSomething() {
        System.out.println("Ish bajarildi!");
    }
}
```
**Tushuntirish**: Javadoc generatsiya qilinganda `@DocumentedAnnotation` hujjatda ko'rinadi.

#### `@Documented` siz
```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
public @interface NonDocumentedAnnotation {
    String value();
}

@NonDocumentedAnnotation("Bu hujjatlashtirilmagan")
class MyClass {
    public void doSomething() {
        System.out.println("Ish bajarildi!");
    }
}
```
**Tushuntirish**: Javadoc generatsiya qilinganda `@NonDocumentedAnnotation` ko'rinmaydi.

## 4. `@Inherited`
`@Inherited` annotatsiyasi maxsus annotatsiyaning voris classlarga avtomatik ravishda o'tishini ta'minlaydi. Faqat `ElementType.TYPE` (classlar) uchun ishlaydi.

### Misol
#### `@Inherited` bilan
```java
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Inherited
@Retention(RetentionPolicy.RUNTIME)
public @interface InheritedAnnotation {
    String value();
}

@InheritedAnnotation("Bu class annotatsiyasi")
class ParentClass {
}

class ChildClass extends ParentClass {
}

class AnnotationReader {
    public static void main(String[] args) {
        InheritedAnnotation annotation = ChildClass.class.getAnnotation(InheritedAnnotation.class);
        System.out.println("Value: " + annotation.value());
    }
}
```
**Natija**:
```
Value: Bu class annotatsiyasi
```
**Tushuntirish**: `@Inherited` tufayli `@InheritedAnnotation` `ChildClass` ga avtomatik ravishda o'tadi.

#### `@Inherited` siz
```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
public @interface NonInheritedAnnotation {
    String value();
}

@NonInheritedAnnotation("Bu class annotatsiyasi")
class ParentClass {
}

class ChildClass extends ParentClass {
}

class AnnotationReader {
    public static void main(String[] args) {
        NonInheritedAnnotation annotation = ChildClass.class.getAnnotation(NonInheritedAnnotation.class);
        System.out.println("Annotation: " + annotation); // null
    }
}
```
**Natija**:
```
Annotation: null
```
**Tushuntirish**: `@NonInheritedAnnotation` voris classga o'tmaydi.

## 5. `@Repeatable`
`@Repeatable` annotatsiyasi maxsus annotatsiyani bir elementga bir necha marta qo'llash imkonini beradi. Buning uchun alohida "container" annotatsiyasi talab qilinadi.

### Misol
#### `@Repeatable` bilan
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Repeatable;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Tasks {
    Task[] value();
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Repeatable(Tasks.class)
public @interface Task {
    String description();
    int priority();
}

class MyClass {
    @Task(description = "Database update", priority = 1)
    @Task(description = "Send email", priority = 2)
    public void processTasks() {
        System.out.println("Vazifalar bajarilmoqda...");
    }
}

class AnnotationReader {
    public static void main(String[] args) throws Exception {
        Tasks tasks = MyClass.class.getMethod("processTasks").getAnnotation(Tasks.class);
        for (Task task : tasks.value()) {
            System.out.println("Description: " + task.description() + ", Priority: " + task.priority());
        }
    }
}
```
**Natija**:
```
Description: Database update, Priority: 1
Description: Send email, Priority: 2
```
**Tushuntirish**: `@Repeatable` tufayli `@Task` annotatsiyasi bir metodga bir nechta marta qo'llaniladi va `Tasks` container orqali o'qiladi.

#### `@Repeatable` siz
Agar `@Repeatable` ishlatilmasa, bir elementga bir annotatsiyani faqat bir marta qo'llash mumkin. Ikkinchi marta qo'llash kompilyatsiya xatosiga olib keladi.

---

## Qo'shimcha Eslatmalar
- **To'g'ri meta-annotatsiyani tanlash**: Annotatsiyaning maqsadiga qarab to'g'ri `@Retention` va `@Target` qiymatlarini tanlang. Masalan, reflection uchun `RUNTIME`, kod generatsiyasi uchun `SOURCE`.
- **Hujjatlash**: `@Documented` annotatsiyalarni API hujjatlarida ko'rsatish uchun muhim.
- **Vorislavlik**: `@Inherited` faqat class annotatsiyalari uchun ishlaydi, metod yoki o'zgaruvchilar uchun emas.
- **Takrorlash**: `@Repeatable` ishlatishda container annotatsiyasini to'g'ri aniqlash muhim.
- **Java versiyalari**: Ba'zi `@Target` qiymatlari (masalan, `MODULE`, `RECORD_COMPONENT`) faqat yangi Java versiyalarida mavjud.
