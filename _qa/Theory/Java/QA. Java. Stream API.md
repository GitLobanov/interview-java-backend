## QA

###### Что такое Stream Api и для чего нужны Stream?

Представляет собой последовательность элементов, над которой можно производить различные операции. Его задача - упростить работу с наборами данных , в частности, упростить операции фильтрации, сортировки и другие манипуляции данными

_**С какими типами данных может работать?**_

- **Объектами** — `Stream<T>`, где `T` — любой ссылочный тип (например, `String`, `Integer`, пользовательские классы).
    
- **Примитивами** — существуют специализированные стримы:
    
    - **IntStream** для работы с `int`.
    - **LongStream** для работы с `long`.
    - **DoubleStream** для работы с `double`.
    
    Для других примитивов стримы не предусмотрены, их нужно упаковывать в объекты-обёртки (например, `Byte` для `byte`).
    
_**Проблема с примитивами в стримах?**_

Работа с примитивами через `Stream<T>` приводит к **автоупаковке и распаковке**. Это накладывает следующие проблемы:

1. **Снижение производительности** — упаковка/распаковка добавляют накладные расходы.
2. **Увеличение памяти** — вместо примитивов создаются объекты (например, `Integer`, `Double`).

Специализированные стримы (**IntStream, LongStream, DoubleStream**) решают эти проблемы, обеспечивая работу непосредственно с примитивами без автоупаковки.
######  Почему Stream называют ленивым?

**Ленивость Stream** = вычисления только по требованию (при вызове терминальной операции).

```java
Stream.iterate(0, n -> n + 1)  // 0, 1, 2, 3, 4, 5, ...
      .filter(n -> n % 2 == 0) // 0, 2, 4, 6, ...
      .map(n -> n * 10)        // 0, 20, 40, 60, ...
      .limit(3)                // Берем только 3 элемента
      .forEach(System.out::println); // Терминальная операция

// если не не вызовим терминальную операцию - исполнение стрима не произойдет
```
###### Какие существуют способы создания Stream?

- Пустой стрим. `Stream.empty()`
- Стрим из List. `list.stream()`
- Стрим из Map. `map.entrySet().stream()`
- Стрим из массива. `Arrays.stream(array)`
- Стрим из указанных элементов. `Stream.of(”1”, “2”, “3”)`
- Конкатенацией двух стримов

###### Типы методов в Stream API

Промежуточные и терминальные.

Промежуточные делятся на statefull и state
###### Что такое промежуточные методы? Какие промежуточные методы в стримах вы знаете?

_Промежуточный метод_ - метод, который возвращает Stream. Существуют:

- `filter(Predicate<T> predicate)` Используется для фильтрации элементов потока по условию

```java
List<String> names = List.of("hey", "goodbye", "ok");

List<String> newNames = names.stream()
                .filter(s -> s.length() > 2)
                .toList();
```

- `map(Function<T, R> mapper)` Преобразует каждый элемент потока в другой элемент

```java
List<Integer> numbers = List.of(1,2,3,4);
        numbers.stream()
                .map(i -> i * i)
                .forEach(System.out::println);
```

- `flatMap(Function<T, Stream<R>> mapper)` Разворачивает вложенные структуры, объединяя несколько потоков в один

```java
List<Human> humans = asList(
                new Human("Sam", asList("Buddy", "Lucy")),
                new Human("Bob", asList("Frankie", "Rosie")),
                new Human("Marta", asList("Simba", "Tilly")));

        List<String> petNames = humans.stream()
                .flatMap(human -> human.getPetNames().stream())
                .toList();
                // Вернется список имен домашних животных (Buddy, Lucy и т.д)
```

- `limit(long maxSize)` Ограничивает количество элементов в потоке

```java
List<String> names = List.of("hey", "goodbye", "ok");

        List<String> newNames = names.stream()
                .limit(2)
                .toList();
                // Будет выведено два первых слова
```

- `skip(long n)` Пропускает первые n элементов в потоке

```java
List<String> names = List.of("hey", "goodbye", "ok");

        List<String> newNames = names.stream()
                .skip(2)
                .toList();
                // Будет выведена последняя строка 'ok'
```

- `concat(Stream<T> a, Stream<T> b)` Объединяет два потока в один

```java
List<String> listOne = List.of("Apple", "Banana", "Cherry");
        List<String> listTwo = List.of("Orange", "Peach", "Plum");

        Stream<String> combinedStream = Stream.concat(listOne.stream(), listTwo.stream());
        combinedStream.forEach(System.out::println);
```

- `peek(Consumer<T> action)` Выполняет действие над каждым элементом потока (например, для отладки)

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7);

        List<Integer> result = numbers.stream()
                .peek(num -> System.out.println("Исходное число:" + num))
                .map(num -> num * 2)
                .peek(num -> System.out.println("Число после удвоения" + num))
                .toList();
                // Вывод:
                // Исходное число: 1
                // Число после удвоения: 2 и т.д
```

- `distinct()` Убирает дубликаты, оставляя только уникальные элементы

```java
List<Integer> numbers = asList(1,2,3,4,5,5);

        List<Integer> distinctNumbers = numbers.stream()
                .distinct()
                .toList();
```

- `sorted() / sorted(Comparator<T> comparator)` Использует естественный порядок фильтрации, либо принимает Comparator для сортировки элементов

```java
List<String> words = List.of("hello", "bye", "ok", "goodbye", "andrew");

        List<String> sortedWords = words.stream()
                .sorted(Comparator.comparing(String::length))
                .toList();
```

###### Что такое терминальные методы? Какие терминальные методы в стримах вы знаете?

_Терминальные методы_ завершают работу с потоком, после их вызова дальнейшие операции невозможны. Основные терминальные методы:

- `forEach(Consumer<T> action)` Выполняет действие над каждым элементом потока

```java
List<String> words = List.of("hi", "hi", "goodbye", "ok", "dog", "cat", "dog");

        words.stream()
                .filter(w -> w.length() > 2)
                .forEach(System.out::println);
```

- `collect(Collector<T, A, R> collector)` Преобразует элементы потока в другую структуру данных (список, множество и т.д.)

```java
List<String> words = List.of("hi", "hi", "goodbye", "ok", "dog", "cat", "dog");

        Set<String> wordsSet = words.stream()
                .filter(w -> w.length() > 2)
                .collect(Collectors.toSet());
```

- `reduce(BinaryOperator<T> accumulator)` Сворачивает все элементы потока в одно значение, используя бинарную операцию

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

        int sum = numbers.stream()
                .reduce(0, Integer::sum);
```

- `count()` Возвращает количество элементов в потоке

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 10, 1298, 27);

        long counted = numbers.stream()
                .filter(n -> n % 2 == 0)
                .count(); // Посчитали четные числа
```

- `min(Comparator<T> comparator)` Возвращает минимальный элемент потока по заданному компаратору

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 10, 1298, 27);

        int max = numbers.stream()
                .min(Integer::compareTo).orElseThrow();
```

- `max(Comparator<T> comparator)` Возвращает максимальный элемент потока по заданному компаратору

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 10, 1298, 27);

        int max = numbers.stream()
                .max(Integer::compareTo).orElseThrow();
```

- `findFirst()` Возвращает первый элемент потока (опционально)

```java
List<Integer> numbers = List.of(1,2,3,4,5,6,10,120,130);

        numbers.stream()
                .filter(n -> n > 10)
                .findFirst()
                .orElseThrow();
```

- `findAny()` Возвращает любой элемент потока (опционально, полезно в параллельных потоках)

```java
List<String> names = List.of("hello java", "goodBye");

        Optional<String> name = names.stream()
                .filter(s -> s.contains("java"))
                .findAny();
```

- `anyMatch(Predicate<T> predicate)` Возвращает true, если хотя бы один элемент соответствует условию

```java
List<Integer> nums = List.of(1,2,3,4,5,6);

        boolean is = nums.stream()
                .anyMatch(n -> n % 2 == 0);
                //Вернет true
```

- `allMatch(Predicate<T> predicate)` Возвращает true, если все элементы соответствуют условию

```java
List<Integer> nums = List.of(-1,2,3,4,5,6);

        boolean is = nums.stream()
                .allMatch(n -> n > 0);
                //Вернет false
```

- `noneMatch(Predicate<T> predicate)` Возвращает true, если ни один элемент не соответствует условию

```java
List<Integer> nums = List.of(1,3,3,5,5,7);

        boolean is = nums.stream()
                .noneMatch(n -> n % 2 == 0);
                //Вернет true (т.к ни одно не удовлетворяет условию)
```

###### Можно ли переиспользовать Stream после терминального метода?

Нет, **стрим нельзя переиспользовать после вызова терминального метода**. После выполнения терминальной операции стрим закрывается, и любая последующая попытка вызова методов вызовет `IllegalStateException`.

Если нужно работать с данными снова, их придётся пересоздать (например, вызвать `.stream()` для исходной коллекции).

###### Использование Stream без терминального метода?

Если стрим не завершается терминальным методом, никакие операции с ним не будут выполнены. Причина в **ленивой оценке** (lazy evaluation) Stream API — промежуточные методы не выполняются до вызова терминального метода.

###### Метод peek()

Метод `peek()` используется для промежуточной обработки элементов стрима. Он позволяет выполнить действие над каждым элементом стрима без изменения самих элементов. Основное применение — это отладка или логирование данных перед выполнением конечной операции.

_Важно:_ метод `peek()` не изменяет поток и чаще всего используется для поиска багов (например, вывода информации о каждом элементе).

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> result = numbers.stream()
    .peek(n -> System.out.println("Processing: " + n))
    .map(n -> n * n)
    .collect(Collectors.toList());
System.out.println(result); // [1, 4, 9, 16, 25]
```

###### Метод map()

Метод `map()` применяется для преобразования каждого элемента стрима с помощью функции. Он берет входной элемент и возвращает новый элемент, который соответствует результату примененной функции.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> squares = numbers.stream()
    .map(n -> n * n)
    .collect(Collectors.toList());
System.out.println(squares); // [1, 4, 9, 16, 25]
```

###### Метод flatMap()

Метод `flatMap()` используется для преобразования каждого элемента стрима в другой стрим, а затем “выравнивания” полученных стримов в один плоский стрим. Это особенно полезно для работы с коллекциями внутри коллекций.

```java
List<List<Integer>> numbers = Arrays.asList(
    Arrays.asList(1, 2),
    Arrays.asList(3, 4),
    Arrays.asList(5)
);
List<Integer> flatNumbers = numbers.stream()
    .flatMap(List::stream)
    .collect(Collectors.toList());
System.out.println(flatNumbers); // [1, 2, 3, 4, 5]
```

###### Чем отличаются методы map() и flatMap()?

- `map()` преобразует каждый элемент в один элемент (или объект).
- `flatMap()` преобразует каждый элемент в стрим и объединяет все полученные стримы в один.

###### Метод filter()

Промежуточный. Stateless

Метод `filter()` используется для фильтрации элементов стрима на основе условия (предиката). Он пропускает элементы, которые не удовлетворяют условию.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
System.out.println(evenNumbers); // [2, 4]
```


###### Метод limit()

Промежуточный. Statefull

Метод `limit()` возвращает стрим, состоящий не более чем из указанного количества элементов. Это полезно для ограничения размера стрима.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> limited = numbers.stream()
    .limit(3)
    .collect(Collectors.toList());
System.out.println(limited); // [1, 2, 3]
```


###### Метод skip()

Промежуточный. Stateful

Метод `skip()` пропускает указанное количество элементов и возвращает стрим, начинающийся с оставшихся элементов.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> skipped = numbers.stream()
    .skip(2)
    .collect(Collectors.toList());
System.out.println(skipped); // [3, 4, 5]
```

###### Метод sorted()

Промежуточный. Stateful (ждет когда все элементы дойдут до него, только затем пропускает их дальше) 

Метод `sorted()` сортирует элементы стрима. По умолчанию элементы сортируются в естественном порядке, но можно задать компаратор для пользовательской сортировки.

```java
List<String> names = Arrays.asList("John", "Anna", "Tom");
List<String> sortedNames = names.stream()
    .sorted()
    .collect(Collectors.toList());
System.out.println(sortedNames); // [Anna, John, Tom]
```

###### Метод distinct(). Сложность по времени

Метод `distinct()` возвращает стрим, который содержит только уникальные элементы (без дубликатов).

```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 4, 4, 5);
List<Integer> uniqueNumbers = numbers.stream()
    .distinct()
    .collect(Collectors.toList());
System.out.println(uniqueNumbers); // [1, 2, 3, 4, 5]
```

**Сложность по времени**

- **Добавление в `LinkedHashSet`:**
    - Каждая операция добавления имеет **амортизированную сложность** **O(1)** при удачном хэшировании.
    - В худшем случае (много коллизий) — **O(n)** на элемент.
- **Общая сложность метода:**
    - При удачном хэшировании: **O(n)**, где **n** — количество элементов в исходном потоке.
    - При плохом хэшировании (много коллизий): до **O(n^2)**.
    
    В реальных сценариях при корректно реализованных `hashCode()` и `equals()` метод работает с линейной сложностью **O(n)**.
    
###### Метод collect()

Метод `collect()` используется для преобразования стрима в коллекцию или другой тип данных. Чаще всего используется для сбора элементов в список, множество или строку.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> collectedList = numbers.stream()
    .collect(Collectors.toList());
System.out.println(collectedList); // [1, 2, 3, 4, 5]
```

###### Метод groupingBy()

Разделяет элементы исходной коллекции или потока на группы (категории) на основе указанной функции.

Возвращает результат в виде Map, где:

- Ключ — значение, возвращённое функцией группировки.
- Значение — список элементов, соответствующих этому ключу.

```java
List<String> strings = Arrays.asList("cat", "dog", "fish", "ant", "elephant");

        Map<Integer, List<String>> groupedByLength = strings.stream()
                .collect(Collectors.groupingBy(String::length));

        System.out.println(groupedByLength);

  //3=[cat, dog, ant], 
  //4=[fish], 
  //8=[elephant]
```


###### Метод reduce()

Метод reduce() сводит (агрегирует) элементы стрима к одному значению с помощью функции аккумулятора. Например, для вычисления суммы или произведения всех элементов.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream()
    .reduce(0, Integer::sum); // 0 — начальное значение
System.out.println(sum); // 15
```

###### Метод parallelStream()

Метод **`parallelStream()`** создаёт стрим, выполняющий операции в несколько потоков (параллельно). Он используется для коллекций и предоставляет возможность автоматически разделить обработку данных на части.

```java
import java.util.Arrays;
import java.util.List;

public class ParallelStreamExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("A", "B", "C", "D");

        list.parallelStream()
            .forEach(item -> {
                System.out.println(Thread.currentThread().getName() + " processes " + item);
            });
    }
}
```

###### Расскажите про класс Collectors и его методы [#](https://zhukovsd.github.io/java-backend-interview-prep/%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D1%8B-java/stream-api/#6-%d1%80%d0%b0%d1%81%d1%81%d0%ba%d0%b0%d0%b6%d0%b8%d1%82%d0%b5-%d0%bf%d1%80%d0%be-%d0%ba%d0%bb%d0%b0%d1%81%d1%81-collectors-%d0%b8-%d0%b5%d0%b3%d0%be-%d0%bc%d0%b5%d1%82%d0%be%d0%b4%d1%8b)

_Класс Collectors_ предоставляет ряд статических методов для создания коллекций, таких как списки, множества, карты и другие, из элементов потока (Stream). Эти методы широко используются для агрегации данных в потоках. Основные методы:

- `toList()` Собирает элементы потока в список
- `toSet()` Собирает элементы потока в множество (Set)
- `toMap()` Собирает элементы потока в карту (Map), используя ключи и значения, которые задаются функциями
- `joining()` Объединяет элементы потока в одну строку, используя заданный разделитель и префикс/суффикс
- `counting()` Считает количество элементов в потоке
- `summarizingInt()` Собирает статистику по числовым значениям (например, сумма, среднее, максимальное и минимальное значение) Return IntSummaryStatistics
- `averagingInt()` Вычисляет среднее значение по числовым элементам
- `partitioningBy()` Разделяет элементы потока на две группы по заданному предикату
- `groupingBy()` Группирует элементы потока по заданному классификатору
- `reducing()` Выполняет редукцию элементов потока с использованием заданного бинарного оператора
- `toCollection(Supplier<C>)` Превращает поток в коллекцию

```java
List<String> names = Arrays.asList("Jaime", "", "Tyron");

        Queue<String> newNames = names.stream()
                .filter(s -> !s.isEmpty())
                .collect(Collectors.toCollection(LinkedList::new));
```

###### Для группировки элементов в Map какой Collector будешь использовать?

###### Что такое IntStream и DoubleStream?

IntStream и DoubleStream - это специальные стримы в Java для работы с примитивами int и double. Поддерживают дополнительные терминальные методы:

- `sum()`
- `average()`
- `mapToObj()`

###### Разница между parallel и parallerStream? 

|Метод|Описание|
|---|---|
|`stream().parallel()`|Преобразует **уже существующий** стрим в параллельный.|
|`parallelStream()`|Создаёт **сразу параллельный стрим** из коллекции.|

Оба метода дают один и тот же результат, но разница в способе вызова:

- `list.stream().parallel()` полезно, если сначала требуется настроить стрим (например, добавить фильтрацию), а затем переключить его в параллельный режим.
- `list.parallelStream()` используется для простоты, если сразу нужен параллельный стрим.

###### Что такое параллельные стримы? 

**В Java Stream API параллельное выполнение обеспечивается с помощью **`ForkJoinPool`**, который разбивает задачу на подзадачи (fork) и объединяет результаты (join). При вызове **`parallelStream()`**, элементы потока делятся на части, и эти части могут обрабатываться разными потоками внутри ForkJoinPool для повышения производительности.

**Пример 1: Сравнение `stream()` и `parallelStream()`**

```java
import java.util.Arrays;
import java.util.List;

public class ParallelStreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Dave");

        // Последовательный стрим
        System.out.println("Sequential Stream:");
        names.stream().forEach(name -> System.out.println(Thread.currentThread().getName() + ": " + name));

        // Параллельный стрим
        System.out.println("\nParallel Stream:");
        names.parallelStream().forEach(name -> System.out.println(Thread.currentThread().getName() + ": " + name));
    }
}
```

Вывод (примерный):

```
Sequential Stream:
main: Alice
main: Bob
main: Charlie
main: Dave

Parallel Stream:
ForkJoinPool.commonPool-worker-1: Alice
ForkJoinPool.commonPool-worker-3: Bob
ForkJoinPool.commonPool-worker-2: Charlie
main: Dave
```

**Когда использовать параллельные стримы?**

✅ **Когда данных очень много** (миллионы записей).  
✅ **Когда операции CPU-интенсивные** (например, сложные вычисления).  
✅ **Когда нет зависимостей между элементами** (например, суммирование чисел).

🚫 **Когда НЕ использовать?**  
❌ Если коллекция маленькая (параллельность создаст накладные расходы).  
❌ Если порядок важен (параллельные стримы могут изменить порядок).  
❌ Если есть побочные эффекты (`forEach()` в `parallelStream` может работать непредсказуемо).


###### Что такое ForkJoinPool

Рекурсивно делит задачу на подзадачи, после этого раскладывает их по очередям потоков и начинает выполнение.

Фреймворк Fork/Join, представленный в JDK 7, - это набор классов и интерфейсов позволяющих использовать преимущества многопроцессорной архитектуры современных компьютеров. Он разработан для выполнения задач, которые можно рекурсивно разбить на маленькие подзадачи, которые можно решать параллельно.

- Этап Fork: большая задача разделяется на несколько меньших подзадач, которые в свою очередь также разбиваются на меньшие. И так до тех пор, пока задача не становится тривиальной и решаемой последовательным способом.
- Этап Join: далее (опционально) идёт процесс «свёртки» - решения подзадач некоторым образом объединяются пока не получится решение всей задачи.

Решение всех подзадач (в т.ч. и само разбиение на подзадачи) происходит параллельно.

Для решения некоторых задач этап Join не требуется. Например, для параллельного QuickSort — массив рекурсивно делится на всё меньшие и меньшие диапазоны, пока не вырождается в тривиальный случай из 1  элемента.

Хотя в некотором смысле Join будет необходим и тут, т.к. всё равно остаётся необходимость дождаться пока не закончится выполнение всех подзадач.

Ещё одно замечательное преимущество этого фреймворка заключается в том, что он **использует work-stealing алгоритм: потоки, которые завершили выполнение собственных подзадач, могут «украсть» подзадачи у других потоков, которые всё ещё заняты**.

###### Какая разница между findAny() и findFirst()

| Критерий               | `findAny()`                                             | `findFirst()`                                                  |
| ---------------------- | ------------------------------------------------------- | -------------------------------------------------------------- |
| **Что возвращает**     | **Любой** элемент потока (если есть)                    | **Первый** элемент потока (если есть)                          |
| **Порядок**            | Не гарантирует порядок                                  | Гарантирует, что вернёт именно первый элемент в порядке стрима |
| **Параллельный стрим** | Может работать **быстрее** (не нужно соблюдать порядок) | Может работать **медленнее** (должен сохранять порядок)        |
| **Когда использовать** | Если **неважно**, какой элемент получить                | Если нужно получить **именно первый** элемент в порядке        |
| **Пример**             | `stream.parallel().findAny()`                           | `stream.parallel().findFirst()`                                |

**Итог**:

- Если важен **порядок** — используй `findFirst()`.
- Если порядок не важен, и нужна **производительность** — `findAny()`.
## Resources
