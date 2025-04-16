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
---
