## Что такое шаг цены?

Шаг цены — это минимальное изменение цены определенного инструмента.

### Пример

Шаг цены для инструмента = `0.1`, а последняя цена = `10.5` — это означает, что заявка может быть выставлена по одной из следующих цен:

* 10.4
* 10.5
* 10.6

Цена `10.55` будет некорректной и заявка не будет выставлена.

```scala
def isValidPrice(price: BigDecimal, increment: BigDecimal): Boolean = {
    price % increment == 0
}

isValidPrice(10.1, 0.1) // true
isValidPrice(10.16, 0.1) // false
```

##Цены облигаций и фьючерсов

Цены облигаций и фьючерсов в TINKOFF INVEST API предоставляются в пунктах. Методика расчёта стоимости 
лота в валюте отличается в зависимости от типа биржевого инструмента. 

###Перевод цены облигации в валюту 

Пункты цены для котировок облигаций представляют собой проценты номинала облигации. Для пересчёта пунктов
в валюту можно воспользоваться формулой: 

> **price** / 100 * **nominal**

Где 

* **price** — текущая котировка ценной бумаги;

* **nominal** — номинал облигации.

<a name="futures"></a>
###Перевод цены фьючерса в валюту

Стоимость фьючерсов так же предоставляется в пунктах, для пересчёта можно воспользоваться формулой: 
> **price** / **min_price_increment** * **min_price_increment_amount**

Где 

* **price** — текущая котировка ценной бумаги;

* **min_price_increment** — шаг цены;

* **min_price_increment_amount** — стоимость шага цены.

Так же при работе с фьючерсами важно учитывать размер гарантийного обеспечения. Узнать эти параметры фьючерсов
можно при помощи метода: [getFuturesMargin](/investAPI/instruments#getfuturesmargin). Подробнее про срочный
рынок читайте [тут](https://help.tinkoff.ru/forts/)

##Внебиржевые инструменты в TINKOFF INVEST API

В данный момент TINKOFF INVEST API предполагает работу только с биржевыми инструментами.

##Торговля бумагами Тинькофф через TINKOFF INVEST API

Из-за огромного количества скальперских сделок мы закрыли торговлю БПИФ от УК "Тинькофф Капитал" в TINKOFF INVEST API. 
Список бумаг смотрите [здесь](https://tinkoff.github.io/invest-openapi/).

##Валюты в TINKOFF INVEST API

Получить список доступных валют можно при помощи метода [getInstruments/currencies](/investAPI/instruments#currencies).

Обратите внимание, что лотность валют ограничена лотностью, которая предоставляет биржа. Например, операции
с Евро и Долларами возможны только на количества кратные 1000.

##Почему отличаются исторические цены в TINKOFF INVEST API и других источниках?

Исторические данные [Тинькофф Инвестиций](https://www.tinkoff.ru/invest/) могут отличаться от данных,
которые предоставляют другие сервисы. Связано это может быть как с различными источниками первичных данных,
так и с различными алгоритмами их обработки и агрегации.  