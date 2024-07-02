---
title: "Поиск по картинкам {{ search-api-name }}"
description: "На этой странице вы узнаете о том, как использовать {{ search-api-name }} для отправки поисковых запросов по картинкам и получения поисковой выдачи в формате XML."
---

# GET-запросы

{{ search-api-name }} позволяет выполнять поиск изображений по индексу [Яндекс Картинок](https://yandex.ru/images) с заданным набором параметров и получать результат поиска в формате XML. Параметры поиска передаются в сервис в виде HTTP-запроса методом GET. {{ search-api-name }} формирует [ответ](./pic-response.md) в виде документа в формате XML.

{% include [text-search-intro](../../_includes/search-api/text-search-intro.md) %}

## Формат запроса {#get-request-format}

```httpget
https://yandex.<домен>/images-xml
  ? [folderid=<идентификатор_каталога>]
  & [apikey=<API-ключ>]
  & [text=<текст_поискового_запроса>]
  & [groupby=<настройки_группировки_результатов>]
  & [p=<номер_страницы>]
  & [fyandex=<фильтр_семейный_поиск>]
  & [site=<доменное_имя_сайта>]
  & [itype=<формат_картинки>]
  & [iorient=<тип_ориентации_изображения>]
  & [isize=<размер_картинки>]
  & [icolor=<цвет_картинки>]
```

### Параметры запроса {#parameters}

* `folderid` — [идентификатор каталога](../../resource-manager/operations/folder/get-id.md) сервисного аккаунта, от имени которого вы отправляете запросы. Обязательный параметр.
* `apikey` — [API-ключ](../../iam/concepts/authorization/api-key.md) сервисного аккаунта. Обязательный параметр.
* `text` — текст поискового запроса. Например: `funny+cats`. Обязательный параметр.

    {% note warning %}

    Специальные символы, передаваемые в качестве значений параметров, необходимо заменять на соответствующие экранированные последовательности в соответствии с percent-encoding. Например, вместо знака равно `=` используйте последовательность `%3D`, а вместо пробела — `+`.

    {% endnote %}

* `groupby` — настройки группировки результатов. С помощью параметра `groups-on-page` задается количество групп результатов, которое выводится на одной странице с результатами поиска. Необязательный параметр. Если параметр не задан, значение по умолчанию — `20`. Одна группа результатов содержит один документ.
    Например, чтобы выводить на одной странице по пять групп с результатами, укажите `groupby=attr=ii.groups-on-page=5`.

* `p` — номер запрашиваемой страницы поисковой выдачи. Нумерация страниц начинается с нуля (первой странице соответствует значение `0`). Необязательный параметр. По умолчанию возвращается первая страница поисковой выдачи.
* `fyandex` — использование фильтра «Семейный поиск» при формировании поискового ответа. Необязательный параметр. Если параметр не задан, фильтр не используется. Возможные значения:
    * `0` — фильтр «Семейный поиск» выключен.
    * `1` — фильтр «Семейный поиск» включен.
* `site` — поиск картинок только на указанном сайте. Например, чтобы выполнить поиск только по сайту `somepics.ru`, укажите `site=somepics.ru`. Необязательный параметр. Если параметр не задан, поиск выполняется по всем сайтам поисковой базы. 
* `itype` — поиск картинок указанного формата. Необязательный параметр. Если параметр не задан, выполняется поиск по картинкам всех форматов. Возможные значения:
    * `jpg` — поиск картинок в формате [JPG](https://ru.wikipedia.org/wiki/JPEG).
    * `gif` — поиск картинок в формате [GIF](https://ru.wikipedia.org/wiki/GIF).
    * `png` — поиск картинок в формате [PNG](https://ru.wikipedia.org/wiki/PNG).

* `iorient` — поиск картинок с указанной ориентацией изображения. Необязательный параметр. Если параметр не задан, выполняется поиск по картинкам с любой ориентацией изображения. Возможные значения:
    * `horizontal` — поиск картинок с горизонтальной ориентацией изображения.
    * `vertical` — поиск картинок с вертикальной ориентацией изображения.
    * `square` — поиск картинок с равными сторонами (квадрат).

* `isize` — поиск картинок указанного размера. Необязательный параметр. Если параметр не задан, выполняется поиск по картинкам всех размеров. Возможные значения:
    * `enormous` — поиск картинок очень большого размера (размеры в пикселях более 1600 × 1200).
    * `large` — поиск картинок большого размера (размеры в пикселях от 800 х 600 до 1600 × 1200).
    * `medium` — поиск картинок среднего размера (размеры в пикселях от 150 × 150 до 800 × 600).
    * `small` — поиск картинок маленького размера (размеры в пикселях от 32 × 32 до 150 × 150).
    * `tiny` — поиск иконок (размеры в пикселях не более 32 × 32).
    * `wallpaper` — поиск обоев для рабочего стола.

* `icolor` — поиск картинок, содержащих указанный цвет. Необязательный параметр. Если параметр не задан, выполняется поиск по картинкам с любыми цветами. Возможные значения:
    * `gray` — черно-белые;
    * `color` — цветные;
    * `red` — красные;
    * `orange` — оранжевые;
    * `yellow` — желтые;
    * `green` — зеленые;
    * `cyan` — голубые;
    * `blue` — синие;
    * `violet` — фиолетовые;
    * `white` — белые;
    * `black` — черные.

### Пример поискового запроса {#sample-request}

Следующий запрос вернет третью страницу результатов поиска картинок по запросу `funny cats`. Тип поиска — `{{ ui-key.yacloud.search-api.test-query.label_search_type-russian }}` (yandex.ru). Сервис вернет результаты для найденных на сайте `somepics.ru` цветных картинок среднего размера в формате JPG с горизонтальной ориентацией изображения. К результатам поиска будет применен фильтр «Семейный поиск». Страница будет содержать три группы результатов поиска.

```html
https://yandex.ru/images-xml?folderid=b1gt6g8ht345********&apikey=your_service_account_API_key********&text=funny+cats&groupby=attr=ii.groups-on-page=3&p=2&fyandex=1&site=somepics.ru&itype=jpg&iorient=horizontal&isize=medium&icolor=color
```

#### См. также {#see-also}

* [{#T}](./pic-response.md)
* [{#T}](../operations/searching.md)