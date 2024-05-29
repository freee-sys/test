# About
Система доставки еды представляет собой комплексный сервис, включающий три роли: Администратор, Пользователь и Курьер.

### Пользователь
Пользовательская часть системы ориентирована на удобство и интуитивность. Пользователь может просматривать ассортимент доступных товаров, детально изучать информацию о каждом продукте, включая описание, цену и отзывы. После выбора нужных товаров пользователь добавляет их в корзину, где можно корректировать количество и наименование товаров. Завершив выбор, пользователь формирует заказ, указывая адрес доставки и способ оплаты. Также пользователь может отслеживать статус своего заказа в реальном времени и получать уведомления о текущем состоянии доставки.

### Курьер
Курьерская часть системы предназначена для упрощения работы курьеров. Курьеры могут видеть список назначенных им заказов, просматривать детальную информацию о каждом заказе, включая адрес доставки и контактную информацию пользователя. После успешной доставки курьер отмечает заказ как выполненный. Также у курьеров есть доступ к отчетам о выполненных заказах, где отображается сумма заработанных денег за каждый выполненный заказ. Это позволяет курьерам легко отслеживать свой доход и планировать рабочее время.

### Администратор
Администратор играет ключевую роль в обеспечении качества сервиса и решении возникающих проблем. Администратор имеет доступ к истории заказов всех пользователей и курьеров, что позволяет анализировать и решать проблемы, связанные с доставкой. Например, если возникли задержки или недовольство клиента, администратор может оперативно выяснить причины и принять меры для их устранения. Кроме того, администратор может просматривать статистику работы сервиса, включая количество выполненных заказов, среднее время доставки, оценки пользователей и другие важные показатели. Это помогает в стратегическом планировании и улучшении качества сервиса.

# Use case
![Pasted image 20240524193704](https://github.com/Misha224/test/assets/114202713/f7da4634-3fb7-49e0-8f94-4ce72f1ed3f3)
```
@startuml
left to right direction
actor "Пользователь" as User
actor "Курьер" as Courier
actor "Администратор" as Admin

rectangle DeliverySystem {
    User --> (Просмотр информации о товаре)
    User --> (Добавление товара в корзину)
    User --> (Формирование заказа)

    Courier --> (Просмотр информации о заказе)
    Courier --> (Просмотр отчета о заработке)

    Admin --> (Просмотр истории заказов)
    Admin --> (Просмотр статистики работы сервиса)
}

@enduml
```
# Sequence diagram
![Pasted image 20240524194544](https://github.com/Misha224/test/assets/114202713/2da55348-a6f8-442c-833c-48fbc8189c22)

```plantuml
@startuml
actor Пользователь as User
participant "Приложение" as App
participant "Сервер" as Server
participant "База данных" as DB

User -> App: Просмотр информации о товаре
activate App
App -> Server: Запрос информации о товаре
activate Server
Server -> DB: Получить информацию о товаре
activate DB
DB -> Server: Информация о товаре
deactivate DB
Server -> App: Информация о товаре
deactivate Server
App -> User: Отображение информации о товаре
deactivate App

User -> App: Добавление товара в корзину
activate App
App -> Server: Обновить корзину
activate Server
Server -> DB: Сохранить данные корзины
activate DB
DB -> Server: Подтверждение сохранения
deactivate DB
Server -> App: Подтверждение обновления корзины
deactivate Server
App -> User: Товар добавлен в корзину
deactivate App

User -> App: Формирование заказа
activate App
App -> Server: Создать заказ
activate Server
Server -> DB: Сохранить заказ и данные пользователя
activate DB
DB -> Server: Подтверждение сохранения
deactivate DB
Server -> App: Подтверждение создания заказа
deactivate Server
App -> User: Заказ сформирован
deactivate App

@enduml

```
# Диаграмма состояний
![Pasted image 20240524194631](https://github.com/Misha224/test/assets/114202713/c1b5cbfc-00e2-40f6-aee6-5e99a3a54823)

```plantuml
@startuml
[*] --> Заказ_сформирован

Заказ_сформирован : Статус изменен на "Заказ сформирован"
Заказ_сформирован --> Курьер_назначен : Назначить курьера

Курьер_назначен : Статус изменен на "Курьер назначен"
Курьер_назначен --> Курьер_забрал_заказ : Курьер забрал заказ

Курьер_забрал_заказ : Статус изменен на "Курьер забрал заказ"
Курьер_забрал_заказ --> Курьер_отдал_заказ : Курьер отдал заказ

Курьер_отдал_заказ : Статус изменен на "Курьер отдал заказ"
Курьер_отдал_заказ --> [*]

Заказ_сформирован --> Заказа_нет_в_наличии : Проверка наличия
Заказа_нет_в_наличии : Статус изменен на "Заказа нет в наличии"
Заказа_нет_в_наличии --> [*]
@enduml
```

# Activity diagram
![Pasted image 20240524210512](https://github.com/Misha224/test/assets/114202713/2afb69d0-93f5-4b54-9316-5a1b94b51578)

```plantuml
@startuml
|Пользователь|
start
:Выбор товаров;
:Добавление товаров в корзину;
:Формирование заказа;
:Отправка заказа;

|Приложение|
:Получение заказа;
:Проверка наличия товаров;
if (Все товары в наличии?) then (да)
    :Создание заказа;
    :Назначение курьера;
    :Отправка уведомления курьеру;
else (нет)
    :Уведомление пользователя о нехватке товара;
    :Отмена заказа;
    stop
endif

|Курьер|
:Получение уведомления о заказе;
:Просмотр информации о заказе;
:Забор заказа;
:Доставка заказа пользователю;
:Подтверждение доставки;

|Приложение|
:Получение подтверждения доставки;
:Обновление статуса заказа;

|Администратор|
:Просмотр истории заказов;
:Анализ и решение проблем с заказами (если есть);
stop
@enduml

```

# Диаграмма классов
![Pasted image 20240524195136](https://github.com/Misha224/test/assets/114202713/a3e94b58-d96f-441f-8d0c-e0662005625e)

```plantuml
@startuml
package "Система доставки еды" {
    
    class Пользователь {
        - id: int
        - имя: string
        - адрес: string
        - телефон: string
        + просматриватьТовар(): void
        + добавлятьТоварВКорзину(): void
        + формироватьЗаказ(): void
    }
    
    class Курьер {
        - id: int
        - имя: string
        - телефон: string
        + просматриватьИнформациюОЗаказе(): void
        + просматриватьОтчетОЗаработке(): void
    }

    class Администратор {
        - id: int
        - имя: string
        + просматриватьИсториюЗаказов(): void
        + просматриватьСтатистикуРаботыСервиса(): void
    }

    class Товар {
        - id: int
        - название: string
        - описание: string
        - цена: float
        + получитьИнформацию(): void
    }

    class Заказ {
        - id: int
        - дата: datetime
        - статус: string
        + создать(): void
        + изменитьСтатус(): void
    }

    Пользователь --> Заказ : формирует
    Пользователь --> Товар : просматривает
    Заказ --> Товар : содержит
    Курьер --> Заказ : доставляет
    Администратор --> Заказ : просматривает
    Администратор --> Курьер : управляет
    Администратор --> Пользователь : управляет
}
@enduml
```
