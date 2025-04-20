### Почему используют `JdbcTemplate`?

1. **Упрощает код**: Обычная работа с JDBC требует большого количества шаблонного кода для управления соединениями, подготовленных выражений и обработки исключений. `JdbcTemplate` берет на себя эту работу, позволяя сосредоточиться только на написании SQL-запросов и бизнес-логике.
    
2. **Управление ресурсами**: Автоматически управляет открытием и закрытием ресурсов, таких как `Connection`, `Statement` и `ResultSet`, что помогает избежать утечек ресурсов.
    
3. **Обработка исключений**: `JdbcTemplate` оборачивает все SQL-исключения в объекты `DataAccessException`, что делает обработку ошибок более удобной и унифицированной.
    
4. **Поддержка транзакций**: Легко интегрируется с системой транзакций Spring, что позволяет управлять транзакциями без написания дополнительного кода.
    
5. **Гибкость**: Позволяет использовать как простые SQL-запросы, так и сложные вызовы хранимых процедур, а также может быть расширен кастомными обработчиками.
    
6. **Работа с параметрами**: `JdbcTemplate` поддерживает параметризированные запросы, что делает код более безопасным и устойчивым к SQL-инъекциям.

### Пример использования `JdbcTemplate`

#### 1. Простое выполнение запроса для получения данных

```java
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Repository;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

@Repository
public class UserDao {

    private final JdbcTemplate jdbcTemplate;

    // Внедрение зависимости JdbcTemplate через конструктор
    public UserDao(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    // Получение всех пользователей
    public List<User> getAllUsers() {
        String sql = "SELECT id, username, email FROM users";
        return jdbcTemplate.query(sql, new UserRowMapper());
    }

    // RowMapper для преобразования строки ResultSet в объект User
    private static class UserRowMapper implements RowMapper<User> {
        @Override
        public User mapRow(ResultSet rs, int rowNum) throws SQLException {
            User user = new User();
            user.setId(rs.getLong("id"));
            user.setUsername(rs.getString("username"));
            user.setEmail(rs.getString("email"));
            return user;
        }
    }
}
```

В этом примере `JdbcTemplate` используется для выполнения запроса на выборку данных из таблицы `users`. Метод `query` выполняет SQL-запрос и использует `RowMapper` для преобразования каждой строки результата в объект `User`.

#### 2. Вставка данных с помощью `JdbcTemplate`

```java
public void insertUser(User user) {
    String sql = "INSERT INTO users (username, email) VALUES (?, ?)";
    jdbcTemplate.update(sql, user.getUsername(), user.getEmail());
}
```

Этот метод выполняет SQL-запрос для вставки нового пользователя в базу данных. Параметры передаются в запрос через знак вопроса `?`, и `JdbcTemplate` автоматически подставляет их.

#### 3. Обновление данных

```java
public void updateUserEmail(Long id, String newEmail) {
    String sql = "UPDATE users SET email = ? WHERE id = ?";
    jdbcTemplate.update(sql, newEmail, id);
}
```

Здесь происходит обновление электронной почты пользователя по его `id`. Опять же, `JdbcTemplate` управляет подстановкой параметров и выполнением SQL-запроса.

#### 4. Удаление данных

```java
public void deleteUser(Long id) {
    String sql = "DELETE FROM users WHERE id = ?";
    jdbcTemplate.update(sql, id);
}
```


Простой запрос на удаление пользователя из базы данных по его идентификатору.

### Продвинутые возможности `JdbcTemplate`

#### 1. Использование транзакций

Spring легко интегрирует поддержку транзакций с `JdbcTemplate` через аннотацию `@Transactional`. Например:

```java
import org.springframework.transaction.annotation.Transactional;

@Transactional
public void transferMoney(Long fromUserId, Long toUserId, BigDecimal amount) {
    // Снятие денег с одного счета
    jdbcTemplate
	    .update("UPDATE accounts SET balance = balance - ? WHERE user_id = ?", amount, fromUserId);

    // Добавление денег на другой счет
    jdbcTemplate
	    .update("UPDATE accounts SET balance = balance + ? WHERE user_id = ?", amount, toUserId);
}
```

#### 2. Использование хранимых процедур

```java
public void callStoredProcedure(Long userId) {
    String sql = "{call update_user_status(?)}";
    jdbcTemplate.update(sql, userId);
}
```

В этом примере вызывается хранимая процедура для обновления статуса пользователя.
#### 3. Получение значений из базы с помощью метода `queryForObject`

```java
public int getUserCount() {
    String sql = "SELECT COUNT(*) FROM users";
    return jdbcTemplate.queryForObject(sql, Integer.class);
}
```


Здесь `JdbcTemplate` выполняет SQL-запрос, который возвращает количество пользователей в таблице, и преобразует результат в объект типа `Integer`.

### Реализация в проекте Nihongo Nexus

В проекте Nihongo Nexus, где нужно управлять базой данных с информацией о словах и их переводах, `JdbcTemplate` может использоваться для создания, обновления и выборки данных о словах.

```java
public class VocabularyRepository {

    private final JdbcTemplate jdbcTemplate;

    public VocabularyRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public void addWord(String word, String translation) {
        String sql = "INSERT INTO vocabulary (word, translation) VALUES (?, ?)";
        jdbcTemplate.update(sql, word, translation);
    }

    public List<Vocabulary> getAllWords() {
        String sql = "SELECT * FROM vocabulary";
        return jdbcTemplate.query(sql, new RowMapper<Vocabulary>() {
            @Override
            public Vocabulary mapRow(ResultSet rs, int rowNum) throws SQLException {
                Vocabulary vocab = new Vocabulary();
                vocab.setWord(rs.getString("word"));
                vocab.setTranslation(rs.getString("translation"));
                return vocab;
            }
        });
    }
}
```


### Когда следует использовать `JdbcTemplate`?

- **Маленькие и средние приложения**: Если требуется простая работа с базой данных без сложной ORM (Object-Relational Mapping) или избыточных абстракций.
- **Высокая производительность**: В случаях, когда требуется максимальная производительность без накладных расходов на ORM, `JdbcTemplate` может быть отличным выбором.
- **Легкость тестирования**: `JdbcTemplate` легко мокировать для тестов, что делает его хорошим выбором для приложений с высокой степенью тестирования.

