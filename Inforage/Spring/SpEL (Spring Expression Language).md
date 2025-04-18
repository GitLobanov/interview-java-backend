---
Определение: Мощный язык выражений, встроенный в Spring Framework, который позволяет динамически оценивать выражения внутри конфигураций, аннотаций и в бинах.
Примечание: SpEL используется для управления данными, выполнения операций и взаимодействия с бинами напрямую через конфигурацию. Способен работать с методами, полями и объектами Java прямо в выражениях.
---
### Примеры

1. **Управление доступом и проверка ролей в аннотациях**: SpEL может использоваться для динамической проверки ролей или разрешений в приложениях, которые управляют доступом к различным уровням данных.
    
Пример:

```java
@PreAuthorize("hasRole('ADMIN') and #user.name == authentication.name")
public void deleteUser(User user) {
    // метод только для администратора, и пользователя, который удаляет себя
}
```

2. **Динамическая настройка конфигурации**: SpEL используется для изменения конфигураций приложения в зависимости от состояния системы или внешних условий.

Пример:

```java
@Value("#{systemProperties['os.name'] == 'Windows 10' ? 'windowsService' : 'unixService'}")
private MyService myService;
```

3. **Вычисление и взаимодействие с бинами**: SpEL позволяет вызывать методы бинов, работать с их полями и свойствами напрямую из конфигурационных файлов или аннотаций.

Пример:

```java
@Value("#{myBean.someMethod()}")
private String result;
```

### Дополнительные примеры из систем

#### Пример 1: Управление уровнем сложности в `Nihongo Nexus`

**Сценарий:** В проекте `Nihongo Nexus`, который включает изучение японского языка через игровые элементы, необходимо динамически изменять уровень сложности задач в зависимости от уровня пользователя. Для этого можно использовать SpEL для настройки уровня сложности на основе уровня прогресса пользователя.

```java
@Component
public class DifficultyManager {

    @Value("#{userProgress.level > 5 ? 'Advanced' : 'Beginner'}")
    private String difficultyLevel;

    public String getDifficultyLevel() {
        return difficultyLevel;
    }

    // Метод для изменения сложности
    public void adjustDifficulty(UserProgress userProgress) {
        ExpressionParser parser = new SpelExpressionParser();
        String expression = userProgress.getLevel() > 5 ? "Advanced" : "Beginner";
        this.difficultyLevel = parser.parseExpression(expression).getValue(String.class);
    }
}
```

Объяснение:

- SpEL Выражение: `#{userProgress.level > 5 ? 'Advanced' : 'Beginner'}`
- Цель: Определяет уровень сложности на основе уровня прогресса пользователя. Если уровень пользователя больше 5, устанавливается "Advanced", иначе — "Beginner".

Целесообразность: СпEL позволяет динамически адаптировать уровень сложности в зависимости от данных, что делает систему более гибкой и персонализированной.

#### Пример 2: Определение доступности функциональности в приложении трекинга времени

Сценарий: В приложении трекинга времени задач нужно включить или отключить определенные функции в зависимости от времени работы пользователя. СпEL может помочь в управлении доступностью функций в реальном времени.

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;
import org.springframework.stereotype.Component;

import java.time.LocalTime;

@Component
public class FeatureAccessManager {

    @Value("#{T(java.time.LocalTime).now().isAfter(T(java.time.LocalTime).parse('08:00')) and T(java.time.LocalTime).now().isBefore(T(java.time.LocalTime).parse('18:00')) ? true : false}")
    private boolean isFeatureAvailable;

    public boolean isFeatureAvailable() {
        return isFeatureAvailable;
    }

    // Метод для проверки доступности функции
    public void checkFeatureAvailability() {
        ExpressionParser parser = new SpelExpressionParser();
        LocalTime now = LocalTime.now();
        boolean isAvailable = parser.parseExpression("T(java.time.LocalTime).now().isAfter(T(java.time.LocalTime).parse('08:00')) and T(java.time.LocalTime).now().isBefore(T(java.time.LocalTime).parse('18:00'))").getValue(Boolean.class);
        this.isFeatureAvailable = isAvailable;
    }
}
```

#### Пример 3: Адаптация настроек языка в `Nihongo Nexus`

**Сценарий:** В `Nihongo Nexus` можно использовать SpEL для динамической адаптации настроек языка в зависимости от выбранных пользователем предпочтений.

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;
import org.springframework.stereotype.Component;

@Component
public class LanguageSettingsManager {

    @Value("#{userPreferences.language == 'Japanese' ? 'Japanese' : 'English'}")
    private String language;

    public String getLanguage() {
        return language;
    }

    // Метод для обновления настроек языка
    public void updateLanguageSettings(UserPreferences userPreferences) {
        ExpressionParser parser = new SpelExpressionParser();
        String languageSetting = parser.parseExpression("userPreferences.language == 'Japanese' ? 'Japanese' : 'English'").getValue(userPreferences, String.class);
        this.language = languageSetting;
    }
}
```

Объяснение:

- SpEL Выражение: `#{userPreferences.language == 'Japanese' ? 'Japanese' : 'English'}`
- Цель: Настраивает язык приложения в зависимости от предпочтений пользователя.

Целесообразность: Позволяет динамически адаптировать настройки языка в зависимости от предпочтений пользователя, улучшая пользовательский опыт и персонализацию.

