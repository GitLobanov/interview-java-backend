# Проекции

HQL предоставляет удобное решение для создания DTO-классов непосредственно при выполнении запросов. Этот механизм в Hibernate называют проекциями. Например, преобразование списка `Lesson` в список `LessonDto` можно сделать вот так:

```java
@Repository
public interface LessonRepository extends JpaRepository<Lesson, Long> {
    
    @Query("select new com.example.dto.LessonDto(l.id, l.title, l.text, l.course.id) " +
            "from Lesson l")
    List<LessonDto> findAllWithProjection();
}
```

Обратите внимание, что здесь необходимо указывать полное имя класса со всеми пакетами. Попробуйте заменить преобразование через Stream API на использование проекции.

Проекции удобно использовать не только для создания DTO, но и в любой другой ситуации, когда структура результатов запроса не совпадает ни с одной из сущностей, описанных в приложении.


# Resources
- https://stepik.org/lesson/747536/step/1?unit=749344