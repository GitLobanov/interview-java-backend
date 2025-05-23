# Вставка

## Процесс вставки элемента в `HashMap`

1. **Хэширование ключа**:
    - Когда вы вставляете элемент, `HashMap` сначала вычисляет хэш-код ключа с помощью метода `hashCode()`.
    - Хэш-код используется для определения, в какой бакет (ведро) будет помещён элемент. Хэш-код может быть преобразован с помощью дополнительной обработки (например, с использованием маскировки и сдвига) для уменьшения вероятности коллизий.
2. **Определение бакета**:
    - Хэш-код преобразуется в индекс бакета в массиве бакетов. Индекс бакета вычисляется как `index = hash & (array.length - 1)`, где `array.length` — это размер массива бакетов. Это гарантирует, что индекс находится в пределах допустимого диапазона.
3. **Поиск подходящего бакета**:
    - `HashMap` проверяет, существует ли уже бакет по вычисленному индексу. Если бакет пуст, создаётся новый список для хранения элементов.
    - Если бакет уже содержит элементы (т.е., есть коллизии), необходимо определить, является ли элемент с таким же ключом уже в бакете.
4. **Добавление элемента**:
    - Если в бакете нет элемента с таким же ключом, новый элемент добавляется в бакет (в конец связного списка, дерево, или другой используемый контейнер).
    - Если элемент с таким же ключом уже существует, его значение обновляется.
5. **Балансировка и расширение**:
    - При вставке элементов количество элементов может достигнуть определённого порога, что может привести к необходимости увеличения размера массива бакетов и повторному размещению существующих элементов (rehash).

## Важные аспекты вставки в `HashMap`

- **Распределение элементов**: Хорошее распределение элементов в бакетах помогает избежать коллизий и поддерживать эффективное время выполнения операций. Распределение зависит от реализации хэш-функции.
    
- **Коллизии**: Когда два ключа имеют одинаковый хэш-код, они попадают в один бакет. Для управления коллизиями используется связный список, сбалансированное дерево или другая структура данных.
    
- **Порог нагрузки (Load Factor)**: `HashMap` имеет порог нагрузки, который определяет, когда нужно увеличивать размер массива бакетов. По умолчанию это 0.75, что означает, что когда количество элементов превышает 75% от размера массива бакетов, **размер массива удваивается**.
    
- **Rehashing**: При расширении массива бакетов и перемещении элементов в новые бакеты, необходимо пересчитать их индексы и перераспределить. Это может быть ресурсоёмкой операцией, но она позволяет сохранить эффективность доступа к элементам.
    
## Расширение и повторное хеширование

Если количество элементов в `HashMap` достигает 75% от текущего размера массива бакетов, выполняется следующее:

1. Создаётся новый, более крупный массив бакетов.
2. Все существующие элементы перемещаются в новые бакеты на основе пересчитанных хэш-кодов.
3. Новый размер массива бакетов и порог нагрузки обновляются.

Эти шаги помогают поддерживать баланс между временем выполнения операций и использованием памяти, что делает `HashMap` эффективным и гибким инструментом для хранения данных.

## LoadFactor

Load factor (коэффициент загрузки) — это параметр, который определяет, насколько заполненной должна стать хэш-таблица (например, в `HashMap`), прежде чем ее размер будет увеличен (то есть произойдет рехэширование). Он измеряет соотношение между количеством элементов в таблице и ее текущим размером.

- Если load factor = 0.75 (по умолчанию для `HashMap`), это значит, что при заполнении 75% таблицы она будет увеличена в размере (обычно вдвое).

- Меньший load factor снижает количество коллизий и повышает скорость доступа, но увеличивает потребление памяти.
- Больший load factor экономит память, но может увеличить количество коллизий, что замедляет работу.


## Resources

- [The Java HashMap Under the Hood](https://www.baeldung.com/java-hashmap-advanced)