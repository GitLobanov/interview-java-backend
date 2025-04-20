Наследование от такого класса осуществляется с помощью оператора `:`. При этом абстрактному классу не нужен модификатор [`open`](https://bimlibik.github.io/posts/kotlin-open-keyword/), потому что он “открыт” для наследования по умолчанию.

```kotlin
class Pine : Tree() {
  ...
}
```

В теле класса можно объявлять абстрактные свойства и функции. Это полезно, когда часть поведения класса не имеет смысла без реализации в более конкретном подклассе.

```kotlin
abstract class Tree {
  abstract val name: String
  abstract val description: String
  abstract fun info()
}
```

Каждый наследник обязан переопределять их все.

```kotlin
class Pine : Tree() {
  override val name = "Сосна"
  override val description = "Хвойное дерево с длинными иглами и округлыми шишками"
  override fun info() = "$name - ${description.toLowerCase()}."  
}
```

Свойства и функции необязательно должны быть абстрактными. У них может быть обобщенная реализация, которая будет с пользой наследоваться всеми подклассами. В этом случае для них в абстрактном классе объявляется конкретная реализация, к которой имеют доступ все наследники.

```kotlin
abstract class Tree {
  abstract val name: String
  abstract val description: String
  fun info(): String = "$name - ${description.toLowerCase()}."
}

...

class Pine : Tree() {
  override val name = "Сосна"
  override val description = "Хвойное дерево с длинными иглами и округлыми шишками"
}

...

val pine = Pine()
println(pine.info())
```

Так как этот компонент класса уже не будет абстрактным, наследники не смогут его переопределить. Потому-что все по-умолчанию final.

Чтобы это исправить нужно явно задать модификатор [`open`](https://bimlibik.github.io/posts/kotlin-open-keyword/) для функции с конкретной реализацией. Тогда у наследников появляется выбор: либо не переопределять функцию и использовать реализацию суперкласса, либо переопределить и указать свою собственную реализацию.

У абстрактного класса может быть конструктор.

```kotlin
abstract class Tree(val name: String, val description: String) {
  open fun info(): String = "$name - ${description.toLowerCase()}."
}
```

Тогда каждый наследник должен предоставить для него значения.

```kotlin
class Pine(name: String, description: String) : Tree(name, description)

...

val pine = Pine("Сосна", "Хвойное дерево с длинными иглами и округлыми шишками")
println(pine.info())
```

