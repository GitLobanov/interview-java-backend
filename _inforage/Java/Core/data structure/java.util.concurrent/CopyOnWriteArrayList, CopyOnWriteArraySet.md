Две данные коллекции являются иммутабельными, то есть после создания их невозможно поменять, и чтобы модифицировать коллекцию, нужно создать новый экземпляр и только потом записать новые данные.

Именно из-за иммутабельности нет необходимости синхронизовывать разные версии коллекций и не нужно производить дополнительные блокировки для чтения и итерации. Итератор этих коллекций отображает состояние на момент начала итерации и не знает ничего о новых записях.

`CopyOnWriteArrayList, CopyOnWriteArraySet` выгодно использовать только в случаях, когда чтений коллекции значительно больше, чем ее изменений, потому что копирование всех данных для их последующего изменения - очень дорогая операция как для памяти, так и вычислительно.

## Сравнение с Синхронизированными коллекциями

![[../../../../../_res/Pasted image 20241116122104.png]]

Итак, конкурентные коллекции в целом производительнее, чем синхронные, и зачастую стоит выбирать именно их. Но синхронизированные коллекции предоставляют более строгие гарантии: ни чтение, ни итератор не может вывести "неактуальные" данные. Если вам критично использовать самое свежее состояние коллекции, можно выбрать синхронизированные, но при этом помнить об их влиянии на производительность.