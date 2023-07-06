# Offensive Security: Moneybox

Используем Nmap для сканирования машины:
```sh
nmap -sC -sV 192.168.243.230
```
![ScreenShot](screenshots/1.png)

Мы нашли:
- 21 port - FTP (vsftpd 3.0.3)
- 22 port - SSH (OpenSSH 7.9p1)
- 80 port - HTTP (Apache httpd 2.4.38)

Первым делом, перейдем на FTP-сервер, т.к. возможен вход через **anonymous**:

![ScreenShot](screenshots/2.png)

Находим файл **trytofind.jpg**, который скачиваем себе на хостовую машину:

![ScreenShot](screenshots/3.png)

Смотрим для начала этот файл через стандартные для стеганографии инструменты:

![ScreenShot](screenshots/4.png)

![ScreenShot](screenshots/5.png)

![ScreenShot](screenshots/6.png)

Пока тщетно, но проверить стоило. Теперь перейдем на сайт:

![ScreenShot](screenshots/7.png)

Не найдя ничего интересного на данной странице, переходим к сканированию директорий:
```sh
gobuster dir -u http://192.168.243.230 -w /usr/share/wordlists/dirb/common.txt
```

![ScreenShot](screenshots/8.png)

Находим директорию **/blogs**:

![ScreenShot](screenshots/9.png)

Перейдя к найденной директории и проверив исходный код старницы, обнаруживаем подсказку - секретная директория - **S3cre3t-T3xt**

![ScreenShot](screenshots/10.png)

Уже внутри самой секретной директории находим ключ - **3xtr4ctd4t4**. Попробуем применить данный ключ в **steghide**:

![ScreenShot](screenshots/11.png)

Нам удалось извлечь данные, которые были записаны в **data.txt**:

![ScreenShot](screenshots/12.png)

В сообщении говорится, что у пользователя **renu** установлен слабый пароль. Попробуем найти нужный пароль при помощи **hydra**:
```sh
hydra -l renu -P /usr/share/wordlists/rockyou.txt 192.168.243.230 ssh
```

![ScreenShot](screenshots/13.png)

Найденный пароль - **987654321**

### Question 1: User flag?

Подключаемся по найденным данным по SSH:

![ScreenShot](screenshots/14.png)

Забираем первый флаг:

![ScreenShot](screenshots/15.png)

### Question 2: Root flag?

Сразу проверим, что мы можем запускать от имени sudo:

![ScreenShot](screenshots/16.png)

Пользователь **renu** не может использовать sudo. Найти что-то интересное в crontab, а также SUID-файлы не удалось:

![ScreenShot](screenshots/17.png)

![ScreenShot](screenshots/18.png)

Проверим, есть ли в системе еще пользователи:

![ScreenShot](screenshots/19.png)

Обнаруживаем пользователя **lily**, а в директории **/.ssh** пользователя **renu** находим приватный RSA-ключ на подключение:

![ScreenShot](screenshots/20.png)

![ScreenShot](screenshots/21.png)

Посмотрим sudo-права у пользователя **lily**:

![ScreenShot](screenshots/22.png)

В нашем владении доступ до perl. На GTFORBins находим способ повышения привилегий до root-пользователя:

![ScreenShot](screenshots/23.png)

Применяем команду и забираем второй флаг:

![ScreenShot](screenshots/24.png)

![ScreenShot](screenshots/25.png)
