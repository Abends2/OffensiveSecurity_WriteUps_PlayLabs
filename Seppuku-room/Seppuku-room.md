# Offensive Security: Seppuku

Для начала произведем сканирование целевого хоста при помощи Nmap:

```sh
nmap -sC -sV -p- 192.168.186.90
```

![ScreenShot](screenshots/1.png)

Найдено:
- 21 port - FTP (vsftpd 3.0.3)
- 22 port - SSH (OpenSSH 7.9p1)
- 80 port - HTTP (nginx 1.14.2)
- 7080 port - HTTP (LiteSpeed)
- 7601 port - HTTP (Apache httpd 2.4.38)
- 8088 port - HTTP (LiteSpeed httpd)

Осмотрим сайты:

![ScreenShot](screenshots/2.png)

![ScreenShot](screenshots/3.png)

![ScreenShot](screenshots/4.png)

Интересного сходу замечено не было, поэтому сканируем директории через `wfuzz`:

![ScreenShot](screenshots/5.png)

На веб-ресурсе, который расположен на порту 8088 находим `index.php`, где находится WebConsole, но пока, к сожалению, это нам ничего не дает, т.к. нет данных для авторизации:

![ScreenShot](screenshots/6.png)

Продолжаем искать директории:

![ScreenShot](screenshots/7.png)

А вот сайт на порту 7601 куда более интереснее. Посмотрим на `/secret` и `/keys`:

![ScreenShot](screenshots/8.png)

![ScreenShot](screenshots/9.png)

Находим файлы `/secret/password.lst` и `/secret/hostname`, а также `/keys/private`:

![ScreenShot](screenshots/10.png)

![ScreenShot](screenshots/11.png)

Логин и перечень паролей? Пробуем пробиться по SSH:

![ScreenShot](screenshots/12.png)

>seppuku:eeyoree

Подключаемся и забираем первый флаг:

![ScreenShot](screenshots/13.png)

Находим пароль и узнаем, что он от пользователя `samurai`:

![ScreenShot](screenshots/14.png)

![ScreenShot](screenshots/15.png)

Интересно, что мы не можем перемещаться по директориям. Тогда сразу заглянем в права sudo:

![ScreenShot](screenshots/16.png)

Интересная команда. В системе есть еще 1 пользователь - `tanto`. Копируем себе найденный приватный SSH-ключ и пробуем войти:

![ScreenShot](screenshots/17.png)

![ScreenShot](screenshots/18.png)

У нас успешно получилось войти. Т.к. у нас не работаем cd, воспользуемся опцией `-t "bash -noprofile"`. создаем файл `.cgi_bin/bin`, который открывает bash-оболочку:

![ScreenShot](screenshots/19.png)

Переходим обратно на `samurai` и открываем созданный файл:

![ScreenShot](screenshots/20.png)

---

