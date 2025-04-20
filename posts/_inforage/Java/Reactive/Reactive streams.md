![](../../../_res/Pasted%20image%2020250211100038.png)

Это стандартный способ асинхронной обработки в потоковом стиле. В него входят следующие интерфейсы: _subscriber, publisher, subscription и processor_. 

Принцип работы _reactive streams_:

Subscriber подписывается на _publisher_(subscribe()), но общаться с _publisher_ будет через _subscription_. 

_Subscription_ получает данные от _publisher_ и отгружает их в подписчика (onNext(data)). 

C помощью методов _onError()_ и _onComplete() Subscription_ принимает от _Subscriber_ информацию о том, сколько данных он хочет получить от _publisher_ через метод _request(n)_. Старый добрый паттерн Наблюдатель, в лучшей реализации.