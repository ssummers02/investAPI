#Сервис котировок

Данный сервис предназначен для получения различной (в т.ч. исторической) биржевой информации. 
Существует два варианта взаимодействия с сервисом котировок: 

1. **Unary-методы** — данный вариант следует использовать в случаях, когда не требуется оперативность получения
информации или для загрузки исторических данных. Существует ограничение на количество запросов в минуту,
подробнее: [Лимитная политика](/investAPI/limits).
2. **Bidirectional-stream** — используется для получения биржевой информации в реальном времени с минимально 
возможными задержками. Для работы со стрим-соединениями также существуют ограничения, согласно [лимитной политике](/investAPI/limits).


##Получение исторических свечей

В процессе разработки торгового робота требуется анализировать различную историческую информацию. Например,
свечи. Для получения исторических свечей по инструменту можно использовать метод [getCandles](/investAPI/marketdata#getcandles).

Обратите внимание, что максимально допустимый период получения свечей за один запрос — 1 календарный год. 
Получение данных за более длинный период возможно поочередным вызовом метода. 

##Получение последних цен

В процессе работы алгоритма торговли может потребоваться получение цены последней сделки по инструменту или
инструментам. Для реализации данной потребности следует использовать метод [getLastPrices](/investAPI/marketdata#getlastprices).
Используя данный метод вы можете получить последние цены всех доступных для торговли инструментов — для 
этого требуется передать пустой массив *instruments*.

Если для реализации алгоритма требуется оперативно получать информацию о цене последней сделки, то команда
TINKOFF INVEST API рекомендует использовать [подписку на поток обезличенных сделок](/investAPI/marketdata#subscribetradesrequest) 
в рамках [stream-соединения сервиса](/investAPI/marketdata#marketdatastream).

##Получение текущего стакана

Биржевой стакан — один из ключевых показателей торгового инструмента. Стакан содержит заявки пользователей
на покупку или продажу определённого инструмента (подробнее: [Биржевой стакан](https://www.tinkoff.ru/invest/account/help/trade-on-bs/bids/#q13)).
Для получения стакана можно использовать метод [getOrderbook](/investAPI/marketdata#getorderbook). 

Для высоколиквидных инструментов на бирже стакан может изменяться несколько раз за секунду, поэтому 
использование unary-метода не всегда будет удобным. Поэтому рекомендуется использовать 
[подписку на стаканы](/investAPI/marketdata#subscribeorderbookrequest) в рамках 
[stream-соединения сервиса](/investAPI/marketdata#marketdatastream).

<a name="stream"></a>

##Bidirectional-stream получения биржевой информации

В рамках stream-соединения можно получать поток интересующих данных. В рамках одного соединения пользователь
может подписаться на получение:

1. стаканов; 

2. свечей; 

3. потока обезличенных сделок; 

4. статуса торговли инструментов.

**Важно.** В рамках одного запроса можно управлять подпиской только одного типа данных. Т.е. чтобы подписаться на свечи 
и стаканы требуется отправить два запроса [marketdataRequest](/investAPI/marketdata#marketdatarequest), 
содержащие информацию об изменении статуса подписки на свечи и стаканы соответственно. 

Обратите внимание, что максимальное количество подписок на одно соединение ограничено [лимитной политикой](/investAPI/limits/) 
TINKOFF INVEST API. Однако, это ограничение **не распространяется** на подписку *info* (получение торгового
статуса инструмента).