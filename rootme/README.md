# :computer: rootme :octocat:
Здесь Вы можете увидеть решения заданий с платформы rootme (https://www.root-me.org/)
## Категории:
- [Web категория](#WEBкатегория)

## WEB категория
Я совместила две отдельных категории Веба в одну, т.к. - обычно они представляются одной категорией.
Совмещены будут 'Веб - сервер' и 'Веб - Клиент'
#### ___Описание заданий с категорий___:
Эти задачи сталкивают вас с использованием языков сценариев и программирования на стороне клиента. В основном это скрипты, которые нужно анализировать и понимать. Это позволит вам изучить языки, которые широко используются в Интернете.
Эти задачи предназначены для обучения пользователей работе с HTML, HTTP и другими серверными механизмами. Следующая серия задач будет способствовать лучшему пониманию таких методов, как : Базовая работа нескольких механизмов аутентификации, обработка данных форм, внутренняя работа веб-приложений и т.д.
# 
# 
### `HTML - Source code`

Подсказка дается уже в самом название. Нам нужно зайти в код страницы. Попробуем поискать спрятанный флаг (пароль) и наткнемся на ЭТО:
![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/first_task.png)
# 
# 
### `HTTP - User-agent`

Название снова подсказыает нам на то место, куда смотреть. Поэтому мы должны воспользоваться Burp Suite и перехватить запрос с сайта. Теперь мы видим другой User-Agent (то, с какого браузера мы зашли). Меняем его на "admin":
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/2task.png)
Форвардим запрос и перезагружаем страницу. Или кидаем его в Repeater и наживаем "Send". Покажу пример с Repeater:
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/2_1task.png)
#
#
### `File upload - Double extensions`
В задании нужно прочесть ".passwd". А сделать это можно с помощью загрузки файла. Чем мы собственно и займемся.

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/file_upload_1.png)

Если мы попытаемся загрузить файл расширения ".php". Ничего не выйдет, как показано на скрине.

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/file_upload_2.png)

Пишем самый легкий php код. На вывод файла ".passwd". Но не забываем при загрузке файла сделать расширение - ".php.png".

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/file_upload_3.png)

И так файл загрузился, об этом нам говорит надпись. А теперь можно перейти в директорию.

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/file_upload_4.png)

Зашли и видим, что у нас вывелся флаг. Так что задание выполнено, заливаем его и радуемся :3

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/file_upload_5.png)
# 
# 
### `Insecure Code Management`
Открыв страницу начинаем анализировать, где может храниться код. 

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/git_code_1.png)

Перебрав различные варианты, понимаем, что то, что нам надо, хранится в папке .git

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/git_code_2.png)

Открыв ее, пониамем что копаться в ней надолго или мы сможем получить не всю информацию. Но есть другой выход, к примеру git-dumper.

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/git_code_3.png)

Пользуемся утилитой и выгружаем файлы в нашу папку на локальном ПК.

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/git_code_4.png)

А теперь заходим в файл config.php и находим пароль. Декодируем его и получаем ответ.

# 
# 
### `Java - Server-side Template Injection`
Название задания говорит нам о том, что это шаблонная инъекция или же 'template injection'. Мы видим форму и сразу понимаем что нужно сделать что-то с ней. Ищем разные пэйлоады. И по методологии, пытаемся подобрать нужный нам шаблон. Я использовала этот репозиторий:
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/ssti_1.png)

-тут позже допишу-
#
#
### `PHP - Filters`
Здесь нам нужно получить пароль админа. Тут у нас LFI, но не обычный, а с фильтром (об этом нам говорит название задания). Нам нужен php filter, для того, чтобы вывести содержимое файла.

https://kmb.cybber.ru/web/lfi/main.html - я воспользовалась этим сайтом и пэйлоадом оттуда.

![код страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_filter_1.png)

Теперь у нас есть содержимое файла, но в формате base64. В этот раз покажу как раскодировать строку через консоль.

![строка консоли](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_filter_2.png)

Здесь присутствует важная строка - "include(config.php)"; Именно она подсказывает нам, что нужно вместо файла "login.php", вставить "config.php" (делаем так же, как в прошлый раз). Мы все-таки же получаем строку base64. Все-так же идем в консоль.

![строка консоли](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_filter_3.png)

Теперь мы видим внутренность ".passwd". Остался последний шаг этого задания. Снова декодировать в base64.

![строка консоли](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_filter_4.png)

Теперь мы получили наш ответ и можем заливать его на сайт.
#
#
### `Local File Inclusion`

В задании говорится, что нам нужно зайти в секцию админа. А так же то, что уязвимость формата LFI. Перейдя по ссылке на задание, мы видим то, что страница имеет секции, попробуем перейти по ним. И видим что переход осуществляется через параметр. Так же видим файл index.html, который находится в открытом доступе как и остальные.

![сайт](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_1.png)

Так как это LFI, можно попытаться постаивить в параметр слово "admin". Но у нас ничего не выйдет, поэтому попытаемся сделать самый легкий вариант LFI'я, это вставить "../", можно просто оставить "../", но я сразу допишу "admin"

![сайт](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_2.png)

У нас появился файл "index.html", попробуем перейти в этот файл и поищим что-то похожее на флаг. А вот и он.

![сайт](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_3.png)
#
#
### `Local File Inclusion - Double encoding`
Нам нужно найти пароль в исходных файлах сайта. Получается попробуем "php://filter/convert.base64-encode/resource=cv", чтобы прочесть файл. И кажется тут фильтр, атаку распознали.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_1.png)

И так, теперь мы понимаем что оно реагирует на спец.знаки. Значит используем URL decode, но дублируем кодировку два раза, так как задание дает нам это понять (double).

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_2.png)

И так, теперь у нас есть исходник страницы. Но с ходу можно сказать, что это все base64. Значит декодируем.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_3.png)

Декодирую я снова все через консоль. `<?php include("conf.inc.php"); ?>` - дает нам понять, что прочесть нужно именно файл "conf". 

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_4.png)

Теперь меняем прошлый пэйлоад со слова "cv" на "conf". И получаем все такой же base64 код. Осталось снова декодировать.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_lfi_double_5.png)

Снова показываю пример декодирования через консоль. А вот и наш обещанный флаг. 
#
#
### `SQL injection - Authentication`

Название говорит нам о том, что перед нами SQl инъекция. А это означает, что мы будем работать с формой. Попробуем пробежаться по стандартным знакам для инъекции.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/sql_aut_1.png)

Перебрав разное, мы видим то, что колонки будут "username".

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/sql_aut_2.png)

Так же при таком пэйлоаде, можно понять, что это база данных "SQlite3".

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/sql_aut_3.png)

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/sql_aut_4.png)

Теперь нам нужно использовать (как пример) такой пэйлоад - `' OR username='admin'; --`

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/sql_aut_5.png)

А вот и ответ (нам нужно зайти в Код Элемента и он будет перед нами).
#
#
### `LDAP injection - Authentication`
Нам предоставляется стараница с аутентификацией. Само название (как и большинство на платформе root me) дает нам понять, что мы имеем дело с LDAP.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/ldap_1.png)

Попробуем отправить любой "адекватный запрос", но мы ничего не получим, вернее ошибку. Так что попробуем полистать пэйлоады и подставлять уже их. Ну и здесь используем "сигналы" LDAP (некоторый ряд символов, который позволит понять, есть ли уязвимость). Перепробовав несколько вариантов, мы понимаем, что здесь есть уязвимость. А помогает нам ")". На нее с ошибкой реагирует форма и дает увидить запрос.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/ldap_2.png)

Теперь осталось сформировать нормальный запрос. И скажу так, здесь подойдут разные варианты, вот некоторые примеры:

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/ldap_4.png)

А теперь мы получили наш флаг. Но на странице его вроде нет? А все потому что он в коде, зайдите туда и он будет там.

![скрин сайта](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/ldap3.png)

# 
# 
### `PHP - Serialization`
Естественно в этом задании нужно обращать внимание на код, который нам дан, так что при выполнении задания я обращалась именно к нему (white box).

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser.png)

В задании нужно получить доступ админа. Поэтому запомним это и перейдем на страницу задания. Тут по условию вводим данные гостя (guest) и не забываем нажать галочку о том, чтобы нас запомнили, это понадобится для создания нашей сессии. Собственно, сама сессия нужна для ее подмены под данные суперадмина.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_2.png)

Теперь видим, что мы действительно смогли зайти под данными гостя (guest). Следующим шагом предлагаю перейти в куки, то место, где создалась наша сессия. Если что, утилита называется "EditThisCookie" (та, что на скрине).

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_3.png)

Тут сразу видно, что это URL кодировка. Время использовать мой любимый "CyberShef". 
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_4.png)

Но давайте разберем структуру этого куки подробнее. __Все будет описано в примере ниже.__ Во первых нужно отметить то, что "s" означает "size" (размер). Т.е. - нам нужно изменить это при смене "quest" на "superadmin". Так же последнее значение тоже поменялось, с сигнала "s" на "b", что означает boolean (лог. 1 или 0). Вставляем единицу, т.к. нам нужно true.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_5.png)

Теперь дисконектимся (отключаемся) и видим вот это. Но не забывайте сохранить измененные куки (иначе будете как я сидеть 30 минут и не понимать, что не так).

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/php_ser_6.png)
# 
# 
### `JSON Web Token (JWT) - Introduction`
Нам дается регистрационная страница и мы видим опцию "Login as Guest!". Делаем мы это для того, чтобы нам присвоился jwt токен.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_1.png)

Пользуемся моим любимым расширением "EditThisCookie" (то, что на скрине). И теперь пробуем раскопать то, что нам дано.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_2.png)

Поговорим о структуре jwt токенов. То, что обозначено фиолетовым - base64. Остальная же часть - нам не нужна, отбрасываем. Теперь понимаме, что по заданию нам нужно подменить значения, из-за доступа админа. Так что из первой фиолетовой части убираем шифрование и ставим значение "none", а во второй фиолетовой части меняем значение на "admin", но не забываем перевести все в base64.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_3.png)


{"typ":"JWT","alg":"none"} = "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0="

{"username":"admin"} = "eyJ1c2VybmFtZSI6ImFkbWluIn0="

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_4.png)

Убираем знак "=" на конце, так как это лишний хвост. Вот что получилось в итоге.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/rootme_jwt_5.png)

Сохраняем, перезагружаем страницу и флаг наш :)
# 
# 
### `JSON Web Token (JWT) - Weak secret`
![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/weak_1.png)

В этом задании нам предоставляется страница c "/hello", но надпись страницы всячески намекает, что нам нужно передлать это на "/token".

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/weak_2.png)

А вот и наш токен. Попробуем проделать с ним те же операции что с предыдущим заданием. И у нас нчиего не выйдет. А все потому, что нужно узнать секрет. Задача уже труднее, но мы можем воспользоваться тулзой, как: jwtcrack, jwt_tool или даже John The Ripper.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/weak_3.png)

Я использую jwt_tool (на Github нормальная документация). На скрине видно уже конечную команду. Секрет подобрался методом словарного перебора.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/weak_4.png)

Теперь, давайте быстро сделаем токен через консоль Python. Не хитрыми командами нам удается его создать. Передйем к следующему шагу.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/weak_5.png)

Остался последний шаг, это отправить запрос с токеном. Используем любимый BurpSuite. И не забываем что тут POST запрос.

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/weak_6.png)

Добавляем наш токен в заголовок "Authorization". Теперь мы видим ответ, так что бежим заливать флаг

![перехваченый запрос](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/weak_7.png)

# 
# 
##
## 
##
##
##
##
##
##
##
##
## Networks категория
#### ___Описание заданий с категорий___:

Следующий набор проблем касается сетевого трафика, включая различные протоколы. Вам необходимо проанализировать захваты пакетов для решения этих проблем.
Предпосылки:
Знание инструмента анализа сетевых захватов.
Знание наиболее распространенных сетевых протоколов.
# 
# 
### `FTP - authentication`
Мы работаем с файлов .pcap, а это означате что мы можем открыть его в Wireshark. По условию нам нужно работать с FTP и найти пароль.
![скрин С ftp пакетом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/wire1.png)
Так что находим самый первый пакет FTP и наживаем TCP-stream. А теперь ищем место с паролем.
![скрин с ответом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/wire1-2.png)
# 
#
### `TELNET - authentication`
Здесь делаем практически такой же алгоритм, что и в предыдущем задании (FTP - authentication). 
![скрин с ответом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/wire1-2.png)
# 
# 
### `ETHERNET - frame`
Нам дается .txt файл и открыв его, мы видим то, что это hex'ы. Поэтому открываем любой декодер, в моем случае это CyberShef. И переводим hex'ы в текст.
![скрин с ответом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/network_cyber_1.png)
Теперь мы видим кодировку base64, и опять же, просто переводим в обычный текст. Заливаем ответ.
#
#
### `Twitter authentication`
Здесь нам дается файл расширения .pcap, а значит интуитивно заходим в Wireshark. И видим всго один пакет.
![скрин с ответом](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/network_wire_2.png)

Выбираем "follow TCP" и нам открывается такое окно. Как мы видим, у запроса есть поле авторизации. Копируем текст оттуда и переводим base64 в текст.
#
#
### `Bluetooth - Unknown file`
В описании дается понять, что мы имеем дело с файлом Bluetooth. Воспользовавшись командой "cat", мы можем посмотреть то, что находится внутри файла и увидим расширение BTSnoop. Теперь мы можем воспользоваться Wireshark, так как он поддерживает работу с файлами связанными с Bluetooth. Теперь заходим во вкладку "Wireless" и переходим в "Bluetooth devices".

![что нам выдает файл](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/device.png)

А теперь просто переводим в sha1, но ___НЕ ЗАБЫВАЕМ ПЕРЕВЕСТИ В ВЕРХНИЙ РЕГИСТР БУКВЫ MAC АДРЕССА___.
#
#
### `CISCO - password`
В данном задании мы получаем txt'шник, в котором видим ряд различных данных. Но давайте не забывать про название таска ведь оно всегда дает нам хинт. Обратимся к txt повнимательней. Видим пароли, вот с ними нам и надо работать.

![что нам выдает файл](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/cisco_1.png)

Пользуемся сайтом со взломом.

![что нам выдает файл](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/cisco_2.png)

![что нам выдает файл](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/cisco_3.png)

![что нам выдает файл](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/cisco_4.png)

Видим анологию и остается только вставить сделать кое-что похожее. 6sK0_ оставляем и добавляем слово "Enable". К сожалению пришлось рассказать сам флаг.
#
#
### `DNS - zone transfert`
Здесь мы работаем с DNS, а если быть точнее, нам дается: хост, порт и домен. Значит в голову приходит команда dig.

![результат команды dig](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/networks_dns.png)

Теперь осталось создать нормальную команду, чтобы извлечь то, что нам нужно. Поэтому предлагаю использовать axfr, которая даст нам расширенную информацию о запрашиваемом домене. Осталось только забрать флаг.
#
#
### `IP - Time To Live`
И так, название снова дает нам понять, с чем мы будем работать. И воспользуемся утилитой Wireshark для анализа пакета.

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/ttl_1.png)

Тут довольно много пакетов и наша цель найти TTL. И полистав наши пакеты, можно заметить, что response находится только после TTL=13. Собственно это и будет ответом. К сожалению пришлось рассказать сам флаг.
#
#
### `SIP - authentication`
Максимально легкое задание. Здесь нам нужно посмотреть на конец строк. И там будет либо пароль. Как в первой строке. Либо хэш. Наша цель - это пароль, значит копируем цифры и вообщем-то все. На самом деле я дольше сидела над этим заданием, так как мне казалось странным такое быстрое решение

![фото страницы](https://github.com/YanaGerasimenko/ctf-writeups/blob/main/rootme/pics/sip_1.png)
# 
# 
##
## 
##
##
##
##
##
##
##
##
## Forensic категория
#### ___Описание заданий с категорий___:

Задача компьютерной криминалистики состоит в том, чтобы научить вас методикам, приемам и инструментам, связанным с цифровыми исследованиями. Эта наука заключается в сборе доказательств для понимания хода действий злоумышленника на компьютере или в информационной системе.
# 
# 
### `Command & Control - level 2`
