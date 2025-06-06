---
Определение: Минимизирует зависимости между классами, модулями
Примечание: Классы зависят друг от друга через интерфейсы или абстракции.
---
Если объекты в приложении сильно связанны, то любое их изменение приводит к изменениям во всех связанных объектах. А это неудобно и порождает множество проблем. Low coupling как раз говорит о том что необходимо, чтобы <mark style="background: #ABF7F7A6;">код был слабо связан и зависел только от абстракций</mark>. 

Слабая связанность так же встречается в [[../../../Java/Рефакторинг/SOLID]] как [[../../../Java/Рефакторинг/Dependency Inversion Principle]] и слабая связанность по сути это реализация [[../../../Java/Dependency Injection/Dependency Injection]] принципа. Когда мы уходим от конкретных реализаций и абстрагируемся на уровнях интерфейсов(которые легко подменять нужными нам реализациями), тогда код не завязан на определенные реализации.
