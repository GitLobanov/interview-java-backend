Коммитит прочитанные сообщения в отдельный топик 

```Topic
_consumer_offsets
```

Включающем в себя топик, партицию, группу консьюмеров, а также смещение.

![[Pasted image 20240928135307.png]]

```properties
enable.auto.commit_ = true // Слушатель сохраняет предыдущий offset
auto.commit.interval.ms_=6000 // сохранения смещения каждые 6 секунд
auto.offset.reset = earliest / latest // Выбор считивания очередия для новых групп консьюмеров
```