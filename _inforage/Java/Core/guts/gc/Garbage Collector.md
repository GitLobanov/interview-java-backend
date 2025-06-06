# Введение

Что такое "мусор"? Мусором считается объект, который больше не может быть достигнут по ссылке из какого-либо объекта. Поскольку такие объекты больше не используются в приложении, то их можно удалить из памяти.

Например, на диаграмме ниже объект fruit2 может быть удален из памяти, поскольку на него нет ссылок.

![[../../../../../_res/Pasted image 20240922142747.png]]

Что такое сборка мусора? Сборка мусора — это процесс автоматического управления памятью. Освобождение памяти (путем очистки мусора) выполняется автоматически специальным компонентом JVM — сборщиком мусора (Garbage Collector, GC).

# Сборка мусора: процесс

Для сборки мусора используется алгоритм пометок (Mark & Sweep). Этот алгоритм состоит из трех этапов:

1. **Mark (маркировка)**. На первом этапе GC сканирует все объекты и помечает живые (объекты, которые все еще используются). На этом шаге выполнение программы приостанавливается. Поэтому этот шаг также называется "Stop the World" .
2. **Sweep (очистка)**. На этом шаге освобождается память, занятая объектами, не отмеченными на предыдущем шаге.
3. **Compact (уплотнение)**. Объекты, пережившие очистку, перемещаются в единый  непрерывный блок памяти. Это уменьшает фрагментацию кучи и позволяет проще и быстрее размещать новые объекты.

![[../../../../../_res/Pasted image 20240922142946.png]]
## Поколения объектов

#### Что такое поколения объектов?

Для оптимизации сборки мусора память кучи дополнительно разделена на четыре области. В эти области объекты помещаются в зависимости от их возраста (как долго они используются в приложении).

1. **Young Generation (молодое поколение)**. Здесь создаются новые объекты. Область young generation разделена на три части раздела: Eden (Эдем), S0 и S1 (Survivor Space — область для выживших).
2. **Old Generation (старое поколение)**. Здесь хранятся давно живущие объекты.

![[../../../../../_res/Pasted image 20240922143030.png]]
## Что такое Stop the World?

Когда запускается этап mark, работа приложения останавливается. После завершения mark приложение возобновляет свою работу. Любая сборка мусора — это "Stop the World".

## Что такое гипотеза о поколениях?

Как уже упоминалось ранее, для оптимизации этапов mark и sweep используются поколения. Гипотеза о поколениях говорит о следующем:

1. Большинство объектов живут недолго.
2. Если объект выживает, то он, скорее всего, будет жить вечно.
3. Этапы mark и sweep занимают меньше времени при большом количестве мусора. То есть маркировка будет происходить быстрее, если анализируемая область небольшая и в ней много мертвых объектов.
    
Таким образом, алгоритм сборки мусора, использующий поколения, выглядит следующим образом:

![[../../../../../_res/Pasted image 20241026092544.png]]

1. Новые объекты создаются в области Eden. Области Survivor (S0, S1) на данный момент пустые.
    
2. Когда область Eden заполняется, происходит **минорная сборка мусора (Minor GC)**. Minor GC — это процесс, при котором операции mark и sweep выполняются для young generation (молодого поколения).
    
3. После Minor GC живые объекты перемещаются в одну из областей Survivor (например, S0). Мертвые объекты полностью удаляются.
    
4. По мере работы приложения пространство Eden заполняется новыми объектами. При очередном Minor GC области young generation и S0 очищаются. На этот раз выжившие объекты перемещаются в область S1, и их возраст увеличивается (отметка о том, что они пережили сборку мусора).
    
5. При следующем Minor GC процесс повторяется. Однако на этот раз области Survivor меняются местами. Живые объекты перемещаются в S0 и у них увеличивается возраст. Области Eden и S1 очищаются.
    
6. Объекты между областями Survivor копируются определенное количество раз (пока не переживут определенное количество Minor GC) или пока там достаточно места. Затем эти объекты копируются в область Old.
    
7. Major GC. При **Major GC** этапы mark и sweep выполняются для Old Generation. Major GC работает медленнее по сравнению с Minor GC, поскольку старое поколение в основном состоит из живых объектов.
    

## Преимущества использования поколений

Minor GC происходит в меньшей части кучи (~ 2/3 от кучи). Этап маркировки эффективен, потому что область небольшая и состоит в основном из мертвых объектов.

## Недостатки использования поколений

В каждый момент времени одно из пространств Survivor (S0 или S1) пустое и не используется.

## Сборка мусора: флаги

В этом разделе приведены некоторые важные флаги, которые можно использовать для настройки процесса сборки мусора.

|   |   |
|---|---|
|**Флаг**|**Описание**|
|-Xms|Первоначальный размер кучи|
|-Xmx|Максимальный размер куча|
|-XX:NewRatio=n|Отношение размера Old Generation к Young Generation|
|-XX:SurvivorRatio=n|Отношение размера Eden к Survivor|
|-XX:MaxTenuringThreshold=n|Возраст объекта, когда объект перемещается из области Survivor в область Old Generation|

_Флаги JVM_

# Типы сборщиков мусора

## Кратко по версиям спецификации

1. Java 1.3 — Serial Garbage Collector
2. Java 1.4 — Parallel Garbage Collector
3. Java 1.4.2 — CMS (Concurrent Mark-Sweep) Garbage Collector — _Deprecated_ (deleted in 13)
4. Java 7 — [[G1. Garbage First|G1 (Garbage-First) Garbage Collector]]
5. Java 8 — Epsilon Garbage Collector
6. Java 8 — Parallel Old Garbage Collector
7. Java 11 — ZGC (Z Garbage Collector)
8. Java 12 — [[Shenandoah Garbage Collector]]

![[../../../../../_res/Pasted image 20241019074613.png]]

## Расширенное описание

| **Сборщик мусора** | **Описание**                                                         | **Преимущества**                                                                                                                 | **Когда использовать**                                                                                                                                                                                                                      | **Флаги для включения** |
| ------------------ | -------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| Serial             | Использует один поток.                                               | Эффективный, т.к. нет накладных расходов на взаимодействие потоков.                                                              | Однопроцессорные машины.<br><br>Работа с небольшими наборами данных.                                                                                                                                                                        | -XX:+UseSerialGC        |
| Parallel           | Использует несколько потоков.                                        | Многопоточность ускоряет сборку мусора.                                                                                          | В приоритете пиковая производительность.<br><br>Допустимы паузы при GC в одну секунду и более.<br><br>Работа со средними и большими наборами данных.<br><br>Для приложений, работающих на многопроцессорном или многопоточном оборудовании. | -XX:+UseParallelGC      |
| G1                 | Выполняет некоторую тяжелую работу параллельно с работой приложения. | Может использоваться как на небольших системах, так и на больших с большим количеством процессоров и большим количеством памяти. | Когда время отклика важнее пропускной способности.<br><br>Паузы GC должны быть меньше одной секунды.                                                                                                                                        | -XX:+UseG1GC            |
| Z1                 | Выполняет всю тяжелую работу параллельно с работой приложения.       | Низкая задержка.                                                                                                                 | В приоритете время отклика.                                                                                                                                                                                                                 | +UseZGC                 |
# Инструменты мониторинга GC

#### Что мониторить?

1. Частота запуска сборки мусора. Так как GC вызывает "stop the world", поэтому чем время сборки мусора меньше, тем лучше.
    
2. Длительность одного цикла сборки мусора.
    
#### Как мониторить сборщик мусора?

Для мониторинга можно использовать следующие инструменты:

1. [Visual VM](https://visualvm.github.io/)
    
2. Для включения логирования событий сборщика мусора добавьте следующие параметры JVM:
    

```bash
-XX:+PrintGCDateStamps -verbose:gc -XX:+PrintGCDetails -
Xloggc:/tmp/[Application-Name]-[Application-port]-%t-gc.log -
XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=20 -
XX:GCLogFileSize=100M
```

Логи GC выглядят следующим образом:

```c
[15,651s][info ][gc] GC(36) Pause Young (G1 Evacuation Pause) 239M->57M(307M) (15,646s, 15,651s) 5,048ms
```

Объяснение:

```c
[Время старта приложения = 15,651s]

[Уровень сообщения = info]

[Тег = gc]

[Тип GC = Pause Young] 

[Причина запуска GC = G1 Evacuation Pause] 

[Информация о потреблении памяти : "занято до GC" = 239M -> "занято после GC" = 57M (Размер кучи = 307M)] 

[Время начала и окончания GC = 15,646s, 15,651s] 

[Общее время GC = 5,048ms]
```

# Выбор сборщика мусора

Выбор сборщика мусора в Java имеет значение, когда речь идет об оптимизации производительности приложения, особенно при работе с памятью и нагрузкой. Разные сборщики мусора подходят для различных типов приложений в зависимости от их требований к задержкам, пропускной способности и размерам кучи.

### Когда и почему следует выбирать сборщик мусора:

1. Тип приложения:
    
    - Приложения с низкой задержкой (low-latency): Например, высокочастотные торговые системы, серверы с реальным временем. Здесь важно минимизировать паузы на сбор мусора.
    - Приложения с высокой пропускной способностью (throughput): Серверные приложения, работающие с большим количеством данных, где важна быстрая обработка большого объема задач, но допустимы небольшие паузы.
2. Характеристики приложения:
    
    - Размер кучи (heap size): Чем больше куча, тем более эффективен должен быть сборщик мусора.
    - Частота создания и уничтожения объектов: Если приложение активно создает и уничтожает объекты, выбор сборщика мусора может влиять на производительность.
    - Многопоточность: Для приложений с большим количеством потоков важно, как сборщик мусора работает в многопоточном окружении.
3. Цели оптимизации:
    
    - Минимизация пауз (latency): Для приложения, где важно минимизировать задержки, сборщики мусора с низкими паузами предпочтительны.
    - Максимизация производительности (throughput): Важно для серверов или сервисов, обрабатывающих большой объем данных, где допустимы небольшие паузы.

### На каком этапе принимать решение:

1. Этап проектирования:
    - Когда ты знаешь требования к памяти и задержкам, можно сразу выбрать сборщик мусора в зависимости от типа приложения.
2. Этап тестирования и оптимизации:
    - После создания прототипа или тестовой версии приложения можно провести нагрузочные тесты с различными сборщиками мусора, чтобы увидеть, какой из них лучше справляется с задачами и удовлетворяет требованиям приложения.
3. Этап масштабирования:
    - Если приложение изначально разрабатывалось для небольших нагрузок, но со временем оно выросло, может потребоваться смена сборщика мусора для более эффективной работы с большими кучами и потоками.

### Пример выбора:

- Если твое приложение — это **финансовый сервис** с критичными требованиями к задержкам, используй **ZGC** или **Shenandoah** для минимизации пауз.
- Для **серверного приложения** с высокой пропускной способностью, которое не так чувствительно к задержкам, подойдет **Parallel GC**.
- Если ты работаешь с **большими объемами данных**, но хочешь предсказуемых пауз, **G1 GC** станет хорошим выбором