### Пессимистичные блокировки

Класс _LockMode_ определяет различные уровни блокировок, которые может захватывать Hibernate.  

- LockMode.WRITE  
    Захватывается автоматически, когда Hibernate обновляет или вставляет строку.
- LockMode.UPGRADE  
    Захватывается после явного запроса пользователя с использованием SELECT… FOR UPDATE на БД, поддерживающих данный синтаксис.
- LockMode.UPGRADE_NOWAIT  
    Захватывается после явного запроса пользователя с использованием SELECT… FOR UPDATE NOWAIT в Oracle
- LockMode.READ  
    Захватывается автоматически когда Hibernate читает данные под уровнями изоляции Repeatable Read или Serializable. Может быть повторно захвачен явным запросом пользователя.
- LockMode.NONE  
    Отсутствие блокировки. Все объекты переключаются на этот режим блокировки в конце транзакции. Объекты, ассоциированные с сессией  
    через вызов update() или saveOrUpdate также начинают в этом режиме .


### LockModeType или как выставить блокировку

Блокировку можно выставить через вызов метода lock у EntityManager.  
  

```
entityManager.lock(myObject, LockModeType.OPTIMISTIC);
```

  
LockModeType задает стратегию блокирования.  
LockModeType бывает 6 видов(2 из которых относятся к _оптимистичным_, а 3 к _пессимистичным_):  
1. NONE — отсутствие блокировки
2. OPTIMISTIC
3. OPTIMISTIC_FORCE_INCREMENT
4. PESSIMISTIC_READ
5. PESSIMISTIC_WRITE
6. PESSIMISTIC_FORCE_INCREMENT

