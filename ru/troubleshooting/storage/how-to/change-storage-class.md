# Как изменить класс хранения


## Описание сценария {#case-description}

Необходимо изменить класс хранения и разобраться, применится ли изменение к уже хранящимся объектам.

## Решение {#case-resolution}

Смена класса хранилища в свойствах бакета применяется только для вновь загружаемых в бакет объектов.
Если объекты уже были загружены ранее с использованием другого класса хранения, тип хранения для них не изменится на новый автоматически.

Вы можете сменить  класс хранилища для таких объектов с помощью жизненного цикла. О том, как это сделать, пишем [в документации](https://server-yfm.website.yandexcloud.net/server-yfm-512b5eed-aa26-4020-9137-a0cd5f030640-yc/ru/ru/storage/concepts/lifecycles).

{% note info "Заголовок примечания" %}

Правила жизненного цикла в {{ objstorage-short-name }} срабатывают в 00:00 UTC ежедневно.

{% endnote %}

Также для смены класса хранилища у уже загруженных объектов вы можете воспользоваться графическими или консольными S3-клиентами:

* [WinSCP](../../../storage/tools/winscp.md);

* [CyberDuck](../../../storage/tools/cyberduck.md);

* [AWS CLI](../../../storage/tools/aws-cli.md);

* [s3cmd](../../../storage/tools/s3cmd.md).