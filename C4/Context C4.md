
- **Контейнеры:**
    
    - **Web Application:** Фронтенд веб-приложения (React). Интерфейс для сотрудников.
    - **Kitchen Application:** Фронтенд приложения для кухни (React). Интерфейс для работников кухни с Face Recognition.
    - **API Application:** Бэкенд (ASP.NET Core). Предоставляет REST API для взаимодействия между фронтендом и базой данных, а также для взаимодействия с Telegram-ботом.
    - **Telegram Bot:** Обрабатывает команды от пользователей в Telegram и отправляет уведомления.
    - **Database:** База данных (MSSQL). Хранит всю информацию о пользователях, меню, заказах и т.д.
    - **Face Recognition Service:** Отдельный сервис, отвечающий за распознавание лиц. Может быть реализован на C# или Python (с использованием библиотек типа Dlib, OpenCV, Face++) и предоставлять API (gRPC или REST). Вынесение в отдельный сервис обеспечивает лучшую масштабируемость и позволяет использовать наиболее подходящие технологии для распознавания.
        
- **Взаимодействия:**
    - Пользователи взаимодействуют с Web Application через HTTPS.
    - Работники кухни взаимодействуют с Kitchen Application через HTTPS.
    - Все фронтенд-приложения (Web Application, Kitchen Application) взаимодействуют с API Application через REST API по HTTPS.
    - Telegram Bot взаимодействует с пользователями через Telegram API и с API Application через REST API.
    - API Application читает и записывает данные в Database с помощью SQL-запросов.
    - Kitchen Application взаимодействует с Face Recognition Service через API (gRPC или REST) для распознавания лиц.
    - Telegram Bot взаимодействует с Telegram через Telegram API


```plantuml
@startuml
!include <c4/C4_Container>

System_Boundary(c1, "TL Food System") {
    Person(user, "Сотрудник компании", "Заказывает еду")
    Person(cook, "Работник кухни", "Получает заказы")
    Person(accountant, "Бухгалтер", "Получает отчеты")

    ContainerDb(database, "Database", "Relational Database (MSSQL)", "Хранит данные о пользователях, меню, заказах, предпочтениях, отзывах.")

    Container(web_app, "Web Application", "TypeScript, React", "Предоставляет пользовательский интерфейс для заказа еды, просмотра меню, управления настройками.")
    Rel(user, web_app, "Использует", "HTTPS")

    Container(kitchen_app, "Kitchen Application", "TypeScript, React", "Предоставляет интерфейс для кухни: распознавание лиц, отображение заказов.")
    Rel(cook, kitchen_app, "Использует", "HTTPS")

     Container(tg_bot, "Telegram Bot", "C#", "Обрабатывает команды от пользователей, отправляет уведомления.")
     Rel(user, tg_bot, "Взаимодействует", "Telegram API")

    Container(api, "API Application", "C#, ASP.NET Core", "Предоставляет API для взаимодействия между компонентами системы.")
    Rel(web_app, api, "Использует", "HTTPS, REST API")
    Rel(kitchen_app, api, "Использует", "HTTPS, REST API")
    Rel(tg_bot, api, "Использует", "HTTPS, REST API")

    Rel(api, database, "Читает/записывает", "SQL")

    Container(face_recognition, "Face Recognition Service", "C# / Python", "Сервис распознавания лиц.")
     Rel(kitchen_app, face_recognition, "Использует", "API (gRPC/REST)")
}
  System_Ext(telegram, "Telegram", "Платформа обмена сообщениями")
  Rel(tg_bot, telegram, "Взаимодействует", "Telegram API")

@enduml
```