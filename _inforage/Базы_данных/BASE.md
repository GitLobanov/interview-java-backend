## In short

> **BASE** — это своеобразный контраст ACID, который говорит нам, что истинная согласованность не может быть достигнута в реальном мире и не может быть смоделирована в высокомасштабируемых системах.

- Basic Availability. Система отвечает на любой запрос, но этот ответ может быть содержать ошибку или несогласованные данные.
- Soft-state. Состояние системы может меняться со временем из-за изменений конечной согласованности.
- Eventual consistency (конечная согласованность). Система, в конечном итоге, станет согласованной. Она будет продолжать принимать данные и не будет проверять каждую транзакцию на согласованность.

**Basically Available (Базовая доступность)**
- Система гарантирует доступность данных даже в случае частичных отказов.
- Данные могут быть не полностью согласованными, но запросы не блокируются.
- Например, если один сервер недоступен, другой всё равно может выдать старую версию данных.

**Soft state (Гибкое состояние)**
- Состояние системы может изменяться со временем даже без новых запросов.
- Это значит, что данные в разных узлах могут временно отличаться из-за асинхронной репликации.
- Например, если запись обновилась на одном сервере, изменения могут дойти до других с задержкой.

**Eventually consistent (Окончательная согласованность)**
- Система со временем достигает согласованности, но не сразу.
- Это позволяет масштабировать базы данных и повышать отказоустойчивость.
- Например, в DynamoDB или Cassandra данные могут быть сначала разными на разных узлах, но через некоторое время синхронизируются.

### Когда выбирают BASE вместо ACID?

**BASE подходит, когда:**

- Важнее скорость и отказоустойчивость, чем мгновенная согласованность.
- Данные могут быть временно несогласованными, но постепенно синхронизируются.
- Используются распределённые NoSQL базы (Cassandra, DynamoDB, Riak, CouchDB).

В отличие от ACID, BASE жертвует строгой целостностью ради масштабируемости и высокой доступности.

### **Basically Available (Базовая доступность)**

_**Базовая доступность** означает, что база данных должна быть доступна одновременно для всех пользователей в любое время. Пользователю не нужно ждать, пока завершатся другие транзакции, прежде чем обновлять запись. Например, в случае внезапного роста трафика система электронной коммерции может отдавать предпочтение выдаче списков и приему заказов. Не страшно, если обновление количества запасов будет выполнено с небольшой задержкой, зато пользователи продолжают покупать товары._

---

Даже если часть системы выходит из строя, база продолжает работать.

- **Репликация данных** – копии хранятся на нескольких узлах.
- **Eventual Consistency** – данные синхронизируются позже, но система не блокируется.
- **Шардирование (разделение данных)** – нагрузка делится на множество узлов.

Пример:

- Cassandra или DynamoDB используют quorum-репликацию, где большинство узлов должны подтвердить запись.
- Redis Cluster распределяет данные по узлам, гарантируя высокую доступность.

### **Soft State (Гибкое состояние)**

_**Гибкое/Мягкое состояние** здесь означает, что любые данные могут находиться в промежуточных или временных состояниях и в некоторый момент изменяться «сами по себе», без внешних событий или поступления новых данных. Эта концепция отражает неопределенное состояние записи, которую обновляют несколько приложений одновременно. Значение такой записи будет окончательно определено только после завершения всех транзакций. Например, если пользователи редактируют сообщение в социальной сети, внесенные ими изменения могут быть не сразу видны другим пользователям. Через некоторое время система учтет все внесенные изменения, и произойдет обновление сообщения, хотя ни один пользователь его не инициировал._

---

Система может изменяться даже без новых транзакций.

- **Асинхронная репликация** – данные распространяются между узлами не сразу, а с задержкой.
- **Кэширование** – данные могут храниться в распределённых узлах и устаревать.
- **Механизмы разруливания конфликтов** – например, last-write-wins (LWW), CRDT (Conflict-Free Replicated Data Types).

Пример:

- В Riak можно настроить стратегию разрешения конфликтов – брать последнее изменение или объединять данные.
- В MongoDB при репликации вторичные узлы могут временно содержать устаревшие данные.

### **Eventually Consistent (Окончательная согласованность)**

_**Окончательная согласованность** означает, что запись достигнет согласованности не сразу, а после завершения всех одновременных обновлений. После этого все приложения, запрашивающие эту запись, увидят одно и то же значение. Давайте рассмотрим в качестве примера распределенную систему редактирования документов, в которой несколько пользователей могут одновременно редактировать документ. Если пользователь A и пользователь B одновременно редактируют один и тот же раздел документа, их локальные копии могут временно различаться, пока не завершатся процессы распространения и синхронизации. Однако со временем, «в конечном счете», система достигает согласованности, распространяя и объединяя все изменения от разных пользователей._

---

Данные на всех узлах рано или поздно становятся одинаковыми.

- Гибкие уровни согласованности – например, «читаем свежие данные, если возможно».
- Механизмы Gossip Protocol – узлы обмениваются информацией о последних изменениях.
- Read Repair – если запрос вернул устаревшие данные, база может их обновить в фоновом режиме.

Пример:

- Cassandra использует hinted handoff (хранит «подсказки» для недоступных узлов и передаёт их позже).
- DynamoDB применяет vector clocks (временные метки для согласования версий данных).

### Краткое описание различий между ACID и BASE

Вместо строгой ACID-согласованности базы BASE обеспечивают «размытую» согласованность, используя асинхронную репликацию, механизмы конфликтного разрешения и компромиссные модели консистентности.

Ниже будут рассмотрены некоторые из приемов, которые обеспечивают тот или иной пункт модели BASE.

![](../../../_res/Pasted%20image%2020250213162640.jpg)


## В чем разница между базами данных ACID и BASE?

> Аббревиатуры BASE и ACID специально подбирались так, чтобы в английском языке они противопоставлялись друг другу, поскольку эти же слова обозначают химически противоположные термины «щелочь» и «кислота».

---

В базах данных ACID приоритет отдается согласованности в ущерб доступности. Если на любом этапе транзакции возникает ошибка, вся транзакция полностью отменяется. Напротив, базы данных BASE отдают приоритет доступности в ущерб согласованности. При неудачном завершении транзакции пользователи могут временно получать несогласованные данные. Согласованность данных восстанавливается с некоторой задержкой.

## Для чего важны ACID и BASE?

ACID
Например, когда покупатель добавляет товар в корзину на веб-сайте электронной коммерции, все остальные покупатели должны увидеть снижение уровня запасов этого товара. Если очередной покупатель добавит в корзину последний экземпляр, все остальные пользователи должны увидеть, что товара нет в наличии. Разработчикам баз данных нужно сделать выбор о поведении базы данных в том случае, если какая-либо операция в транзакции не может быть выполнена. База данных может действовать по одному из следующих принципов.

Отменить транзакцию и возвратить сообщение об ошибке, что снизит доступность, но гарантирует согласованность. Покупатель в таком случае не сможет добавить товар в корзину или другие покупатели не получат данных по некоторым товарам, пока не завершатся все операции добавления в корзину.

Продолжать остальные операции, что обеспечит максимально возможную доступность, но может приводить к несогласованности. В этом случае покупатель успешно добавит товар в корзину, а другие покупатели получат сведения об уровне запасов, но эти сведения будут неверными по крайней мере в течение некоторого времени.

BASE
Например, когда вы принимаете запрос на добавление в друзья в социальной сети, никого не беспокоит, что другие пользователи некоторое время видят неточное количество друзей в вашем профиле. Вам более важно, чтобы доступ к ленте сообщений не пропадал на период синхронизации данных. В таких сценариях предпочтительным будет модель BASE.

## Может ли база данных быть одновременно ACID и BASE?

Согласно теореме CAP, база данных может удовлетворять только двум из трех требований: согласованность, доступность и устойчивость к разделению. Обе модели баз данных ACID и BASE обеспечивают устойчивость к разделению, то есть не могут быть одновременно высокосогласованными и постоянно доступными. Это означает, что любая база данных может больше или меньше соответствовать концепции ACID или BASE, но не может использовать оба подхода одновременно. Например, базы данных SQL структурированы по модели ACID, а базы данных NoSQL используют архитектуру BASE. Некоторые базы данных NoSQL могут обладать определенными свойствами, характерными для ACID, но никогда не могут полностью соответствовать концепции ACID.

## Когда что использовать: ACID в сравнении с BASE

Несмотря на существенные различия, системы баз данных ACID и BASE применяются в разных приложениях. ACID идеально подходит для корпоративных приложений, в которых требуются согласованность, надежность и предсказуемость данных. Например, банки всегда используют для хранения транзакций клиентов базу данных с архитектурой ACID, поскольку целостность данных здесь является главным приоритетом. В свою очередь, базы данных BASE лучше подходят для аналитической обработки слабо структурированных данных в больших объемах. Например, веб-сайты электронной коммерции используют базы данных с архитектурой BASE для хранения цен на товары, которые часто меняются. В этом случае точность отображения цен менее важна, чем своевременное информирование клиентов об изменении цены.

## Ключевые различия между ACID и BASE

**При выборе между моделями транзакций ACID и BASE для баз данных нужно искать компромиссы.**

**Масштабирование**

База данных с моделью транзакций ACID масштабируется хуже, поскольку она ориентирована на согласованность. Для любой записи в любой момент времени может выполняться только одна транзакция, что затрудняет горизонтальное масштабирование.

С другой стороны, базу данных с архитектурой BASE легко масштабировать по горизонтали, поскольку нет необходимости поддерживать строгую согласованность. Простое добавление нескольких узлов в кластер позволяет базе данных с архитектурой BASE повысить доступность данных, которая является основополагающим принципом этой архитектуры базы данных.

**Гибкость**

Базы данных ACID менее гибки в обработке данных. Такая база данных должна гарантировать немедленную согласованность, поэтому она может ограничивать доступ к некоторым приложениям в случае сбоев в сети или в электроснабжении. Аналогичным образом, приложениям приходится ждать своей очереди на обновление данных, если другие программные модули уже обрабатывают некоторую запись. Базы данных BASE намного более гибкие. В архитектуре BASE не используются строгие ограничения, и приложения могут изменять записи по мере появления обновлений.

**Производительность**

База данных ACID может испытывать проблемы с производительностью при обработке больших объемов данных или параллельных запросов. Поскольку все операции обрабатываются в строгом порядке, накладные расходы на поддержание целостности транзакций приводят к дополнительным задержкам, которые влияют на доступ всех приложений к затронутой записи.

Напротив, к базе данных BASE приложения могут обращаться для обработки записей в любое время. Это позволяет избежать длительного времени ожидания и повышает пропускную способность базы данных.

**Синхронизация**

База данных с архитектурой ACID нуждается в механизме синхронизации, чтобы выполненные в транзакции изменения были зафиксированы одновременно во всех связанных записях. В то же время, каждая изменяемая запись должны быть заблокирована от доступа других сторон до завершения или отмены транзакции. База данных BASE, в свою очередь, работает без блокировки записей и поддерживает только согласованность «в конечном счете», без каких-либо гарантий по времени ее достижения. При работе с базами данных BASE разработчики понимают, что при обработке записей могут и будут возникать несоответствия, и принимают необходимые меры предосторожности в самом приложении.