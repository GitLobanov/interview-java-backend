В Spring Framework для определения маршрутов (routes) HTTP-запросов используются аннотации. Эти аннотации позволяют указать, какой метод контроллера должен обрабатывать запросы на определенные URL. Основные аннотации для маршрутизации — это **`@RequestMapping`**, **`@GetMapping`**, **`@PostMapping`**, **`@PutMapping`**, **`@DeleteMapping`** и другие.

### 1. **`@RequestMapping`**

**`@RequestMapping`** — это наиболее общая аннотация для определения маршрутов. Она может использоваться для обработки любого типа HTTP-запросов, но для этого нужно вручную указать метод запроса (GET, POST и т.д.).

Эту аннотацию можно применять как на уровне класса, так и на уровне метода. На уровне класса она определяет базовый URL, а на уровне метода — конкретный путь для обработки запроса.

#### Пример:

```java
@Controller
@RequestMapping("/users")
public class UserController {

    @RequestMapping(value = "/list", method = RequestMethod.GET)
    public String getUserList(Model model) {
        // Логика получения списка пользователей
        return "userList"; // Возвращает название шаблона
    }

    @RequestMapping(value = "/create", method = RequestMethod.POST)
    public String createUser(@ModelAttribute User user) {
        // Логика создания нового пользователя
        return "userCreated";
    }
}
```

В этом примере:

- **`@RequestMapping("/users")`** на уровне класса указывает базовый URL `/users`.
- **`@RequestMapping(value = "/list", method = RequestMethod.GET)`** указывает, что метод `getUserList()` обрабатывает GET-запросы на `/users/list`.
- **`@RequestMapping(value = "/create", method = RequestMethod.POST)`** указывает, что метод `createUser()` обрабатывает POST-запросы на `/users/create`.

### 2. **`@GetMapping`**

**`@GetMapping`** — это специальная аннотация для обработки GET-запросов. Она является сокращением от **`@RequestMapping(method = RequestMethod.GET)`** и используется для запросов, которые запрашивают данные (например, отображение страницы, получение списка объектов и т.д.).

#### Пример:

```java
@RestController
@RequestMapping("/api")
public class ProductController {

    @GetMapping("/products")
    public List<Product> getProducts() {
        // Логика получения списка продуктов
        return productService.getAllProducts();
    }
}
```

В этом примере:

- **`@GetMapping("/products")`** обрабатывает GET-запросы на `/api/products` и возвращает список продуктов в формате JSON.

### 3. **`@PostMapping`**

**`@PostMapping`** используется для обработки POST-запросов. POST-запросы обычно применяются для создания новых ресурсов или передачи данных на сервер.

#### Пример:

```java
@RestController
@RequestMapping("/api")
public class ProductController {

    @PostMapping("/products")
    public Product createProduct(@RequestBody Product product) {
        // Логика создания нового продукта
        return productService.saveProduct(product);
    }
}
```

В этом примере:

- **`@PostMapping("/products")`** обрабатывает POST-запросы на `/api/products` и создает новый продукт, принимая его данные в виде JSON с помощью **`@RequestBody`**.

### 4. **`@PutMapping`**

**`@PutMapping`** используется для обработки PUT-запросов, которые обычно применяются для обновления существующих ресурсов.

#### Пример:

```java
@RestController
@RequestMapping("/api")
public class ProductController {

    @PutMapping("/products/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product product) {
        // Логика обновления продукта по ID
        return productService.updateProduct(id, product);
    }
}
```

В этом примере:

- **`@PutMapping("/products/{id}")`** обрабатывает PUT-запросы на `/api/products/{id}` для обновления продукта с указанным `id`.

### 5. **`@DeleteMapping`**

**`@DeleteMapping`** используется для обработки DELETE-запросов, которые удаляют существующие ресурсы.

#### Пример:

```java
@RestController
@RequestMapping("/api")
public class ProductController {

    @DeleteMapping("/products/{id}")
    public void deleteProduct(@PathVariable Long id) {
        // Логика удаления продукта по ID
        productService.deleteProduct(id);
    }
}
```

В этом примере:

- **`@DeleteMapping("/products/{id}")`** обрабатывает DELETE-запросы на `/api/products/{id}` для удаления продукта с указанным `id`.

### 6. Другие аннотации для маршрутизации

- **`@PatchMapping`** — используется для обработки PATCH-запросов, которые применяются для частичного обновления ресурсов.
    
#### Пример:

```java
@PatchMapping("/products/{id}")
public Product patchProduct(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
    // Логика частичного обновления продукта
    return productService.patchProduct(id, updates);
}
```

- **`@RequestParam`** — используется для захвата параметров запроса (например, ?name=value).
#### Пример:

```java
@GetMapping("/search")
public List<Product> searchProducts(@RequestParam String name) {
    // Логика поиска продуктов по имени
    return productService.searchByName(name);
}
```


- **`@PathVariable`** — используется для захвата переменных пути (например, /products/{id}).

#### Пример:

```java
@GetMapping("/products/{id}")
public Product getProductById(@PathVariable Long id) {
    // Логика получения продукта по ID
    return productService.getProductById(id);
}
```


