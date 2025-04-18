### Основные виды репозиториев в Spring Data JPA:

1. **`CrudRepository<T, ID>`**
2. **`PagingAndSortingRepository<T, ID>`**
3. `JpaRepository<T, ID>`
4. QueryDslJpaRepository
5. SimpleJpaRepository
6. Кастомные репозитории

### 1. **`CrudRepository<T, ID>`**

`CrudRepository` — это базовый интерфейс для работы с данными. Он предоставляет основные CRUD (Create, Read, Update, Delete) операции для сущности.

#### Основные методы:

- **`save(S entity)`** — сохраняет или обновляет сущность.
- **`findById(ID id)`** — находит сущность по её ID.
- **`findAll()`** — возвращает все записи.
- **`deleteById(ID id)`** — удаляет сущность по её ID.
- **`deleteAll()`** — удаляет все записи.

### 2. **`PagingAndSortingRepository<T, ID>`**

`PagingAndSortingRepository` расширяет `CrudRepository` и добавляет возможности для пагинации и сортировки результатов.

#### Дополнительные методы:

- **`findAll(Sort sort)`** — возвращает все записи с заданной сортировкой.
- **`findAll(Pageable pageable)`** — возвращает страницу данных с заданной пагинацией.

Пример использования:

```java
@Service
public class ProductService {

    @Autowired
    private ProductRepository productRepository;

    public Page<Product> getProductsPaginated(int page, int size) {
        Pageable pageable = PageRequest.of(page, size);
        return productRepository.findAll(pageable);
    }

    public Iterable<Product> getProductsSorted(Sort sort) {
        return productRepository.findAll(sort);
    }
}
```


### 3. **`JpaRepository<T, ID>`**

`JpaRepository` расширяет `PagingAndSortingRepository` и добавляет дополнительные методы, специфичные для JPA, такие как:

- **`flush()`** — синхронизирует состояние в памяти с базой данных.
- **`saveAndFlush(S entity)`** — сохраняет и немедленно сбрасывает изменения в базу данных.
- **`deleteInBatch(Iterable<T> entities)`** — удаляет сущности пакетом.
- **`findById(ID id)`** — находит сущность по её идентификатору.

`JpaRepository` предоставляет полный набор возможностей для работы с данными в рамках JPA.

### 4. QueryDslJpaRepository


### 5.  SimpleJpaRepository


### 6. Кастомные репозитории

Помимо стандартных методов, предоставляемых интерфейсами, вы можете добавлять свои кастомные методы в репозитории.

#### Пример кастомного метода:

```java
public interface OrderRepository extends JpaRepository<Order, Long> {

    // Кастомный запрос с использованием JPQL
    @Query("SELECT o FROM Order o WHERE o.status = :status")
    List<Order> findOrdersByStatus(@Param("status") String status);
}
```

