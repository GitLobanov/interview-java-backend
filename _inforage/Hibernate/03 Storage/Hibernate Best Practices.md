## Уникальный идентификатор сущности

Всегда задавайте уникальный идентификатор (далее id) для всех сущностей, и желательно что бы это было какое нибудь число, например, у вас есть сущность account у которого есть уникальное поле email - может показаться что данное поле может выступать в качестве id, и поначалу это может оказаться вполне рабочим решением. Но если вкратце, оно не идеально и в будущем может наложить некоторые ограничения на приложение, к тому же по производительности могут возникнуть вопросы.

В случае если СУБД поддерживает тип **SEQUENCE**(автоинкремент) то всегда предпочтительно выбирать именно его, в некоторых случаях это позволяет сильно повысить производительность системы.

К примеру, если есть 5 операций вставки новых общностей, Spring Data или сам Hibernate могут быть достаточно умны, что бы поместить их в так называемый Batch и отдать их в СУБД за один раз, но для этого id новых записей надо знать заранее, вот тут то и вступает в игру тот самый автоинкремент, запросив текущее значение счетчика, не трудно догадаться какие id будут у всех пяти записей.

## Equals и HashCode методы

Всегда ли нужно определять эти два метода, ответ нет, в среднем случае приложению достаточно дефолтных реализаций. Но так же есть и те случаи кода требуется кастомная реализация, т. к. это может существенно увеличить производительность приложения, либо ухудшить, в случае если вы допустили ошибку.  

Буде внимательны при выборе полей кандидатов в Equals и HashCode, это критично если ваши сущности будут храниться в коллекции типа Set или Map, либо вам приходится работать с detached объектами. В основном рекомендуют использовать [уникальный идентификатор и/или так называемые «натуральные ключи»](https://docs.jboss.org/hibernate/core/4.0/manual/en-US/html/persistent-classes.html#persistent-classes-equalshashcode), т. е. Поле однозначно идентифицирующее объект и которое является уникальным.

[Избегайте использования @Data и @ToString аннотаций Lombok](https://www.jpa-buddy.com/blog/lombok-and-jpa-what-may-go-wrong/). Обе эти аннотации использую все поля объектов по умолчанию, что может послужить причиной проблем. При большом желании, можно использовать @ToString, главное не забывайте исключить LAZY поля вышей сущности из обработки при помощи @ToString.Exclude.

```java
@OneToMany(mappedBy = "account", cascade = CascadeType.ALL)
@ToString.Exclude
private Set<RelatedPlatformEntity> relatedPlatforms = new HashSet<>();
```

## Обработка исключений

В случае если вы вдруг используете голый Hibernate, то **никогда не обрабатывайте исключение брошенное Jdbc как recoverable, т. е. не пытайтесь продолжать транзакцию, просто откатите её и закройте EntityManager.** Иначе Hibernate не гарантирует актуальность загруженных данных!

Если же вы используете Spring Data, то можете быть спокойны, он позаботился об этом за нас.

## Двухсторонние отношение сущностей

Как вы знаете в Hibernate существуют два типа отношений между сущностями, одно и двустороннее. Не вникая глубоко в детали, скажу что желательно всегда использовать двустороннее, если вы конечно на 100 % уверены, что обратная связи вам никогда не понадобится, то и одностороннее сойдет. Разница между ними не сильно большая (если все сделать грамотно), а пользы гораздо больше от двусторонней.

И так, как же это сделать грамотно? Тут все просто, дабы Hibernate создал именно двустороннее отношение (а не два односторонних) нужно уточнить, какая из сторон является владельцем отношений, а какая является обратной стороной. В этом нам поможет атрибут mappedBy. Он указывается в аннотации, которая находится на противоположной стороне отношения и указывает на владельца.

```java
@ManyToOne
@JoinColumn(name = "platform_id")
private PlatformEntity platform;
...

@OneToMany(mappendBy = "platform", cascade = {CascadeType.PERSIST})
private Set<RelatedPlatformEntity> statuses = new HashSet<>();
```

Так же в рамках данного топика хотел бы заострить  внимание на том как описываю связь в самой сущности, а конкретно:

```java
@OneToMany(mappedBy = "platform", cascade = {CascadeType.PERSIST})
private Set<RelatedPlatformEntity> statuses = new HashSet<>();
```

Хорошей привычкой является инициализация коллекции, иногда может спасти  от неприятного NullPointerException, либо от постоянной проверки на null коллекции. Так же, вы можете заметить что вместо List, я использую Set, данный трюк может помочь где-то выиграть в ресурсах (при условии что Equals и HashCode переопределены), хотя и List вполне справляется.

## Ленивая загрузка

В продолжение темы отношений между сущностями, в зависимости от типа отношений, Hibernate применяет LAZY или EAGER тип загрузки связанных сущностей. Если у вас нет особой причины для обратного, то убедитесь что выставлен тип LAZY.

## Параметризованные запросы

Если вдруг вам случается писать JPQL или Native запросы, не забывайте передавать критерии поиска через параметры, никогда не пишите их на прямую в запросе, т.к. это создаёт уязвимость для атаки SQL injection

![[../../../_res/Pasted image 20240921090344.png]]

## Кэш второго уровня

Это очень полезная фитча Hibernate. На этапе изучения фреймворка, этому не уделяется много внимания, да и в принципе маленький проект может вполне существовать без кэша второго уровня. Но если вы метите в Enterprise разработку, ну или просто пилите проект с более или менее серьезной нагрузкой то обязательно ознакомиться. 

Безопасность метода заключается в том, что работать мы будем только со статическими сущностями, т. е. кандидаты в кэш второго уровня, это те сущности которые не будут меняться в вашем приложении, они либо создаются, либо удаляются. При таком раскладе у нас меньше шансов ошибиться.

Hibernate не содержит реализации данной фитчи, по этому придется использовать сторонние реализации, коих множество, к примеру Ehcache 3. Так же что бы подружить Hibernate 5 с Ehcache 3 необходимо подключить Hibernate-jcache зависимость.

![[../../../_res/Pasted image 20240921090748.png]]

После подключения зависимостей необходимо указать Spring что с ними надо работать, а в частности задать свойства jpa persitence sharedCache и hibernate cache.

![[../../../_res/Pasted image 20240921090847.png]]


Далее выбранную сущность кандидат в кэш необходимо пометить тремя аннотациями @Cacheable @Cache и @Immutable

![[../../../_res/Pasted image 20240921090938.png]]

Вот и все, вы реализовали потокобезопасный кэш второго уровня! Остаются еще такие тонкости как настройка времени жизни объектов в кэше, размер кэша и т. д. Тем не менее на дефолтных настройках это тоже работает.

## Аннотация @Column и @Table

При использовании данных аннотаций так же является хорошей практикой всегда задавать имя поля/таблицы. Если вы этого не сделаете, то Hibernate заделает это за вас и имя может не всегда соответствовать ожиданиям.

![[../../../_res/Pasted image 20240921091016.png]]

## Количество загруженных сущностей

Дело в том что управляемые сущности не только занимают память, но еще потребляют процессорное время. Hibernate периодически проверяет состояние загруженных объектов на соответствие в базе данных, за это отвечает так называемый dirty checking. В рамках много пользовательского приложения это может быть значимой нагрузкой. Здесь очень полезна ленивая загрузка.

## Проблема N+1 Select

При получении сущности или списка общностей, Hibernate сделает один select и все бы ничего, но если данные сущности имеют EAGER отношения (ManyToOne и OneToOne по умолчанию EAGER) то для каждого из них будет сделан дополнительный select!

Выходом из этой ситуации может быть изменение дефолтного EAGER на LAZY.

```java
@ManyToOne(fetch = FetchType.LAZY)@JoinColumn(name = "status")
private StatusEntity status;
```
Все же это решение подходит не всегда, допустим мы получаем список сущностей AccountEntiy, который содержит в себе связь с StatusEntity, инеобходим проверить статус каждой сущности (разумеется вы делаете это в рамках одной транзакции, иначе вы получите LazyInitializationException), это приведет снова к той же проблеме, при обращении к статусу каждой сущности Hibernate сделает отдельный select, и вот проблема снова вернулась.

В этом случае проблему поможет решить инструкция JOIN FETCH, которая укажет Hibernate что связанные сущности тоже надо загрузить, вот пример JPQL запроса:

```sql
select account from AccountEntity account left join fetch account.status
```

В этом решении есть одна проблема, если вдруг список сущностей очень сильно вырастит. Представьте что количество записей AccountEntity равно пятидесяти тысячам, загрузить такое количество общностей будет не легко, а тут в нагрузку еще пятидесяти тысяч StatusEntity, это ужу совсем непростая задача. Дабы избежать этих трудностей нужно вернуться к схеме с ленивой загрузкой, в итоге система за раз загрузит только AccountEntity, что уменьшит нагрузку, но в то же время вернет проблему N+1 запросов. 

Стратегия загрузки Batch Fetching позволяет получать связанные сущности не одиночными запросами, а пакетно. Размер пакета задается аннотацией @BatchSize.

```java
@ManyToOne@JoinColumn(name = "status")
@BatchSize(size = 10)
private StatusEntity status;.
```
Использование данной аннотации с параметром «size=10» приведет к тому, что Hibernate за раз будет загружать ту сущность, что вы хотите прочитать и девять следующих по порядку.
## Источники

- [Hibernate Best Practices для начинающих](https://habr.com/ru/articles/679216/)
- 




