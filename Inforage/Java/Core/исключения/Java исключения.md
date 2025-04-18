**Исключение в Java** — это ошибка, которая происходит во время работы программы и мешает ей выполнять задачи.

![[hierarchy-exceptions.png]]


# Unchecked RuntimeException

|№|Исключение и описание|
|---|---|
|1|**java.lang.ArithmeticException** Арифметическая ошибка, например, деление на ноль.|
|2|**java.lang.ArrayIndexOutOfBoundsException** Индекс массива выходит за пределы.|
|3|**java.lang.ArrayStoreException** Присвоение элементу массива несовместимого типа.|
|4|**java.lang.ClassCastException** Недопустимое приведение типов.|
|5|**java.lang.IllegalArgumentException** Недопустимый аргумент, используемый для вызова метода.|
|6|**java.lang.IllegalMonitorStateException** Недопустимая работа монитора, например, ожидание разблокированного потока.|
|7|**java.lang.IllegalStateException** Окружающая обстановка или приложение находится в неправильном состоянии.|
|8|**java.lang.IllegalThreadStateException** Запрошенная операция несовместима с текущим состоянием потока.|
|9|**java.lang.IndexOutOfBoundsException** Некоторый тип индекса находится за пределом.|
|10|**java.lang.NegativeArraySizeException** Массив создан с отрицательным размером.|
|11|**java.lang.NullPointerException** Недопустимое использование нулевой ссылки.|
|12|**java.lang.NumberFormatException** Неверное преобразование строки в числовой формат.|
|13|**java.lang.SecurityException** Попытка нарушить безопасность.|
|14|**java.lang.StringIndexOutOfBounds** Попытка индексирования за пределами строки.|
|15|**java.lang.UnsupportedOperationException** Была обнаружена неподдерживаемая операция.|

Ниже приведен список контролируемых исключений (Checked Exceptions) в Java, определенных в java.lang.

# Checked Exceptions

|№|Исключение и описание|
|---|---|
|1|**java.lang.ClassNotFoundException** Класс не найден.|
|2|**java.lang.CloneNotSupportedException** Попытка клонировать объект, который не реализует Cloneable интерфейс.|
|3|**java.lang.IllegalAccessException** Запрещен доступ к классу.|
|4|**java.lang.InstantiationException** Попытка создать объект абстрактного класса или интерфейса.|
|5|**java.lang.InterruptedException** Один поток был прерван другим потоком.|
|6|**java.lang.NoSuchFieldException** Запрошенное поле не существует.|
|7|**java.lang.NoSuchMethodException** Запрошенный метод не существует.|