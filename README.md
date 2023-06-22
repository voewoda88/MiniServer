# Мини-сервер на Си

Задача ‒ разработка многопоточного сервера и клиента, работающих по простому протоколу.

Изучаемые системные вызовы: socket(), bind(), listen(), conect(), accept() и прочих, связанных с адресацией в домене AF_INET.

Протокол должен содержать следующие запросы:

**ECHO** ‒ эхо-запрос, возвращающий без изменений полученное от клиента;
**QUIT** ‒ запрос на завершение сеанса;
**INFO** ‒ запрос на получения общей информации о сервере;
**CD** ‒ изменить текущий каталог на сервере;
**LIST** ‒ вернуть список файловых объектов из текущего каталога.

Протокол может содержать дополнительные запросы по выбору студента, не выходящие за пределы корневого каталога сервера и не изменяющих файловую систему в его дереве. Запросы клиенту отправляются на stdin. Ответы сервера и ошибки протокола выводятся на stdout. Ошибки системы выводятся на stderr. Подсказка клиента для ввода запросов ‒ символ '>'. Клиент помимо интерактивных запросов принимает запросы из файла. Файл с запросами указывается с использованием префикса '@':
```
$ myclient server.domen
Вас приветсвует учебный сервер 'myserver'
> @file
> ECHO какой-то_текст
какой-то_текст
> LIST
dir1
dir2
file
> CD dir1
dir1> QUIT
BYE
$
```

**ECHO** ‒ эхо-запрос, возвращающий без изменений полученное от клиента.
```
> ECHO "произвольный текст"
произвольный текст
>
QUIT ‒ запрос на завершение сеанса
> QUIT
BYE
$
```

**INFO** ‒ запрос на получения общей информации о сервере.
Cервер отправляет текстовый файл с соответствующей информацией.
```
> INFO
Вас приветсвует учебный сервер 'myserver'>
```
Этот же файл сервер отправляет клиенту при установлении сеанса.

**LIST** ‒ вернуть список файловых объектов из текущего каталога.
Текущий каталог ‒ каталог в дереве каталогов сервера. Корневой каталог сервера устанавливается из командной строки при старте сервера.
```
> LIST
dir1/
dir2/
file1
file2 --> dir2/file2
file3 -->> dir1/file
>
```
Каталоги выводятся с суффиксом '/' поcле имени, файлы ‒ как есть, симлинки на регулярные файлы разрешаются через '-->', симлинки на симлинки разрешаются через '-->>'. Корневой каталог сервера пр выводе указывается префиксом '/' перед именем.

**CD** ‒ изменить текущий каталог на сервере. Выход за пределы дерева корневого каталога сервера запрещается, команда безмолвно игнорируется.
```
> CD dir2
dir2> LIST
file2
dir2> CD ../dir1
dir1> LIST
file --> /file1
dir1> CD ..
> CD ..
>
```
Соединения функционируют независимо, т.е. текущий каталог у каждого соединения свой.
