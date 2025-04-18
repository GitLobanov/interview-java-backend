### 1. **`@Controller`**

Аннотация **`@Controller`** используется для определения контроллера, который возвращает **вид** (view) как ответ на HTTP-запросы. Она часто используется в традиционных веб-приложениях, где важно передавать данные в шаблоны (например, JSP, Thymeleaf) для генерации HTML-страниц.

```java
@Controller
public class HomeController {

    @GetMapping("/home")
    public String home(Model model) {
        model.addAttribute("message", "Welcome to Nexora!");
        return "home"; // Возвращает название HTML-страницы или другого вида
    }
}
```

В этом примере:

- Контроллер отвечает за обработку GET-запросов по `/home`.
- Возвращает строку `"home"`, которая указывает, какой шаблон или вид отрендерить.
- **`Model`** используется для передачи данных (в данном случае `"message"`) в представление.

**Основная задача `@Controller`:**

- Возвращать представление (HTML, JSP, Thymeleaf и др.).
- Используется для традиционных веб-приложений с серверной генерацией страниц.

### 2. **`@RestController`**

**`@RestController`** — это специальная версия **`@Controller`**, которая автоматически добавляет аннотацию **`@ResponseBody`** ко всем методам. Это означает, что вместо возврата представления (HTML), данные возвращаются напрямую в виде JSON, XML или других форматов.

```java
@RestController
public class ApiController {

    @GetMapping("/api/message")
    public Map<String, String> getMessage() {
        Map<String, String> response = new HashMap<>();
        response.put("message", "Welcome to the Nexora API!");
        return response; // Возвращает JSON-ответ
    }
}
```

В этом примере:

- Контроллер возвращает JSON-ответ на GET-запрос по пути `/api/message`.
- **`@RestController`** автоматически преобразует возвращаемый объект (в данном случае Map) в JSON-ответ.

**Основная задача `@RestController`:**

- Обрабатывать запросы для RESTful API.
- Возвращать данные в виде JSON или других форматов, вместо представления.

### Основные различия между **`@Controller`** и **`@RestController`**:

|Характеристика|`@Controller`|`@RestController`|
|---|---|---|
|Возвращаемый тип данных|Возвращает вид (HTML, JSP, и т.д.)|Возвращает JSON, XML и другие данные|
|Аннотация `@ResponseBody`|Не используется по умолчанию|Автоматически добавляется ко всем методам|
|Основное применение|Традиционные веб-приложения|RESTful веб-службы|
### Когда использовать **`@Controller`**:

- Когда нужно вернуть HTML-страницу или другой вид (например, с помощью шаблонизатора).
- Применяется в MVC-приложениях (Model-View-Controller).

### Когда использовать **`@RestController`**:

- Когда создается REST API, где ответы — это данные в формате JSON, XML и т.п.
- Для микросервисов и приложений с фронтендом на React, Angular и других фреймворках, где необходим обмен данными через API.

### Пример использования в проекте Nihongo Nexus:

**Контроллер для фронтенда (с использованием `@Controller`):**

```java
@Controller
public class VocabularyController {

    @GetMapping("/vocabulary")
    public String vocabulary(Model model) {
        List<String> words = List.of("こんにちは", "ありがとう", "さようなら");
        model.addAttribute("words", words);
        return "vocabulary"; // Возвращает HTML страницу с отображением слов
    }
}
```

Контроллер для API (с использованием `@RestController`):

```java
@RestController
public class VocabularyApiController {

    @GetMapping("/api/vocabulary")
    public List<String> getVocabulary() {
        return List.of("こんにちは", "ありがとう", "さようなら"); // Возвращает JSON
    }
}
```

В этих примерах:

- Первый контроллер возвращает страницу с HTML, отображающую список японских слов.
- Второй контроллер возвращает этот же список в формате JSON для использования во фронтенд-приложении или мобильном клиенте.
