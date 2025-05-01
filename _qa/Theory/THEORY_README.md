#### Что храниться в MetaSpace

- **Метаданные классов** (Class metadata):
    - Структуры классов (имена, методы, поля, аннотации).
    - Bytecode методов.
    - Константы (String Pool, кроме примитивов).
- **Загруженные ClassLoader'ы**.
- **JIT-оптимизированный код** (в некоторых реализациях JVM).
- Metaspace использует **нативную память** (не ограничена `-Xmx`).
- Лимит задаётся флагами:
    -XX:MaxMetaspaceSize=256m  # Максимальный размер
    -XX:MetaspaceSize=64m      # Начальный размер
#### Какие нормальные формы знаешь?

1. **1НФ**: Все атрибуты атомарны (нет массивов/списков в ячейках).
2. **2НФ**: 1НФ + нет частичных зависимостей от ключа (все поля зависят от всего PK).
3. **3НФ**: 2НФ + нет транзитивных зависимостей (поля зависят только от PK).
4. **BCNF (Бойса-Кодда)**: Усиленная 3НФ (нет зависимостей атрибутов PK от не-PK полей).
5. **4НФ**: Нет многозначных зависимостей.
6. **5НФ**: Нет зависимостей соединения.

#### Какие алгоритмы поиска знаешь?
- **Линейный поиск** (`O(n)`) — перебор элементов.
- **Бинарный поиск** (`O(log n)`) — для отсортированных массивов.
- **Поиск в хеш-таблицах** (`O(1)` в среднем) — по ключу.
- **Поиск в деревьях**:
    - Двоичное дерево (`O(n)` в худшем случае).
    - AVL/Красно-черное дерево (`O(log n)`).
- **Поиск в графах**:
    - BFS (поиск в ширину).
    - DFS (поиск в глубину).
#### Какие основные классы в Spring Security знаешь?

SecurityContextHolder

```java
Authentication auth = SecurityContextHolder.getContext().getAuthentication();
```

**`Authentication`** Представляет аутентифицированного пользователя:
- `Principal` (логин пользователя)
- `Credentials` (пароль)
- `Authorities` (роли/права)
    
**`AuthenticationManager`** Интерфейс для аутентификации:
```java
Authentication authenticate(Authentication auth) throws AuthenticationException;
```

**`UserDetailsService`** Загружает пользователя по имени (например, из БД):

```java
UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
```

Загружает пользователя по имени (например, из БД):
#### Какая ошибка при изменении коллекции от List.of() 
UnsupportedOperationException
#### Что такое force push

- git push --force  # Перезаписывает историю удалённого репозитория.
- **Опасность**: Может удалить коммиты других разработчиков.
- Аналог: git push --force-with-lease  # Отменяет push, если ветка изменилась.
#### Настройки памяти и gc (-Xms, -Xmx, -Xss, -XX:+Use...)

```bash
-Xms256m  # Начальный размер хипа
-Xmx1024m # Максимальный размер
```

```bash
-XX:+UseG1GC            # Garbage-First (по умолчанию с Java 9)
-XX:+UseParallelGC      # Parallel GC
-XX:+UseConcMarkSweepGC # CMS (устарел)
-XX:+UseZGC             # ZGC (для больших хипов)
```
#### Из чего состоит стэк?

- **Фреймы стека** (Stack Frames) — на каждый вызов метода:
    1. **Локальные переменные**.
    2. **Операндный стек** (для вычислений).
    3. **Ссылка на текущий класс** (для `this`).
    4. **Возвращаемое значение**.
    5. **Ссылка на константы**.
#### Что такое on update, on delete

- **`ON DELETE CASCADE`**: Удаляет дочерние записи при удалении родителя.
- **`ON DELETE SET NULL`**: Заменяет внешний ключ на `NULL`.
- **`ON UPDATE CASCADE`**: Обновляет дочерние записи при изменении родителя.

```sql
CREATE TABLE Orders (
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES Users(id) 
        ON DELETE CASCADE
        ON UPDATE SET NULL
);
```
#### LEFT JOIN vs LEFT OUTER JOIN

- **`LEFT JOIN`** и **`LEFT OUTER JOIN`** — это одно и то же.
- Возвращает **все строки из левой таблицы** + совпадения из правой.
- Если совпадений нет, правые поля будут `NULL`.
#### Допускает ли UNIQUE значения множество null значения, как не не допускать несколько null значений

- **`UNIQUE`**: Запрещает дубликаты, но разрешает один `NULL`.
- **`UNIQUE + NOT NULL`**: Запрещает и дубликаты, и `NULL`.
#### В чем разница DDL, DML, DCL

- **DDL** (Data Definition): `CREATE`, `ALTER`, `DROP`
- **DML** (Data Manipulation): `SELECT`, `INSERT`, `UPDATE`, `DELETE`
- **DCL** (Data Control): `GRANT`, `REVOKE`

#### Какие есть триггеры в SQL?

```SQL
CREATE TRIGGER log_update 
AFTER UPDATE ON orders FOR EACH ROW 
INSERT INTO audit_log VALUES(NEW.id, NOW());
```
#### Что такое CTE (with) in SQL

Временный результат для сложных запросов:

```sql
WITH temp AS (SELECT * FROM users WHERE age > 18) 
SELECT * FROM temp;
```
#### Char vs varchar

- **CHAR(10)** — всегда 10 символов (дополняет пробелами).
- **VARCHAR(10)** — до 10 символов (экономит место).
#### Виды оконных функций

- `ROW_NUMBER()` — нумерация строк.
- `RANK()` — ранжирование с пропусками.
- `SUM() OVER (PARTITION BY group)` — сумма по группам.
#### Почему Perm Gen был изменен на Metaspace?

- **PermGen**: Фиксированный размер, OutOfMemoryError.
- **Metaspace**: Автомасштабирование в нативной памяти.
#### Многопоточные коллекции

- `ConcurrentHashMap`
- `CopyOnWriteArrayList`
- `BlockingQueue`

#### Как работает CopyOnArrayList

- При **записи** создается **новая копия** массива (старый массив остается для чтения).
- Чтение **без блокировок** (из "старого" массива).
- Запись **с блокировкой** (чтобы избежать конфликтов).

```java
// Псевдокод
volatile Object[] array = new Object[10];

void add(Object item) {
    lock();
    Object[] newArray = Arrays.copyOf(array, array.length + 1);
    newArray[newArray.length - 1] = item;
    array = newArray; // Атомарная замена ссылки
    unlock();
}
```
#### Что такое Json Schema?

Схема для валидации JSON

```json
{
  "type": "object",
  "properties": {
    "age": { "type": "number", "minimum": 18 }
  }
}
```

#### Поле `type` в JSON Schema

```json
{
  "type": "string"  // Допустимые: "object", "array", "number", "boolean", "null"
}
```

#### Валидация в Open API

OpenAPI (Swagger) позволяет валидировать структуру и типы данных в запросах/ответах.

```yaml
components:
  schemas:
    UserDTO:
      type: object
      properties:
        id: 
          type: integer
          format: int64
        name: 
          type: string
          minLength: 1
      required: [id, name]
```

#### Что такое кафка Streams?

Библиотека для обработки потоков данных в Kafka (аналогично стримам в Java).

#### Плюсы gRPC

1. Скорость (бинарный Protocol Buffers).
2. Автогенерация кода.
3. Поддержка потоков. (One to One, One to Many and etc.)
4. HTTP/2.


