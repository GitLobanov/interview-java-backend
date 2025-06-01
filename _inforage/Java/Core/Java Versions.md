## JDK 8

- MetaSpace заменила PermGen

## JDK 9

- Фабричные методы для коллекций (JDK 9): `List.of()`, `Set.of()`, `Map.of()`.
- Compact Strings (JDK 9): Более эффективное хранение строк в памяти.
- Модульная система (Project Jigsaw) (JDK 9): Введение модулей для управления зависимостями.
- G1 стал GC по умолчанию.
- Garbage Collector Interface
## JDK 10

- **`var` для локальных переменных** (JDK 10): Автоматическое определение типа переменной.
## JDK 11

- Epsilon GC (JDK 11)  Epsilon GC — "no-op" GC, не освобождает память  Полезен для тестирования производительности или short-lived приложений  Добавлен в JDK 11- ZGC (JDK 11) 
- Z Garbage Collector — появился в JDK 11  Цель: масштабируемость под гигантские кучи (до нескольких терабайт)  Подходит для низких задержек (<10 мс)
## JDK 12

- Shenandoah GC
## JDK 14

- Switch Expressions (JDK 14): Улучшенный синтаксис для `switch`, поддерживающий выражения и стрелочную нотацию.
## JDK 15

- **Text Blocks** (JDK 15): Многострочные строки без необходимости экранирования.

## JDK 16

- `.toList()` (JDK 16): Удобный метод для преобразования потока в список.
- `.mapMulti()` (JDK 16): Альтернатива `flatMap`.
- Records (JDK 16): Сокращенный синтаксис для создания неизменяемых классов данных.
- Pattern Matching для `instanceof` (JDK 16): Упрощение проверки типов и приведения.

## JDK 17

- Sealed Classes (JDK 17): Ограничение наследования, позволяющее явно указывать разрешенные подклассы.

## JDK 18

- **Deprecation Finalization** (JDK 18): Устаревание метода `finalize` для освобождения ресурсов.
## JDK 19

- **Foreign Function & Memory API (Preview)**
- **Record Patterns (Preview)**: Первая предварительная версия, которая затем дорабатывалась и стала финальной позже.
- **Virtual Threads (Preview)**: Тоже очень ожидаемая фича, которая вышла в Preview в JDK 19.
- **Pattern Matching for switch (Second Preview)**: Продолжение работы над этой фичей.
- **Structured Concurrency (Incubator)**: Еще одна интересная фича в области параллелизма.
## JDK 21

-  [Pattern Matching for switch](https://openjdk.org/jeps/441)

```java
static String formatterPatternSwitch(Object obj) {
    return switch (obj) {
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> obj.toString();
        var users = repo.getUsers();
    };
}
```

- [Virtual Threads](https://openjdk.org/jeps/444)

The introduction to the JVM of the concept of threads managed managed not by the operating system, but by the virtual machine itself.

```java
Thread.builder().virtual().factory();
```

- String Templates (JDK 21, Preview): Безопасная интерполяция строк.

- Record Patterns для `switch` и `instanceof`: Разбор сложных вложенных структур

Она позволяет использовать паттерны для деконструкции сложных вложенных объектов, таких как записи (`records`) или другие структуры данных, в сочетании с `switch` и `instanceof`

Пример.

record Point2D(int x, int y) {}
record ColoredPoint(Point2D point, Color color) {}

Теперь мы можем использовать **Record Patterns** для деконструкции объекта `ColoredPoint` непосредственно в выражении `instanceof`:

```java
if (r instanceof ColoredPoint(Point2D(int x, int y), Color c)) {
    System.out.println("Point coordinates: (" + x + ", " + y + ")");
    System.out.println("Color: " + c);
}

```

```java
if (r instanceof ColoredPoint) {
	ColoredPoint c = (ColoredPoint) r;
	Point2D p2d = (Point2D) c.getPoint();
    System.out.println("Point coordinates: (" + x + ", " + y + ")");
    System.out.println("Color: " + c);
}

```
1. Компилятор проверяет, является ли объект `r` экземпляром `ColoredPoint`.
2. Если да, то он автоматически разбирает (деконструирует) объект на его компоненты:
    - `Point2D` внутри `ColoredPoint`.
    - Далее разбирает `Point2D` на координаты `x` и `y`.
    - Также извлекает цвет `c`.

Это позволяет сразу работать с переменными `x`, `y` и `c` внутри блока `if`.
## JDK 24

- JEP 485: Stream Gatherers