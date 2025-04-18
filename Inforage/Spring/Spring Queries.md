### Ключевые слова для распознавания запросов

- SELECT: `SELECT e FROM Employee e`, `List<Employee> findAll();`
- FROM: `SELECT e FROM Employee e`, `List<Employee> findAll();`
- WHERE: `SELECT e FROM Employee e WHERE e.salary > 1000`, `List<Employee> findBySalaryGreaterThan(int salary);`
- JOIN: `SELECT e FROM Employee e JOIN e.department d`, `List<Employee> findByDepartment();`
- INNER JOIN: `SELECT e FROM Employee e INNER JOIN e.department d`, `List<Employee> findByDepartmentInnerJoin();`
- LEFT JOIN: `SELECT e FROM Employee e LEFT JOIN e.department d`, `List<Employee> findByDepartmentLeftJoin();`
- ORDER BY: `SELECT e FROM Employee e ORDER BY e.salary DESC`, `List<Employee> findByOrderBySalaryDesc();`
- GROUP BY: `SELECT e.department, COUNT(e) FROM Employee e GROUP BY e.department`, `List<Object[]> countEmployeesByDepartment();`
- HAVING: `SELECT e.department, COUNT(e) FROM Employee e GROUP BY e.department HAVING COUNT(e) > 5`, `List<Object[]> countEmployeesByDepartmentHavingMoreThan(int count);`
- DISTINCT: `SELECT DISTINCT e FROM Employee e`, `List<Employee> findDistinctByDepartment(String dept);`
- UPDATE: `UPDATE Employee e SET e.salary = :salary WHERE e.id = :id`, `@Modifying void updateEmployeeSalaryById(int id, int salary);`
- DELETE: `DELETE FROM Employee e WHERE e.id = :id`, `@Modifying void deleteEmployeeById(int id);`
- COUNT: `SELECT COUNT(e) FROM Employee e`, `long count();`
- MAX: `SELECT MAX(e.salary) FROM Employee e`, `Optional<Integer> findMaxSalary();`
- MIN: `SELECT MIN(e.salary) FROM Employee e`, `Optional<Integer> findMinSalary();`
- SUM: `SELECT SUM(e.salary) FROM Employee e`, `int sumSalary();`
- AVG: `SELECT AVG(e.salary) FROM Employee e`, `double findAverageSalary();`
- EXISTS: `SELECT CASE WHEN (COUNT(e) > 0) THEN true ELSE false END FROM Employee e`, `boolean existsByDepartment(String dept);`
- IN: `SELECT e FROM Employee e WHERE e.department.name IN ('HR', 'Finance')`, `List<Employee> findByDepartmentIn(List<String> departments);`
- BETWEEN: `SELECT e FROM Employee e WHERE e.salary BETWEEN 1000 AND 5000`, `List<Employee> findBySalaryBetween(int minSalary, int maxSalary);`
- LIKE: `SELECT e FROM Employee e WHERE e.name LIKE '%John%'`, `List<Employee> findByNameContaining(String name);`
- IS NULL: `SELECT e FROM Employee e WHERE e.department IS NULL`, `List<Employee> findByDepartmentIsNull();`
- IS NOT NULL: `SELECT e FROM Employee e WHERE e.department IS NOT NULL`, `List<Employee> findByDepartmentIsNotNull();`
- FETCH: `SELECT e FROM Employee e JOIN FETCH e.department`, `List<Employee> findAllWithDepartmentFetch();`
- AND: `SELECT e FROM Employee e WHERE e.salary > 1000 AND e.department.name = 'HR'`, `List<Employee> findBySalaryGreaterThanAndDepartmentName(int salary, String dept);`
- OR: `SELECT e FROM Employee e WHERE e.salary > 5000 OR e.department.name = 'Finance'`, `List<Employee> findBySalaryGreaterThanOrDepartmentName(int salary, String dept);`
### Автоматическая генерация SQL-запросов через методы репозитория

Spring Data JPA позволяет вам определять методы в интерфейсе репозитория, и на основе их имен будет автоматически генерироваться SQL-запрос. Это делается без необходимости явного написания запросов.

#### Примеры

```java
public interface UserRepository extends JpaRepository<User, Long> {
    // Найти пользователя по имени
    User findByName(String name);
    
    // Найти пользователей по возрасту
    List<User> findByAge(int age);
    
    // Найти пользователей по имени и возрасту
    List<User> findByNameAndAge(String name, int age);
}
```

В этом примере методы `findByName`, `findByAge`, и `findByNameAndAge` автоматически генерируют соответствующие SQL-запросы, такие как `SELECT * FROM user WHERE name = ?` и `SELECT * FROM user WHERE age = ?`.
    
- **Сложные запросы с `Like`, `Between` и другими операторами:**

```java
public interface ProductRepository extends JpaRepository<Product, Long> {
    // Найти продукты, чье имя начинается с заданной строки
    List<Product> findByNameStartingWith(String prefix);
    
    // Найти продукты, цена которых находится в заданном диапазоне
    List<Product> findByPriceBetween(Double minPrice, Double maxPrice);
    
    // Найти продукты, чье имя содержит заданную строку
    List<Product> findByNameContaining(String substring);
}
```

Эти методы генерируют запросы типа `SELECT * FROM product WHERE name LIKE ?`, `SELECT * FROM product WHERE price BETWEEN ? AND ?`, и `SELECT * FROM product WHERE name LIKE ?`.

### Создание кастомных запросов с `@Query`

Если вам нужно больше контроля над запросами, вы можете использовать аннотацию `@Query` для написания собственных SQL-запросов или JPQL (Java Persistence Query Language).

#### Примеры

- JPQL-запросы:

```java
public interface UserRepository extends JpaRepository<User, Long> {
    
    @Query("SELECT u FROM User u WHERE u.name = :name")
    User findUserByName(@Param("name") String name);
    
    @Query("SELECT u FROM User u WHERE u.age > :age")
    List<User> findUsersOlderThan(@Param("age") int age);
}
```

Здесь мы используем JPQL для выполнения запросов. `@Query("SELECT u FROM User u WHERE u.name = :name")` генерирует запрос типа `SELECT * FROM user WHERE name = ?`.
    
- SQL-запросы:

```java
public interface ProductRepository extends JpaRepository<Product, Long> {
    
    @Query(value = "SELECT * FROM product WHERE name = :name", nativeQuery = true)
    Product findProductByName(@Param("name") String name);
    
    @Query(value = "SELECT * FROM product WHERE price BETWEEN :minPrice AND :maxPrice", nativeQuery = true)
    List<Product> findProductsInPriceRange(@Param("minPrice") Double minPrice, @Param("maxPrice") Double maxPrice);
}
```

В этом примере используются нативные SQL-запросы. `@Query(value = "SELECT * FROM product WHERE name = :name", nativeQuery = true)` указывает, что запрос будет выполнен как нативный SQL-запрос.

**Если запросы модифицирующие, для этого к ним добавляется еще аннотация Modifying.**











