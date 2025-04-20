Рефлексия в Java — это механизм, который позволяет программе анализировать или модифицировать свое поведение во время выполнения. С помощью рефлексии можно получать информацию о классах, методах, полях и конструкторах во время выполнения программы, а также вызывать методы и изменять значения полей.

Пример использования рефлексии для вызова метода:

```java
import java.lang.reflect.Method;

public class ReflectionExample {
    public static void sayHello(String name) {
        System.out.println("Привет, " + name + "!");
    }

    public static void main(String[] args) {
        try {
            // Получаем класс по имени
            Class<?> cls = Class.forName("ReflectionExample");
            
            // Получаем метод sayHello с параметром String
            Method method = cls.getMethod("sayHello", String.class);
            
            // Вызываем метод с аргументом "Мир"
            method.invoke(null, "Мир");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Класс `Class<T>` параметризован

```java
Документация: _"The actual result type of `getClass()` is `Class<? extends |X|>` where `|X|` is the erasure of the static type of the expression on which `getClass` is called."

Employee e = ...;

//No cast needed!
Class<? extends Employee> c = e.getClass();

//No cast needed!
Employee newEmployee =
  ce.getDeclaredConstructor().newInstance();

//Compile-time error!
Class<? extends Number> cn = e.getClass();
```