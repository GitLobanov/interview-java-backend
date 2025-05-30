В процессе разработки чего-либо у каждого из нас (наверное) возникали такие ситуации, когда переменная должна иметь определённые (заранее известные) значения - константы. Вместо того, чтобы плодить список констант, их все можно _перечислить_ в классе, который был придуман специально для этого. Подобные классы так и называются - **классы перечислений** или просто **enum**.

Класс перечислений позволяет создать набор значений, которые могут быть использованы как единственно допустимые значения переменной. Объявляется такой класс при помощи ключевого слова **enum** перед словом **class**.

```kotlin
enum class ColorType {
  RED,
  BLUE,
  GREEN
}

var color: ColorType 
color = ColorType.RED
```

## Конструкторы классов перечислений

Класс перечислений может иметь конструктор, который используется для инициализации каждого значения (константы) в классе.

```kotlin
enum class ColorType(val rgb: Int) {
  RED(0xFF0000),
  BLUE(0x0000FF),
  GREEN(0x00FF00)
}

...

var color: ColorType = ColorType.RED
println(color.rgb)
```

Свойства и функции можно **открыть** для наследования, тогда каждая константа сможет реализовать их по-своему. Либо не реализовывать вовсе.

```kotlin
enum class ColorType {
  RED {
    override fun colorName() = "Красный"
    override val rgb = "0xFF0000"
    },
  BLUE {
    override fun colorName() = "Голубой"
    override val rgb = "0x0000FF"
    },
  GREEN;

  open fun colorName() = "Красный, Голубой, Зелёный"
  open val rgb = "0xFFFFFF"
}
```

Также свойства и функции можно сделать абстрактными, но тогда все константы будут обязаны их переопределить.

```kotlin
enum class ColorType {
  RED {
    override fun colorName() = "Красный"
    override val rgb = "0xFF0000"
    },
  BLUE {
    override fun colorName() = "Голубой"
    override val rgb = "0x0000FF"
    },
  GREEN {
    override fun colorName() = "Зелёный"
    override val rgb = "0x00FF00"
  };

  abstract fun colorName(): String
  abstract val rgb: String
}
```

## Стандартные функции для работы с enum

По своей структуре класс перечислений напоминает список с определёнными значениями. Поэтому есть стандартная функция `values()`, которая помогает вывести все значения, преобразуя их в список.

``` kotlin
println(ColorType.values().asList())  // выведет - [RED, BLUE, GREEN]
```

Для получения константы по её имени используется функция `valueOf()`.

```kotlin
println(ColorType.valueOf("RED"))   // выведет - RED
println(ColorType.valueOf("red"))   // вылетит ошибка, т.к. чувствителен к регистру
println(ColorType.valueOf("PINK"))  // вылетит ошибка - такого цвета нет
```

Начиная с версии Kotlin 1.1 можно использовать более универсальные функции `enumValues<T>()` и `enumValueOf<T>()`. По сути делают тоже самое, что и описанные выше. Так в чём же их универсальность? А в том, что с их помощью можно реализовать такую функцию, которая будет работать со всеми классами перечислений в проекте.

```kotlin
inline fun <reified T : Enum<T>> printAllValues() {
    print(enumValues<T>().joinToString { it.name })
}

printAllValues<ColorType>()  // вместо ColorType может быть указан любой класс перечислений
```

Также каждая константа в классе перечислений имеет неявные поля **name** и **ordinal**, которые хранят имя и номер позиции соответственно.

```kotlin
val colorRed = ColorType.RED 
println(colorRed.name) // выведет - RED println(colorRed.ordinal) // выведет - 0
```



