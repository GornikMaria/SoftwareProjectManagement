 Диаграмма демонстрирует **процесс взаимодействия пользователя с ботом** для регистрации или входа в систему. Она описывает шаги, необходимые для успешной идентификации пользователя, а также возможные результаты этих действий.


```plantuml
|Пользователь|
start

:Запуск бота;

if (Регистрация или вход?) then (Регистрация)
    :Предоставление фотографии для регистрации;
    :Предоставление фио для регистрации;
    :Сохранение фотографии и фио в системе;

    |Система|
    :Подтверждение успешной регистрации;
    
    |Пользователь|
    :Предоставление лица для входа;

    |Система|
    if (Идентификация успешна?) then (Да)
        :Просмотр своих блюд на сегодня;
    else (Нет)
        :Ошибка идентификации;
        :Предложение повторить попытку или зарегистрироваться;
    endif

else (Вход)
    :Предоставление лица для входа;

    |Система|
    if (Идентификация успешна?) then (Да)
        :Просмотр своих блюд на сегодня;
    else (Нет)
        :Ошибка идентификации;
        :Предложение повторить попытку или зарегистрироваться;
    endif
endif

stop
@enduml
```
