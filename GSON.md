# JsonSerializer bilan maxsus serializatsiya
```java
import com.google.gson.*;
import java.lang.reflect.Type;

public class Main {
    public static void main(String[] args) {
        Gson gson = new GsonBuilder()
                .setPrettyPrinting()
                .registerTypeAdapter(Person.class, new PersonSerializer())
                .create();

        Person person = new Person("Ali", 25);
        String json = gson.toJson(person);
        System.out.println("JSON:\n" + json);
    }
}

class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class PersonSerializer implements JsonSerializer<Person> {
    @Override
    public JsonElement serialize(Person person, Type type, JsonSerializationContext context) {
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("full_name", person.name.toUpperCase()); // Katta harflarga aylantirish
        jsonObject.addProperty("years", person.age); // Maydon nomini o‘zgartirish
        return jsonObject;
    }
}
```

#### Natija:

`JSON:`
```json
{
  "full_name": "ALI",
  "years": 25
}
```
Tushuntirish:
- `PersonSerializer` classi `JsonSerializer<Person>` interfeysini amalga oshiradi.
- `serialize()` metodida yangi `JsonObject` yaratilib, `name` maydoni katta harflarga aylantiriladi va `“full_name”` sifatida qo‘shiladi.
- `age` maydoni `“years”` sifatida qo‘shiladi.
- `registerTypeAdapter()` orqali Person sinfi uchun maxsus serializator ro‘yxatdan o‘tkaziladi.
---

# JsonDeserializer bilan maxsus deserializatsiya
```java
import com.google.gson.*;
import java.lang.reflect.Type;

public class Main {
    public static void main(String[] args) {
        Gson gson = new GsonBuilder()
                .setPrettyPrinting()
                .registerTypeAdapter(Person.class, new PersonDeserializer())
                .create();

        String json = "{\"full_name\":\"ALI\",\"years\":25}";
        Person person = gson.fromJson(json, Person.class);
        System.out.println("Person: name=" + person.name + ", age=" + person.age);
    }
}

class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class PersonDeserializer implements JsonDeserializer<Person> {
    @Override
    public Person deserialize(JsonElement json, Type type, JsonDeserializationContext context) throws JsonParseException {
        JsonObject jsonObject = json.getAsJsonObject();
        String name = jsonObject.get("full_name").getAsString().toLowerCase(); // Kichik harflarga aylantirish
        int age = jsonObject.get("years").getAsInt();
        return new Person(name, age);
    }
}
```
#### Natija:
```text
Person: name=ali, age=25
```
Tushuntirish:
- `PersonDeserializer` classi `JsonDeserializer<Person>` interfeysini amalga oshiradi.
- `deserialize()` metodida JSON’dan `full_name` va `years` kalitlari olinadi.
- `full_name` kichik harflarga aylantiriladi va `Person` obyektining `name` maydoniga o‘rnatiladi.
- `years` `age` maydoniga o‘rnatiladi.
- `registerTypeAdapter()` orqali `Person` classi uchun maxsus deserializator ro‘yxatdan o‘tkaziladi.
---  
# JsonSerializer va JsonDeserializer’ni birgalikda ishlatish
```java
import com.google.gson.*;
import java.lang.reflect.Type;

public class Main {
    public static void main(String[] args) {
        Gson gson = new GsonBuilder()
                .setPrettyPrinting()
                .registerTypeAdapter(Person.class, new PersonSerializer())
                .registerTypeAdapter(Person.class, new PersonDeserializer())
                .create();

        // Serializatsiya
        Person person = new Person("Ali", 25);
        String json = gson.toJson(person);
        System.out.println("JSON:\n" + json);

        // Deserializatsiya
        Person deserializedPerson = gson.fromJson(json, Person.class);
        System.out.println("Deserialized: name=" + deserializedPerson.name + ", age=" + deserializedPerson.age);
    }
}

class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class PersonSerializer implements JsonSerializer<Person> {
    @Override
    public JsonElement serialize(Person person, Type type, JsonSerializationContext context) {
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("full_name", person.name.toUpperCase());
        jsonObject.addProperty("years", person.age);
        return jsonObject;
    }
}

class PersonDeserializer implements JsonDeserializer<Person> {
    @Override
    public Person deserialize(JsonElement json, Type type, JsonDeserializationContext context) throws JsonParseException {
        JsonObject jsonObject = json.getAsJsonObject();
        String name = jsonObject.get("full_name").getAsString().toLowerCase();
        int age = jsonObject.get("years").getAsInt();
        return new Person(name, age);
    }
}
```
---
# TypeAdapter bilan maxsus serializatsiya/deserializatsiya
`TypeAdapter<T>` class' `JsonSerializer` va `JsonDeserializer`’ga muqobil sifatida ishlatiladi. U `serializatsiya` va `deserializatsiya`ni bir classda birlashtiradi va ko‘proq moslashuvchanlik ta'minlaydi, lekin kod murakkabroq bo‘lishi mumkin.
#### Misol: `TypeAdapter`
```java
import com.google.gson.*;
import com.google.gson.stream.JsonReader;
import com.google.gson.stream.JsonWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        Gson gson = new GsonBuilder()
                .setPrettyPrinting()
                .registerTypeAdapter(Person.class, new PersonTypeAdapter())
                .create();

        // Serializatsiya
        Person person = new Person("Ali", 25);
        String json = gson.toJson(person);
        System.out.println("JSON:\n" + json);

        // Deserializatsiya
        Person deserializedPerson = gson.fromJson(json, Person.class);
        System.out.println("Deserialized: name=" + deserializedPerson.name + ", age=" + deserializedPerson.age);
    }
}

class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class PersonTypeAdapter extends TypeAdapter<Person> {
    @Override
    public void write(JsonWriter out, Person person) throws IOException {
        out.beginObject();
        out.name("full_name").value(person.name.toUpperCase());
        out.name("years").value(person.age);
        out.endObject();
    }

    @Override
    public Person read(JsonReader in) throws IOException {
        in.beginObject();
        String name = null;
        int age = 0;
        while (in.hasNext()) {
            String key = in.nextName();
            if (key.equals("full_name")) {
                name = in.nextString().toLowerCase();
            } else if (key.equals("years")) {
                age = in.nextInt();
            } else {
                in.skipValue();
            }
        }
        in.endObject();
        return new Person(name, age);
    }
}
```
#### Natija:
```json
JSON:
{
  "full_name": "ALI",
  "years": 25
}
Deserialized: name=ali, age=25
```
Tushuntirish:
- `TypeAdapter` classi `write()` (serializatsiya) va `read()` (deserializatsiya) metodlarini amalga oshiradi.
- `write()` metodida JSON yoziladi, `read()` metodida JSON o‘qiladi.
- `TypeAdapter` ko‘proq past darajali (low-level) boshqaruvni ta'minlaydi, lekin ko‘proq kod yozishni talab qiladi.
---
# Date uchun maxsus serializatsiya/deserializatsiya
```java
import com.google.gson.*;
import java.lang.reflect.Type;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Main {
    public static void main(String[] args) {
        Gson gson = new GsonBuilder()
                .setPrettyPrinting()
                .registerTypeAdapter(Date.class, new DateSerializer())
                .registerTypeAdapter(Date.class, new DateDeserializer())
                .create();

        // Serializatsiya
        Person person = new Person("Ali", new Date());
        String json = gson.toJson(person);
        System.out.println("JSON:\n" + json);

        // Deserializatsiya
        Person deserializedPerson = gson.fromJson(json, Person.class);
        System.out.println("Deserialized: name=" + deserializedPerson.name + ", date=" + deserializedPerson.date);
    }
}

class Person {
    String name;
    Date date;

    public Person(String name, Date date) {
        this.name = name;
        this.date = date;
    }
}

class DateSerializer implements JsonSerializer<Date> {
    private static final SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd");

    @Override
    public JsonElement serialize(Date date, Type type, JsonSerializationContext context) {
        return new JsonPrimitive(formatter.format(date));
    }
}

class DateDeserializer implements JsonDeserializer<Date> {
    private static final SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd");

    @Override
    public Date deserialize(JsonElement json, Type type, JsonDeserializationContext context) throws JsonParseException {
        try {
            return formatter.parse(json.getAsString());
        } catch (ParseException e) {
            throw new JsonParseException("Invalid date format", e);
        }
    }
}
```
