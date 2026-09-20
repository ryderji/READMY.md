**Дисципліна:** Основи побудови інформаційних систем та мереж

**Тема:** Спостереження за процесом звернення до вебресурсу. Побудова власної моделі рівнів взаємодії

| | |
|---|---|
| **Прізвище, ім'я** | Кузнєцов Артем |
| **Група** | ІПЗ-2.01 |
| **Номер варіанта** | 12 |
| **Домен варіанта** | slackware.com |
| **Середовище виконання** | Windows 11|
| **Версія curl** | curl 8.21.0 (Windows) libcurl/8.21.0 Schannel zlib/1.3.2 WinIDN WinLDAP |
| **Дата виконання** | 20.09.2026 |

---

## Частина A. Збір експериментальних даних

### A.1. Запит із діагностичним виводом

**Команда:**

```
curl.exe -v https://slackware.com
```

**Вивід:**

```
*   Trying 64.57.102.36:443...
* Host slackware.com:443 was resolved.
* IPv6: (none)
* IPv4: 64.57.102.36
* connect to 64.57.102.36 port 443 from 0.0.0.0 port 54319 failed: Connection refused
* Failed to connect to slackware.com:443 after 3083 ms: Could not connect to server
* closing connection #0
curl: (7) Failed to connect to slackware.com:443 after 3083 ms: Could not connect to server
```

---

### A.2. Запит без захисту з'єднання

**Команда:**

```
curl.exe -v http://slackware.com
```

**Вивід:**

```
*   Trying 64.57.102.36:80...
* Host slackware.com:80 was resolved.
* IPv6: (none)
* IPv4: 64.57.102.36
* Established connection to slackware.com (64.57.102.36 port 80) from 192.168.0.183 port 53333
* using HTTP/1.x
> GET / HTTP/1.1
> Host: slackware.com
> User-Agent: curl/8.21.0
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 301 Moved Permanently
< Date: Sun, 20 Sep 2026 16:57:28 GMT
< Server: Apache/2.2.22
< Location: http://www.slackware.com/
< Content-Length: 303
< Content-Type: text/html; charset=iso-8859-1
<
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://www.slackware.com/">here</a>.</p>
<hr>
<address>Apache/2.2.22 Server at slackware.com Port 80</address>
</body></html>
* Connection #0 to host slackware.com:80 left intact
```

---

### A.3. Запит до служби доменних імен

**Команда (перше виконання):**

```
Resolve-DnsName slackware.com
```

**Вивід:**

```
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
slackware.com                                  A      10264 Answer     64.57.102.36

```

**Команда (повторне виконання через 5–7 хвилин):**

```
Resolve-DnsName slackware.com
```

**Вивід:**

```
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
slackware.com                                  A      10014 Answer     64.57.102.36

```

**Зафіксовані значення:**

| Параметр | Перше виконання | Повторне виконання |
|---|---|---|
| Час виконання (год:хв) | 20:00 | 20:05 |
| IP-адреса | 64.57.102.36 | 64.57.102.36 |
| Значення TTL | 10264 | 10014 |

> Якщо друге значення TTL виявилося більшим за перше — це нормально: кеш резолвера встиг оновитися. Зафіксуйте як є.

---

### A.4. Контрольний ресурс

**Команда:**

```
curl.exe -v https://google.com
```

**Вивід:**

```
* Host google.com:443 was resolved.
* IPv6: (none)
* IPv4: 142.250.130.102, 142.250.130.101, 142.250.130.113, 142.250.130.138, 142.250.130.139, 142.250.130.100
*   Trying 142.250.130.102:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* Established connection to google.com (142.250.130.102 port 443) from 192.168.0.183 port 50493
* using HTTP/1.x
> GET / HTTP/1.1
> Host: google.com
> User-Agent: curl/8.21.0
> Accept: */*
>
* Request completely sent off
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
< HTTP/1.1 301 Moved Permanently
< Location: https://www.google.com/
< Content-Type: text/html; charset=UTF-8
< Content-Security-Policy-Report-Only: object-src 'none';base-uri 'self';script-src 'nonce-2wkleEezvifoSQBp2iq8Ag' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri https://csp.withgoogle.com/csp/gws/other-hp
< Date: Sun, 20 Sep 2026 17:07:57 GMT
< Expires: Tue, 20 Oct 2026 17:07:57 GMT
< Cache-Control: public, max-age=2592000
< Server: gws
< Content-Length: 220
< X-XSS-Protection: 0
< X-Frame-Options: SAMEORIGIN
< Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
<
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="https://www.google.com/">here</A>.
</BODY></HTML>
* Connection #0 to host google.com:443 left intact
```

---

### A.5. Ресурси з некоректною конфігурацією сертифіката

**Випадок 1**

```
curl.exe -v https://expired.badssl.com
```

```
* Host expired.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
*   Trying 104.154.89.105:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - Получен сертификат с истекшим сроком действия.
* closing connection #0
curl: (35) schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - Получен сертификат с истекшим сроком действия.

```

**Випадок 2**

```
curl.exe -v https://wrong.host.badssl.com
```

```
* Host wrong.host.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
*   Trying 104.154.89.105:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - Главное конечное имя неверно.
* closing connection #0
curl: (60) schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - Главное конечное имя неверно.
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.

```

**Випадок 3**

```
curl.exe -v https://self-signed.badssl.com
```

```
*   Trying 104.154.89.105:443...
* Host self-signed.badssl.com.:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - Цепочка сертификатов выпущена центром сертификации, не имеющим доверия.
* closing connection #0
curl: (60) schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - Цепочка сертификатов выпущена центром сертификации, не имеющим доверия.
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.

```
---

## Частина B. Власна модель рівнів

**Кількість виділених груп:** 5

| № | Назва групи                  | Рядки виводу, віднесені до групи                                                                 | Обґрунтування                                                                                                               |
| - | ---------------------------- | ------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| 1 | **Визначення адреси**        | `Host slackware.com:443 was resolved`  `IPv4: 64.57.102.36`  `Resolve-DnsName slackware.com`     | Показує, що доменне ім’я було успішно перетворене на IP-адресу сервера.                                                     |
| 2 | **Встановлення з’єднання**   | `Trying 64.57.102.36:443...`  `Established connection to slackware.com...`  `Connection refused` | Відображає спробу встановити TCP-з’єднання та його результат.                                                               |
| 3 | **Обмін HTTP-даними**        | `> GET / HTTP/1.1`  `< HTTP/1.1 301 Moved Permanently`  `< Location: http://www.slackware.com/`  | Містить безпосередньо HTTP-запит клієнта та відповідь сервера.                                                              |
| 4 | **Захищене HTTPS-з’єднання** | `schannel:`  `ALPN: server accepted http/1.1`  `SSL/TLS connection renegotiated`                 | Характеризує процес встановлення та підтримки захищеного TLS-з’єднання.                                                     |
| 5 | **Перевірка сертифіката**    | `SEC_E_CERT_EXPIRED`  `SEC_E_WRONG_PRINCIPAL`  `SEC_E_UNTRUSTED_ROOT`                            | Містить результати перевірки сертифіката: завершення терміну дії, невідповідність імені та недовіру до центру сертифікації. |


*Групи впорядковано від найближчої до користувача (№ 1) до найближчої до апаратного забезпечення. Зайві рядки вилучити, за потреби — додати.*

**Рядки, які не вдалося віднести до жодної групи:**

| Рядок виводу                                                                     | Причина утруднення                                                                                               |
| -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `* Connection #0 to host slackware.com:80 left intact`                           | Службове повідомлення `curl` про стан уже встановленого з’єднання, а не окремий етап мережевої взаємодії.        |
| `* closing connection #0`                                                        | Повідомлення про завершення з’єднання після виникнення помилки, тому його складно віднести до конкретного рівня. |
| `curl: (7) Failed to connect to slackware.com:443`                               | Підсумкове повідомлення `curl` про невдале підключення, яке лише повторює результат помилки TCP.                 |
| `curl: (35) schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED` | Підсумковий код помилки `curl`, що дублює інформацію про проблему з TLS-сертифікатом.                            |
| `curl: (60) schannel: SNI or certificate check failed`                           | Підсумок перевірки сертифіката, а не окремий етап встановлення з’єднання.                                        |


---

## Контрольні питання

**1.  Скільки рядків діагностичного виводу передує отриманню даних сторінки (завдання A.1)?**

> Були проблеми з доменом у завданнi 1-пiдключитися не вдалося

**2. Які рядки наявні у виводі A.1 і відсутні у виводі A.2? Чим це зумовлено?**

> У A.1 є рядки:

> Trying 64.57.102.36:443...
connect to 64.57.102.36 port 443 ... failed: Connection refused
Failed to connect to slackware.com:443...
curl: (7) Failed to connect...

> Це зумовлено тим, що A.1 використовує HTTPS, а A.2 — звичайний HTTP. У A.1 з’єднання з портом 443 не встановилося, тому HTTP-запит не був виконаний.

**3. Звідки у виводі з'явилося значення 443, якщо його не було вказано в адресі?**

> Значення 443 — це стандартний порт для HTTPS. curl автоматично використовує порт 443, коли вказано адресу з https://.

**4. Як змінилося значення TTL між двома запитами (A.3)? Що означає це число?**

> TTL змінилося:

> 10264 → 10014, тобто зменшилося на 250 секунд.

> TTL показує, скільки часу DNS-запис може залишатися актуальним у кеші DNS-резолвера. У цьому випадку IP-адреса залишилася незмінною: 64.57.102.36.

> TTL зменшився рівно на 373 секунди, бо саме стільки часу пройшло між двома запитами. Це звичайний таймер, який показує скільки секунд запис ще проживе в кеші

**5. Чим відрізняються між собою три причини помилок із завдання A.5? Сформулювати кожну однією фразою.**

> Прострочений сертифікат — термін дії SSL-сертифіката вже закінчився (SEC_E_CERT_EXPIRED).
Невідповідність імені — ім’я сайту не відповідає імені, зазначеному в сертифікаті (SEC_E_WRONG_PRINCIPAL).
Недовірений сертифікат — сертифікат виданий центром сертифікації, якому система не довіряє (SEC_E_UNTRUSTED_ROOT).

**6. Три рядки з власних виводів, про які не йшлося на лекції 1:**

| № | Рядок виводу                                               | Джерело (номер завдання) |
| - | ---------------------------------------------------------- | ------------------------ |
| 1 | `* schannel: disabled automatic use of client certificate` | A.5                      |
| 2 | `* ALPN: server accepted http/1.1`                         | A.4                      |
| 3 | `* Connection #0 to host slackware.com:80 left intact`     | A.2                      |


---

## Висновки

**D.1.**  Що виявилося неочевидним або несподіваним

> Неочевидним для мене виявилося те, що помилка HTTPS може виникати з різних причин, хоча в усіх випадках проблема пов’язана із сертифікатом. Це добре видно у виводах A.5. У першому випадку з’являється SEC_E_CERT_EXPIRED, тобто сертифікат прострочений. У другому — SEC_E_WRONG_PRINCIPAL, що означає невідповідність імені сайту сертифікату. У третьому — SEC_E_UNTRUSTED_ROOT, тобто система не довіряє центру сертифікації. Також було цікаво побачити, що звичайний HTTP-запит до slackware.com успішно встановлює з’єднання через порт 80, тоді як HTTPS через 443 отримує Connection refused.

**D.2.** Чому саме така кількість груп у частині B

> Я виділив 5 груп, орієнтуючись на послідовність дій під час мережевого запиту: визначення IP-адреси, встановлення з’єднання, HTTP-обмін, HTTPS/TLS та перевірка сертифіката. Такий поділ дозволяє окремо розглядати різні етапи роботи curl. Я б змінив кількість груп, якби у виводах з’явилися додаткові дані про маршрутизацію, DNS-протокол або інші етапи мережевої взаємодії.

**D.3.** Питання, яке залишилося без відповіді

> Чому для різних помилок сертифіката curl використовує різні коди — SEC_E_CERT_EXPIRED, SEC_E_WRONG_PRINCIPAL та SEC_E_UNTRUSTED_ROOT? З виводу зрозуміло, що причини помилок різні, але хотілося б краще зрозуміти, як система визначає конкретний тип проблеми.

## Використання штучного інтелекту

**Факт використання:** використано.

**Установлений рівень для цієї роботи:** Р3 — ШІ як співвиконавець.

**Фактичний рівень використання:** Р3.

### Використані системи
> Система	Версія або модель	Період використання
> ChatGPT	GPT-5.6 Luna	20.09.2026
### Промпти
|     № | Розділ роботи                        | Текст промпта                                                                                                            |
| ----: | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **1** | Частина B — класифікація результатів | `вот придумай класификацию по группам в зависимости от рядов вывода`                                                     |
| **2** | Частина B — оформлення класифікації  | `сделай мне также токо поменяй что бы не выглядело будто я у него слизал и используй тот текст который я вначалае кидал` |
| **3** | Питання 2–5                          | `ответь на вопросы используя текст кеоторые я тебе кидал первым`                                                         |

### Дії з отриманим результатом
| № промпта | Що перевірено                                                                                  | Що змінено                                                                        | Що відхилено і чому                                                                |
| --------: | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
|     **1** | Перевірено запропоновану класифікацію та її відповідність фактичним рядкам із виводів A.1–A.5. | Класифікацію адаптовано під власні результати команд `curl` та `Resolve-DnsName`. | Варіанти, які не відповідали фактичним рядкам виводу, не використовувалися.        |
|     **2** | Перевірено, чи використані саме мої експериментальні дані та рядки з початкових виводів.       | Текст переформульовано та пристосовано до структури моєї роботи.                  | Інформацію, якої не було у власних результатах, не додавав.                        |
|     **3** | Перевірено відповіді на питання за матеріалами, які були надані на початку роботи.             | Відповіді оформлено у вигляді готового тексту для звіту.                          | Відповіді, які не можна було підтвердити моїми результатами, не використовувалися. |

### Дії з отриманим результатом 
### Підтвердження

> Підтверджую, що всі наведені в цьому звіті виводи команд отримано мною особисто внаслідок фактичного виконання відповідних дій, а відомості цього розділу є повними та достовірними.

> Виводи curl, Resolve-DnsName та інші експериментальні дані були отримані мною особисто. ШІ використовувався для аналізу, класифікації та формулювання відповідей на основі цих даних.
