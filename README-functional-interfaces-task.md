### Loyiha sharti: Kutubxona boshqaruv tizimi (Library Management System)

#### Umumiy maqsad
Maven loyihasi sifatida oddiy kutubxona boshqaruv tizimini yaratishi kerak. Tizimda 10 ta entity (class) bo‘lishi kerak, va loyihada Behavior Parameterization, funktsional interfeyslar (`Predicate`, `Function`, `Consumer`, `Supplier`, `UnaryOperator`, `BinaryOperator`), lambda ifodalari, method reference va Stream API’dan foydalanish talab qilinadi. Tizim kitoblar, a’zolar, ijaralar va boshqa kutubxona elementlarini boshqarish uchun funksiyalarni o‘z ichiga olishi kerak.

#### Talablar
1. **Entity classlari (10 ta)**:
   - Har bir entity real kutubxona tizimidagi ob’ektlarni ifodalashi kerak (masalan, kitob, muallif, a’zo, ijara va hokazo).
   - Har bir class kamida konstruktor, getterlar va `toString` metodiga ega bo‘lishi kerak.
   - classlar bir-biri bilan bog‘langan bo‘lishi kerak (masalan, `Book`da `Author` va `Publisher` bo‘lishi mumkin).

2. **Funktsional interfeyslar va Behavior Parameterization**:
   - `LibraryService` classida quyidagi funktsional interfeyslar ishlatilishi kerak:
     - `Predicate`: Ma’lumotlarni filtrlashtirish uchun (masalan, muayyan kategoriyadagi kitoblar).
     - `Function`: Ma’lumotlarni transformatsiya qilish uchun (masalan, kitob nomlarini olish).
     - `Consumer`: Ma’lumotlarni qayta ishlash yoki chop etish uchun.
     - `Supplier`: Yangi ob’ektlar yaratish uchun (masalan, yangi kitob qo‘shish).
     - `UnaryOperator`: Biror atributni o‘zgartirish uchun (masalan, kitob nomini yangilash).
     - `BinaryOperator`: Ikkita elementni solishtirish yoki birlashtirish uchun (masalan, eng qadimgi kitobni topish).
   - Behavior Parameterization yordamida metodlar moslashuvchan bo‘lishi kerak (masalan, filtr shartini tashqaridan uzatish).

3. **Lambda ifodalari va Method Reference**:
   - Har bir funktsional interfeysda lambda ifodalari va ularga alternativa sifatida method reference ishlatilishi kerak.
   - Masalan, `System.out::println` yoki `Book::getTitle` kabi.

4. **Stream API**:
   - Kitoblarni filtrlashtirish, saralash, transformatsiya qilish va reduksiya qilish uchun Stream API ishlatilishi kerak.

5. **Maven loyihasi**:
   - Loyiha Maven tuzilmasiga ega bo‘lishi kerak (`pom.xml` bilan).
   - Paket tuzilmasi mantiqiy bo‘lishi kerak (masalan, `entity` va `service` paketlari).

---

### Talabalardan kutilayotgan ishlar
Talabalar yuqoridagi shartlarga asoslanib quyidagi kodlarni yozishi kerak:

#### 1. `pom.xml`
- Java 17 uchun moslashtirilgan Maven konfiguratsiyasini yozish.
- `apache lombok` dan foydalanilsin

#### 2. Entity classlari (10 ta)
Talabalar quyidagi classlarni yozishi kerak (masalan):
- `Author`: Muallif (ism, tug‘ilgan yil).
- `Book`: Kitob (nomi, muallif, nashriyot, kategoriya).
- `Category`: Kategoriya (nomi).
- `Library`: Kutubxona (nomi, kitoblar ro‘yxati).
- `Loan`: Ijara (kitob, a’zo, ijara sanasi).
- `Member`: A’zo (ism, ID).
- `Publisher`: Nashriyot (nomi).
- `Reservation`: Band qilish (kitob, a’zo, sana).
- `Review`: Sharh (kitob, a’zo, izoh, baho).
- `Staff`: Xodim (ism, roli).

Har bir classda:
- Konstruktor.
- Getter metodlari.
- `toString` metodi.

#### 3. `LibraryService` classi
Bu class kutubxona funksiyalarini boshqaradi va funktsional interfeyslar bilan ishlaydi:
- `filterBooks(Predicate<Book>)`: Kitoblarni shart asosida filtrlaydi.
- `getBookTitles(Function<Book, String>)`: Kitob nomlarini olish.
- `printBooks(Consumer<Book>)`: Kitoblarni chop etish.
- `addBookFromSupplier(Supplier<Book>)`: Yangi kitob qo‘shish.
- `updateBookTitle(Book, UnaryOperator<String>)`: Kitob nomini o‘zgartirish.
- `findOldestBook(BinaryOperator<Book>)`: Eng qadimgi kitobni topish.
- `getSortedBooks(Comparator<Book>)`: Kitoblarni saralash.

#### 4. `Main` classi
- Yuqoridagi entitylardan ob’ektlar yaratish.
- `LibraryService` metodlarini sinab ko‘rish:
  - Predicate bilan filtr (masalan, fiction kitoblar).
  - Function bilan nomlarni olish.
  - Consumer bilan chop etish.
  - Supplier bilan kitob qo‘shish.
  - UnaryOperator bilan nomni yangilash.
  - BinaryOperator bilan eng qadimgi kitobni topish.
  - Method Reference bilan saralash.

#### 5. Funktsional xususiyatlar
- Har bir metodda lambda ifodalari va method reference’dan foydalanish.
- Stream API bilan filtr, map, reduce va sorted operatsiyalari.

---

### Namuna natija (Konsol chiqishi)
Talabalar loyihani ishga tushirganda quyidagi kabi natija olishi mumkin:
```
Fiction kitoblar:
Book One by John Doe (1970)
Book Two by Jane Smith (1985)

Kitob nomlari:
Book One
Book Two

Barcha kitoblar:
Book One by John Doe (1970) - ABC Publishing
Book Two by Jane Smith (1985) - ABC Publishing

Yangi kitob qo‘shildi:
Book One by John Doe (1970)
Book Two by Jane Smith (1985)
Book Three by John Doe (1970)

Title o‘zgartirildi:
Book One (Updated) by John Doe (1970)
Book Two by Jane Smith (1985)
Book Three by John Doe (1970)

Eng qadimgi kitob:
Book One (Updated) by John Doe (1970)

Saralangan kitoblar:
Book One (Updated) by John Doe (1970)
Book Three by John Doe (1970)
Book Two by Jane Smith (1985)
```

---

### Talabalarga maslahatlar
1. **Qadam-baqadam yozing**:
   - Avval entity classlarni yarating.
   - Keyin `LibraryService` metodlarini qo‘shing.
   - Oxirida `Main` classida sinab ko‘ring.

2. **Funktsional interfeyslarni tushuning**:
   - Har bir interfeysning maqsadini o‘rganing (`Predicate` shart uchun, `Function` transformatsiya uchun va hokazo).

3. **Lambda va Method Reference’ni solishtiring**:
   - Har bir metodda avval lambda yozib, keyin uni method reference bilan almashtirib ko‘ring.

4. **Stream API’dan qo‘rqmang**:
   - Oddiy operatsiyalardan boshlang (filter, map), keyin reduce va sorted’ga o‘ting.

5. **Test qiling**:
   - Har bir metodni alohida sinab, to‘g‘ri ishlashini tekshiring.

---
[x] Hammaga omad G52!
