---
Определение: Позволяет добавлять в программу новые операции, не изменяя классы объектов, над которыми эти операции могут выполняться.
Тип:
  - Поведенческий
пример: Определяет операции для элементов структуры без изменения их классов.
---
Visitor

Добавляем метод в который можно передавать посетителей, посетителю передается обернутый объект класса, который он посетил.
### Проблема

Ваша команда разрабатывает приложение, работающее с геоданными в виде графа. Узлами графа являются городские локации: памятники, театры, рестораны, важные предприятия и прочее. Каждый узел имеет ссылки на другие, ближайшие к нему узлы. Каждому типу узлов соответствует свой класс, а каждый узел представлен отдельным объектом.

![[Pasted image 20240901145413.png]]

Ваша задача — сделать экспорт этого графа в XML. Дело было бы плёвым, если бы вы могли редактировать классы узлов. Достаточно было бы добавить метод экспорта в каждый тип узла, а затем, перебирая узлы графа, вызывать этот метод для каждого узла. Благодаря полиморфизму, решение получилось бы изящным, так как вам не пришлось бы привязываться к конкретным классам узлов.

Но, к сожалению, классы узлов вам изменить не удалось. Системный архитектор сослался на то, что код классов узлов сейчас очень стабилен, и от него многое зависит, поэтому он не хочет рисковать и позволять кому-либо его трогать.

![[Pasted image 20240901145434.png]]

К тому же он сомневался в том, что экспорт в XML вообще уместен в рамках этих классов. Их основная задача была связана с геоданными, а экспорт выглядит в рамках этих классов чужеродно.

Была и ещё одна причина запрета. Если на следующей неделе вам бы понадобился экспорт в какой-то другой формат данных, то эти классы снова пришлось бы менять.
### Решение 

Паттерн Посетитель предлагает разместить новое поведение в отдельном классе, вместо того чтобы множить его сразу в нескольких классах. Объекты, с которыми должно было быть связано поведение, не будут выполнять его самостоятельно. Вместо этого вы будете передавать эти объекты в методы посетителя.

Код поведения, скорее всего, должен отличаться для объектов разных классов, поэтому и методов у посетителя должно быть несколько. Названия и принцип действия этих методов будут схожи, но основное отличие будет в типе принимаемого в параметрах объекта, например:

```python
class ExportVisitor implements Visitor is
    method doForCity(City c) { ... }
    method doForIndustry(Industry f) { ... }
    method doForSightSeeing(SightSeeing ss) { ... }
    // ...
```

Здесь возникает вопрос: как подавать узлы в объект-посетитель? Так как все методы имеют отличающуюся сигнатуру, использовать полиморфизм при переборе узлов не получится. Придётся проверять тип узлов для того, чтобы выбрать соответствующий метод посетителя.

```java
foreach (Node node in graph)
    if (node instanceof City)
        exportVisitor.doForCity((City) node)
    if (node instanceof Industry)
        exportVisitor.doForIndustry((Industry) node)
    // ...
```

Тут не поможет даже механизм перегрузки методов (доступный в Java и C#). Если назвать все методы одинаково, то неопределённость реального типа узла всё равно не даст вызвать правильный метод. Механизм перегрузки всё время будет вызывать метод посетителя, соответствующий типу `Node`, а не реального класса поданного узла.

Но паттерн Посетитель решает и эту проблему, используя механизм [двойной диспетчеризации](https://refactoring.guru/ru/design-patterns/visitor-double-dispatch). Вместо того, чтобы самим искать нужный метод, мы можем поручить это объектам, которые передаём в параметрах посетителю. А они уже вызовут правильный метод посетителя.

```java
// Client code
foreach (Node node in graph)
    node.accept(exportVisitor)

// City
class City is
    method accept(Visitor v) is
        v.doForCity(this)
    // ...

// Industry
class Industry is
    method accept(Visitor v) is
        v.doForIndustry(this)
    // ...
```

Как видите, изменить классы узлов всё-таки придётся. Но это простое изменение позволит применять к объектам узлов и другие поведения, ведь классы узлов будут привязаны не к конкретному классу посетителей, а к их общему интерфейсу. Поэтому если придётся добавить в программу новое поведение, вы создадите новый класс посетителей и будете передавать его в методы узлов.

### Пример

```java
// Создадим интерфейс для посетителя, который будет определять способ обновления курсов валют.

interface Visitor {
    void visit(ExchangeRateUpdater updater);
}


// Создадим конкретные посетители, которые реализуют логику обновления из разных источников.

class ApiVisitor implements Visitor {
    private String apiEndpoint;

    public ApiVisitor(String apiEndpoint) {
        this.apiEndpoint = apiEndpoint;
    }

    @Override
    public void visit(ExchangeRateUpdater updater) {
        // Логика для обновления курсов валют из API
        System.out.println("Fetching exchange rates from API: " + apiEndpoint);
        // Имитация получения данных и обновление
        updater.updateExchangeRate("USD_EUR", 0.87);  // Обновленный курс
    }
}

class ManualVisitor implements Visitor {
    private Map<String, Double> newRates;

    public ManualVisitor(Map<String, Double> newRates) {
        this.newRates = newRates;
    }

    @Override
    public void visit(ExchangeRateUpdater updater) {
        // Логика для обновления курсов валют вручную
        System.out.println("Updating exchange rates manually");
        for (Map.Entry<String, Double> entry : newRates.entrySet()) {
            updater.updateExchangeRate(entry.getKey(), entry.getValue());
        }
    }
}


// Класс `CurrencyExchangeRate` реализует паттерн Одиночка и добавляет метод `accept`, который позволяет посетителю обновлять данные.


import java.util.HashMap;
import java.util.Map;

public class CurrencyExchangeRate {
    private static CurrencyExchangeRate instance;
    private Map<String, Double> exchangeRates;

    private CurrencyExchangeRate() {
        exchangeRates = new HashMap<>();
        // Инициализация начальных значений курсов валют
        exchangeRates.put("USD_EUR", 0.85); // 1 USD = 0.85 EUR
        exchangeRates.put("EUR_USD", 1.18); // 1 EUR = 1.18 USD
    }

    public static synchronized CurrencyExchangeRate getInstance() {
        if (instance == null) {
            instance = new CurrencyExchangeRate();
        }
        return instance;
    }

    public double getExchangeRate(String currencyPair) {
        return exchangeRates.getOrDefault(currencyPair, 0.0);
    }

    public void updateExchangeRate(String currencyPair, double rate) {
        exchangeRates.put(currencyPair, rate);
    }

    // Метод для принятия посетителя
    public void accept(Visitor visitor) {
        visitor.visit(new ExchangeRateUpdater(this));
    }
}

// Создадим класс `ExchangeRateUpdater`, который будет использоваться для передачи `CurrencyExchangeRate` в посетителей.

class ExchangeRateUpdater {
    private CurrencyExchangeRate exchangeRate;

    public ExchangeRateUpdater(CurrencyExchangeRate exchangeRate) {
        this.exchangeRate = exchangeRate;
    }

    public void updateExchangeRate(String currencyPair, double rate) {
        exchangeRate.updateExchangeRate(currencyPair, rate);
    }
}

// Теперь мы можем использовать `CurrencyExchangeRate` вместе с посетителями для обновления курсов валют.


public class CurrencyConverter {
    public static void main(String[] args) {
        // Получаем единственный экземпляр CurrencyExchangeRate
        CurrencyExchangeRate exchangeRate = CurrencyExchangeRate.getInstance();

        // Создаем посетителей
        Visitor apiVisitor = new ApiVisitor("https://example.com/api");
        Map<String, Double> manualRates = new HashMap<>();
        manualRates.put("USD_EUR", 0.86); // Обновленный курс
        Visitor manualVisitor = new ManualVisitor(manualRates);

        // Обновляем курсы валют с помощью посетителей
        exchangeRate.accept(apiVisitor);   // Обновление из API
        exchangeRate.accept(manualVisitor); // Ручное обновление

        // Получаем обновленный курс валют
        double usdToEurRate = exchangeRate.getExchangeRate("USD_EUR");
        System.out.println("Updated USD to EUR rate: " + usdToEurRate);
    }
}
```

- **CurrencyExchangeRate** — это одиночка, который управляет курсами валют.
- **Visitor** — интерфейс для посетителей, которые выполняют обновление данных.
- **ApiVisitor** и **ManualVisitor** — конкретные реализации посетителей для обновления данных из API и вручную.
- **ExchangeRateUpdater** — класс, который предоставляет интерфейс для обновления курсов валют и используется посетителями.
### Шаги реализации

1. Создаем интерфейс посетителя и объявляем в нём методы «посещения» для каждого класса элемента, который существует в программе.
    
2. Описываем интерфейс элементов. Если работаем с уже существующими классами, то объявляем абстрактный метод принятия посетителей в базовом классе иерархии элементов.
    
3. Реализуем методы принятия во всех конкретных элементах. Они должны переадресовывать вызовы тому методу посетителя, в котором тип параметра совпадает с текущим классом элемента.
    
4. Иерархия элементов должна знать только о базовом интерфейсе посетителей. С другой стороны, посетители будут знать обо всех классах элементов.
    
5. Для каждого нового поведения создаем конкретный класс посетителя. Приспосабливаем это поведение для работы со всеми типами элементов, реализовав все методы интерфейса посетителей.
    
    Вы можете столкнуться с ситуацией, когда посетителю нужен будет доступ к приватным полям элементов. В этом случае вы можете либо раскрыть доступ к этим полям, нарушив инкапсуляцию элементов, либо сделать класс посетителя вложенным в класс элемента, если вам повезло писать на языке, который поддерживает вложенность классов.
    
6. Клиент будет создавать объекты посетителей, а затем передавать их элементам, используя метод принятия.
### Отношения с другими паттернами

- Посетителя можно рассматривать как расширенный аналог Команд, который способен работать сразу с несколькими видами получателей.
    
- Можно выполнить какое-то действие над всем деревом [[GoF Компоновщик]] при помощи посетителя.
    
- Посетитель можно использовать совместно с [[GoF Итератор|итератором]]. [[GoF Итератор]] будет отвечать за обход структуры данных, а _Посетитель_ — за выполнение действий над каждым её компонентом. [[EX. Итератор и Посетитель]]