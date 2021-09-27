# Протокол HTTP. Основы работы с консолью разработчика в браузере


## Цель

Получить практические навыки работы с HTTP протоколом. На практике отметить заголовки присущие Request и Response с помощью консоли разработчика в браузере в различных web-ресурсах.


## Постановка задачи

Написать эссе, отражающее особенности как минимум 10 уникальных HTTP-заголовков для Request и Response.


## Эссе

Рассмотреть и проанализировать заголовки я решил на сайте [15 тегов об Алексее Павлове](https://server-edu.me/15-tags-of-me/index.html).

![15 тегов об Алексее Павлове](images/15-tags-of-Aleksei-Pavlov.png)

При переходе на гачимучный сайт of **dungeon master** браузер отправляет следующий запрос:

```yaml
:authority: server-edu.me
:method: GET
:path: /15-tags-of-me/index.html
:scheme: https
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
accept-encoding: gzip, deflate, br
accept-language: en-GB,en-US;q=0.9,en;q=0.8,ru;q=0.7
cache-control: max-age=0
if-modified-since: Fri, 24 Sep 2021 16:56:23 GMT
if-none-match: W/"614e0337-2cc"
sec-ch-ua: "Chromium";v="94", "Google Chrome";v="94", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
sec-fetch-dest: document
sec-fetch-mode: navigate
sec-fetch-site: cross-site
sec-fetch-user: ?1
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36
```

Рассмотрим некоторые из них:

- Заголовок `accept-encoding` объявляет, какие кодировки контента и алгоритмы сжатия поддерживает браузер. В данном случае браузер объявляет о том, что поддерживает методы сжатия gzip, deflate и br.

- Заголовок `accept-language` сообщает серверу, какие языки клиент понимает и какая локаль предпочтительнее. Также в заголовке может указываться параметр `q`, который устанавливает предпочтение в выборе данной локали.

- Заголовок `cache-control` устанавливает инструкции для кеширования данных. В данном случае задается максимальное время в течение которого ресурс будет считаться актуальным - 0 секунд.

- Заголовок `upgrade-insecure-requests` говорит о том, хотим ли мы получить сайт в защищенном режиме. В данном случае браузер запрашивает сайт именно в защищенном режиме (1).

- Заголовок `user-agent` позволяет идентифицировать браузер. Файл агента пользователя чаще всего включает в себя сведения о браузере, его версии, используемом устройстве, операционной системе и механизме веб-рендеринга.

В ответ на данный запрос мы получаем следующее:

```yaml
date: Sun, 26 Sep 2021 14:17:10 GMT
etag: "614e0337-2cc"
last-modified: Fri, 24 Sep 2021 16:56:23 GMT
server: nginx/1.18.0 (Ubuntu)
```

- Заголовок `date` содержит дату и время, в которое сообщение было создано.

- Заголовок `last-modified` содержит дату и время, в которую, по мнению удалённого сервера, запрашиваемый ресурс был изменён.

- Заголовок `etag` является идентификатором специфической версии ресурса. Он же является более надёжным механизм проверки неизменности сайта по сравнению с `last-modified`. Сравнение этих заголовков позволяет быстро определить являются ли два представления ресурса одними и теме же - если хеши разные, то и представления ресурса разные.

При нажатии на кнопку `play` музыкального клипа на данной странице, мы постоянно отправляем на сервер подобный запрос:

```yaml
:authority: server-edu.me
:method: GET
:path: /15-tags-of-me/super-video.mp4
:scheme: https
accept: */*
accept-encoding: identity;q=1, *;q=0
accept-language: en-GB,en-US;q=0.9,en;q=0.8,ru;q=0.7
range: bytes=262144-
referer: https://server-edu.me/15-tags-of-me/index.html
sec-ch-ua: "Chromium";v="94", "Google Chrome";v="94", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
sec-fetch-dest: video
sec-fetch-mode: no-cors
sec-fetch-site: same-origin
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36
```

- Заголовок `range` позволяет получать не весь объект, а лишь его часть, не хранящуюся в кеше. Если объект полностью сохранен в кеше, то браузер больше не будет отправлять подобный запрос для получения частей файла, но здесь есть один **нюанс**.

Если мы во время просмотра перейдём на момент, который не сохранен в кеше, то отправим подобный запрос:

```yaml
:authority: server-edu.me
:method: GET
:path: /15-tags-of-me/super-video.mp4
:scheme: https
accept: */*
accept-encoding: identity;q=1, *;q=0
accept-language: en-GB,en-US;q=0.9,en;q=0.8,ru;q=0.7
if-range: "614def25-725247"
range: bytes=6356992-7492166
referer: https://server-edu.me/15-tags-of-me/index.html
sec-ch-ua: "Chromium";v="94", "Google Chrome";v="94", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
sec-fetch-dest: video
sec-fetch-mode: no-cors
sec-fetch-site: same-origin
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36
```

Здесь появляется новый заголовок `if-range`, который позволяет проверить, не изменился ли файл во время просмотра. Также немного видоизменился заголовок `range` - в первом случае мы запрашивали всё, начиная с **262144** байт файла, а во втором запрашивали диапазон **6356992-7492166** байт.

В первом и втором случае мы получили следующий ответ:

```yaml
Content-Length: 7230023
Content-Range: bytes 262144-7492166/7492167
content-type: video/mp4
date: Mon, 27 Sep 2021 01:05:01 GMT
etag: "614def25-725247"
last-modified: Fri, 24 Sep 2021 15:30:45 GMT
server: nginx/1.18.0 (Ubuntu)
```

- Заголовок `Content-Length` указывает размер отправленного получателю тела объекта в байтах.

- Заголовок `Content-Range` указывает, какие байты сервер предоставил.

Во втором случае мы спрашивали, является ли тело объекта актуальным, и так как заголовок запроса `if-range` и заголовок ответа `etag` равны, мы не загружали с сервера заново **6356992-7492166** байт, так как уже при первом запросе мы их загрузили.


## Вывод

Таким образом мы успешно проверили сайт [15 тегов об Алексее Павлове](https://server-edu.me/15-tags-of-me/index.html) на отсутствие скам-механизмов, забирающих из вашего кошелька **three hundred bucks**.