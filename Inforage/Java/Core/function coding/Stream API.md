# Компоненты

![[Pasted image 20240829194911.png]]

![[Pasted image 20241013110918.png]]

1.Источником данных могут выступать разные сущности - строка, коллекция, массив и тд.  

2.Промежуточных операций может быть сколько угодно. Каждый промежуточный оператор возвращает новый объект стрима.  

3.Терминальная операция одна

# Операции

## Получение источника


## Промежуточные
### `map(Function)`

> Метод `map()` преобразует каждый элемент потока с помощью функции, переданной в виде лямбда-выражения или ссылки на метод. В результате создаётся новый поток, содержащий преобразованные элементы, без изменения исходного потока.

```java
Stream<Integer> lengthsStream = words.stream()
        .map(String::length);
```


## Терминальные

### `сollect(Collector)`

> Он применяется для преобразования элементов потока в определённую структуру данных (например, `List`, `Set`, `Map`), строку или агрегированное значение.

Этот метод принимает объект типа `Collector`, который определяет, как именно будут собраны элементы.

- [[Класс Collector]]

## Stateless-Stateful

### Stateless (бессостоящие) операции

> Операции, которые обрабатывают каждый элемент потока независимо друг от друга, не сохраняя состояние.

- `filter()`, `map()`, `flatMap()`, `forEach()`, `peek()`
- Могут эффективно обрабатываться параллельно, поскольку каждая операция независима от остальных.

### Stateful (состоящие) операции:

> Операции, которым нужно знать больше, чем один элемент потока для принятия решения. Они могут сохранять промежуточное состояние.

- `distinct()`, `sorted()`, `limit()`, `skip()`
- Для выполнения таких операций необходимо проанализировать все или часть элементов потока, чтобы получить результат.


# Вопросы

## Когда использовать Stream API?

Императивный цикл for намного мощнее чем, обычная цепочка вызовов Stream API.

Ты можешь:
- вернуться из цикла любой вложенности в любой момент времени
- изменять данные как угодно и где угодно (а не только affectively final)
- создавать несколько выходных результатов из цикла (а не только один)
- гибко обрабатывать и пробрасывать исключения
- использовать if, else и как угодно еще изменять ход выполнения программы

В свою очередь Stream API навязывает множество ограничений на то, что ты можешь делать:
- ты можешь определить только последовательный список шагов без использования сложных структур данных и операций ветвлений.

Такие ограничения означают, что практически невозможно написать сложный алгоритм, используя Stream API. Либо этот алгоритм будет выглядить очень громоздко, что его проще будет переписать на императивный цикл for.

С другой стороны, благодаря таким ограничениям более простые алгоритмы читаются намного лучше и приятнее программистами. 

Если программист знаком со Stream API - то по такому коду невероятно быстро схватываешь его суть, что он делает.

```java
List<CurrencyDto> currencies = new ArrayList<>();
for (Currency currency : page.getContent()) {
	currencies.add(CurrencyMapper.INSTANCE.conversionCurrencyToDTO(currency));
}
```

И вот как его преобразовывает идея автоматически(alt + enter и выбрать collapse loop):

```java
List<CurrencyDto> currencies = page.getContent()
         .stream()
         .map(CurrencyMapper.INSTANCE::conversionCurrencyToDTO)
         .collect(Collectors.toList());
```

Читабельнее и легче + не нужно инициализировать список самим.

Поэтому: **Если задача может быть представлена в виде последовательной цепочки шагов, то обязательно используй Stream API**.

## Когда избегать Stream API

- Основное правило:
> Если комплексный императивный цикл for сложно преобразовать в Stream (особенно если алгоритм этого цикла делает кучу “side effects”) - ничего страшного, продолжай использовать императивный стиль.

- Программисты часто впадают в две крайности: либо пишут все в виде стримов, либо не используют их вовсе. Но как обычно это и бывает - правда где-то по середине. Другими словами: сила в балансе между двумя подходами и умении их комбинировать. Какие-то задачи лучше решить через Stream API, какие-то в императивном стиле. 

- Когда человек начинает использовать стримы - он начинает прокачивать навык декомпозиции задачи на более мелкие составляющие, что в последующем влияет на декомпозицию задач любой сложности и любого уровня. Ибо в противном случае не получится компактно и понятно представить логику в виде стримов.

- Также на своем опыте скажу, что API более высокого уровня лучше определять с помощью стримов, и только уходя вглубь - прибегать к императивному стилю, если потребуется. Потому что это очень сильно облегчает чтение программного кода, ибо исследуется все "сверху-вниз" как в книге.

Роб Винч:
> В то время как некоторые предпочитают удобочитаемость использования Stream API, накладные расходы на сборку мусора из-за создания дополнительных объектов (включая промежуточные объекты) могут вызвать значительное снижение производительности. Существуют простые бенчмарки, которые иллюстрируют эту проблему. Мы должны заменить использование Stream на циклы for во всем коде Spring Security.

- Но стоит обратить внимание, что это spring security - фреймворк, который используется по всему миру в разных приложениях. Если ваше приложение не подходит под разряд performance critical - вы можете спокойно закрыть на это глаза.

## Обработка исключений

- Using _try-catch_ in Lambda Expression

```java
List<String> fileContents = pathList.stream()
    .map(path -> {
      try {
        return Files.readString(path);
      } catch (IOException e) {
        return null;
      }
    })
    .filter(Objects::nonNull)
    .toList();
```

**Using _try-catch_ in lambda expressions is an anti-pattern because _try-catch_ dictates what happens to Stream elements rather than the result of the previous operation in the Stream**.

-  Using Safe Method Extraction

```java
public class FileUtils {

	public static String safeReadString(Path path) {
		try {
		  return Files.readString(path);
		} catch (IOException e) {
		  return null;
		}
	}
}
```

```java
List<String> fileContents = pathList.stream()
    .map(FileUtils::safeReadString)
    .filter(Objects::nonNull)
    .toList();
```

- Not Throwing the Exception

```java
public class FileUtils {

	static Optional<String> readString(Path path) {
    try {
      return Optional.of(Files.readString(path));
    } catch (IOException e) {
      return Optional.empty();
    }
  }
}

List<Optional<String>> fileContentsList = pathList.stream()
    .map(HandleCheckedExceptions::readString)
    .toList();
```

- Using _FailableStream_ from Apache Commons Lang

```java
List<String> fileContents = **Streams.stream**(pathList.stream())
    .map(Files::readString)
    .collect(Collectors.toList());

<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.12.0</version>
</dependency>
```

# Ресурсы

- [Handle Exceptions Thrown in Java Streams](https://howtodoinjava.com/java/stream/handle-exceptions-in-stream/)
- [Глубокое погружение в Stream API Java: Понимание и Применение](https://struchkov.dev/blog/ru/java-stream-api/#сollectcollector)
- 
