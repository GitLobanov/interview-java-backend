Ключевое слово `object` позволяет одновременно объявить класс и создать его экземпляр (или другими словами, объект). При этом использовать его можно по-разному:

- объявление объекта;
- реализация объекта-компаньона;
- запись объекта-выражения (также известен как анонимный объект и object expressions).

---

## Объявление объекта

Наверняка вам приходилось хоть раз создавать такой класс, который должен существовать в одном экземпляре. Обычно это реализуется при помощи паттерна проектирования [синглтон](https://ru.wikipedia.org/wiki/%D0%9E%D0%B4%D0%B8%D0%BD%D0%BE%D1%87%D0%BA%D0%B0_(%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F) "ru.wikipedia.org"). В Kotlin же предлагается использовать **объявление объекта**, которое одновременно сочетает в себе и объявление класса, и создание его единственного экземпляра.

Объявляется объект при помощи ключевого слова `object`, после которого следует имя объекта.

```kotlin
object SomeObject {
  ...
}
```

Как и класс, объект может обладать свойствами, функциями, блоками инициализации, может наследоваться от классов и реализовывать интерфейсы. Единственное, что не допускается - основные и вторичные конструкторы.

Объекты можно объявлять внутри класса. Такие объекты тоже существуют в единственном экземпляре, т.е. у любого экземпляра класса будет один и тот же экземпляр объекта.

```kotlin
class Someclass {
  ...
  object SomeObject {
    fun create() {
      ...
    }
  }

}

...

val someClass = SomeClass.SomeObject.create()
```

## Объект-компаньон (Companion Object)

Как упоминалось ранее, объекты можно объявлять внутри класса. При этом нет каких-либо ограничений по их количеству. Но лишь один объект можно пометить ключевым словом `companion`. Такому объекту можно не указывать имя, а к его компонентам обращаться через имя класса, в котором он находится.

```kotlin
class SomeClass {

  companion object {
    fun create()
  }
}

...

val someClass = SomeClass.create()
```

Как правило объекты-компаньоны используются для объявления переменных и функций, к которым требуется обращаться без создания экземпляра класса. Либо для объявления [констант](https://bimlibik.github.io/posts/kotlin-const-modifier/). По сути они своего рода замена статическим членам класса (в отличие от Java, в Kotlin нет статики).
## Объект-выражение

Объект-выражение - это выражение, которое “на ходу” создает анонимный объект, который в свою очередь является заменой анонимным внутренним классам в Java.

При разработке приложений анонимный объект чаще всего используется для реализации обработчика событий (клики по компонентам экрана).

```kotlin
button.setOnClickListener(object : View.OnClickListener{
  override fun onClick(v: View?) {
    ...
  }
})
```

В данном примере создаётся объект, который реализует интерфейс `View.OnClickListener` и передаётся функции `setOnClickListener()` в качестве параметра. Этот параметр и является объектом-выражением.

Обратите внимание, что для объекта-выражения не указывается имя. Зачем оно ему, если объект просто передается функции в качестве параметра?

Если же объекту всё таки требуется имя, то его можно сохранить в переменной.

```kotlin
val listener = object : View.OnClickListener{
  override fun onClick(v: View?) { ...}
}
```

В объекте-выражении можно обращаться к переменным, которые находятся в той же функции, в которой был создан анонимный объект.

```kotlin
fun countClicks() {
  var count = 0

  button.setOnClickListener(object : View.OnClickListener{
    override fun onClick(v: View?) {
      count++
    }
  })
}
```

Анонимный объект может реализовывать несколько интерфейсов, тогда они перечисляются через запятую. А может и вовсе не реализовывать ни один.

```kotlin
val tree = object {
    var name = "Сосна"
    var description = "хвойное дерево с длинными иглами и округлыми шишками"
}
print("${tree.name} - ${tree.description}.")

val obj = object : InterfaceA, InterfaceB {
    override fun doSomethingA() {
        println("Выполняется действие A")
    }

    override fun doSomethingB() {
        println("Выполняется действие B")
    }
}
```

Обратите внимание, что анонимные объекты не являются синглтонами. Каждый раз при выполнении объекта-выражения создаётся новый объект.
# Ресурсы

- [Bimblibik](https://bimlibik.github.io/posts/kotlin-object-keyword/)
- [Object Expressions and Declarations](https://kotlinlang.org/docs/reference/object-declarations.html "kotlinlang.org") - официальная документация.  
- [Анонимные объекты и объявление объектов](https://kotlinlang.ru/docs/reference/object-declarations.html "kotlinlang.ru") - неофициальный перевод документации на русский язык.  
- [Kotlin: статика, которой нет](https://habr.com/ru/company/funcorp/blog/430836/ "habr.com") - статья об использовании статики в Kotlin. Есть немного про объекты - сравнение кода Java - Kotlin.