---
Определение: Он используется для модульного тестирования и помогает изолировать классы от внешних зависимостей, таких как базы данных, веб-сервисы и другие компоненты, позволяя сосредоточиться на логике тестируемого класса.
Примечание:
---
# Основные концепции

1. **Mock-объекты**: Mock — это **поддельный объект**, который имитирует поведение реального объекта. С помощью Mockito можно создавать mock-объекты, настраивать их поведение и проверять взаимодействие с ними.
2. **Stubbing**: Stubbing — это процесс задания ожидаемого поведения для mock-объекта. Например, если ты вызываешь метод и хочешь задать, какое значение он должен возвращать.
3. **Verification**: Верификация — это проверка, что определенные методы были вызваны с ожидаемыми параметрами.
# Как работает Mockito

#### 1. Создание Mock-объекта

Mockito позволяет создавать mock-объекты для классов, которые ты хочешь подделать в тестах.

Пример:

```java
    @Test
    public void testMockito() {
        // Создаем mock-объект для интерфейса List
        List<String> mockedList = mock(List.class);

        // Настройка поведения mock-объекта (stubbing)
        when(mockedList.get(0)).thenReturn("Hello Mockito");

        // Используем mock-объект
        System.out.println(mockedList.get(0));  // Выведет "Hello Mockito"

        // Проверка взаимодействия
        verify(mockedList).get(0);  // Проверка, что метод get(0) был вызван
    }
```

#### 2. Stubbing (Настройка поведения методов)

Ты можешь указать Mockito, что должно происходить, когда вызываются определенные методы.

```java
when(mockedObject.someMethod()).thenReturn(someValue);
```

Пример:

```java
List<String> mockedList = mock(List.class);

// Когда вызывается mockedList.get(0), вернуть "Hello"
when(mockedList.get(0)).thenReturn("Hello");
```

Можно также настроить исключения:

```java
when(mockedList.get(1)).thenThrow(new RuntimeException("Error"));
```

#### 3. Verification (Проверка вызовов)

Mockito позволяет проверять, что определенные методы были вызваны или не вызваны.

```java
verify(mockedList).get(0);  // Проверяет, что метод get(0) был вызван хотя бы раз
verify(mockedList, times(2)).get(0);  // Проверяет, что метод был вызван 2 раза
verify(mockedList, never()).get(1);  // Проверяет, что метод get(1) не был вызван
```

#### 4. Аргументы матчеры (Argument Matchers)

Mockito позволяет использовать специальные матчеры для аргументов, если ты не хочешь точно указывать значения параметров.

```java
import static org.mockito.ArgumentMatchers.anyInt;

when(mockedList.get(anyInt())).thenReturn("Matched any int");
```

# Продвинутые возможности Mockito

## 1. Spy (Шпион)

Шпион — это объект, который позволяет отслеживать реальное поведение объекта, но при этом можно "подделать" определенные его методы.

Пример:

```java
List<String> list = new ArrayList<>();
List<String> spyList = spy(list);

// Подделка одного метода
doReturn("Hello").when(spyList).get(0);

// Реальное поведение остальных методов
spyList.add("World");
System.out.println(spyList.get(0));  // Выведет "Hello"
System.out.println(spyList.get(1));  // Выведет "World"
```

## 2. Аргументы захвата (Argument Captor)

Mockito позволяет захватывать аргументы методов для проверки их значений.

```java
import org.mockito.ArgumentCaptor;

ArgumentCaptor<String> captor = ArgumentCaptor.forClass(String.class);
verify(mockedList).add(captor.capture());
System.out.println(captor.getValue());  // Выведет значение, переданное в add()
```

## 3. Mocking `void` методов

Для методов, которые ничего не возвращают (`void`), можно использовать `doNothing()`, `doThrow()`, и другие действия:

```java
doNothing().when(mockedObject).someVoidMethod();
doThrow(new RuntimeException()).when(mockedObject).someVoidMethod();
```

# Интеграция с JUnit

Mockito тесно интегрируется с JUnit, что делает тестирование более удобным.

Пример использования аннотации `@Mock` и `@InjectMocks` для автоматической генерации mock-объектов:

```java
    @Mock
    private MyDependency myDependency;

    @InjectMocks
    private MyService myService;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);
    }

    @Test
    public void testServiceMethod() {
        when(myDependency.someMethod()).thenReturn("Mocked value");

        String result = myService.callDependency();
        assertEquals("Mocked value", result);
    }
```

# Когда использовать Mockito

Mockito лучше всего подходит для **модульного тестирования**, когда нужно протестировать отдельные классы или методы без реальных зависимостей. Это позволяет имитировать окружение и настраивать поведение внешних систем (базы данных, API и т.д.) для более точных и изолированных тестов.

# Mock vs Spy

- Mock: Все методы подлежат подмене, если вызвать метод, для которого не определено поведение, вернется default значение.
- Spy: Только указанные методы подменяются, остальные работают как обычно.

```java
// Mock Example
MyService mockService = Mockito.mock(MyService.class);
Mockito.when(mockService.getData()).thenReturn("Mocked Data");

System.out.println(mockService.getData()); // Выведет: "Mocked Data"

// Spy Example
MyService realService = new MyService();
MyService spyService = Mockito.spy(realService);

Mockito.when(spyService.getData()).thenReturn("Mocked Data"); // Подменяем только этот метод

System.out.println(spyService.getData()); // Выведет: "Mocked Data"
System.out.println(spyService.getOtherData()); // Выведет реальный результат метода
```