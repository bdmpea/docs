# Устранение ошибки 413 при выполнении операции `git push`


## Описание проблемы {#issue-description}

При попытке выполнить команду отправки данных в репозиторий {{ mgl-name }} отображается сообщение об ошибке:

```
error: RPC failed; 
HTTP 413 curl 22 
The requested URL returned error: 413
```

## Решение {#issue-resolution}

Изменить настройки Nginx для конкретного инстанса не получится – максимальный размер `git push` при использовании HTTP ограничен 250 МБ.

В качестве решения этой проблемы вы можете:

1. Использовать загрузку по протоколу SSH.
1. Отправить код в репозиторий частями.

Подробнее этот процесс описан [в документации GitHub](https://docs.github.com/en/get-started/using-git/troubleshooting-the-2-gb-push-limit).