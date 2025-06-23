#### Что такое JdbcTemplate?


#### Propogation in @Transactional

- **Required (по умолчанию)**: Моему методу нужна транзакция, либо откройте ее для меня, либо используйте существующую → `getConnection().setAutocommit(false).commit()`
- **Supports**: Мне не важно, открыта транзакция или нет, я могу работать в любом случае → не имеет отношения к **JDBC**
- **Mandatory**: Я не собираюсь открывать транзакцию сам, но я буду плакать, если никто другой не откроет её → не имеет отношения к **JDBC**
- **Require_new:** Я хочу полностью собственную транзакцию → `getConnection().setAutocommit(false).commit()`
- **Not_Supported:** Мне очень не нравятся транзакции, я даже попытаюсь приостановить текущую, запущенную транзакцию → ничего общего с **JDBC**
- **Never:** Я буду плакать, если кто-то другой запустит транзакцию → не имеет отношения к **JDBC**
- **Nested:** Это звучит так сложно, но мы просто говорим о точках сохранения! → `connection.setSavepoint()`

###### Что такое Spring Data Specification

![[../../../_res/Pasted image 20240926135057.png]]

###### Что в ходит в Transactional?

1. propagation:

- (default) REQUIRED - требует, чтобы все выполнялось в транзакции, если нету, она будет создана
- SUPPORTS - если нету транзакции, просто выполнит вне транзакции
- MANDATORY - обязательно транзакция, если нету выбросит исключение
- REQUIRES_NEW - останавливает текущую транзакцию если она есть, и исполняет следующие инструкции в отдельной транзакции
- NOT_SUPPORTED - останавливает текущую транзакцию если она есть, выполняет без транзакции
- NEVER - нигде не должно быть транзакции
- NESTED - используется savepoints, при вызове исключений, возвращается к savepoint

2. isolation
3. readOnly
4. rollbackFor
5. notRollbackFor
6. timeout

[[../../../_inforage/Spring/Annotations/@Transactional]]
[[../../../_inforage/Spring/Annotations/@Transactional. Основы работы с транзакциями]]

###### Можно ли ставить @Transactional над приватными методами?

- **Нет**, Spring создает прокси только для публичных методов.
- **Приватные методы** не могут быть переопределены в прокси (они не видны в подклассах).

###### А что если вызвать транзакцию внутри класса, в другом методе.

- Транзакция не выполнится
- Бин транзакции оборачивается в proxy, поэтому на этапе вызова в том же классе, еще не было добавлено логики транзакции

###### Что такое Programmatic Transaction Template / Transaction Manager

- С помощью метода execute можно выполнить определенную логику транзакции

```java
        transactionTemplate.execute(status -> {
            try {
                doSomeDatabaseOperations();
                return null;
            } catch (Exception e) {
                status.setRollbackOnly();
                throw new RuntimeException("Error during transaction", e);
            }
        });
```

[[../../../_inforage/Spring/TransactionTemplate]]

### Resources

- [Spring @Transactional — ошибки, которые совершали все](https://habr.com/ru/companies/otus/articles/574470/)
- [Используем аннотацию @Transactional like a pro](https://habr.com/ru/companies/rosbank/articles/707378/)
- 