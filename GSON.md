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
