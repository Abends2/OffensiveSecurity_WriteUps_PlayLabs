# Offensive Security: CyberSploit1

Используем Nmap для сканирования машины:
```sh
nmap -sC -sV 192.168.181.92
```
![ScreenShot](screenshots/1.png)

Мы нашли:
- 22 port - SSH (OpenSSH 5.9p1)
- 80 port - HTTP (Apache httpd 2.2.22)

Первым делом, перейдем на сайт:

![ScreenShot](screenshots/2.png)

В исходном коде страницы находим **username:itsskv**:

![ScreenShot](screenshots/3.png)

Запустим процесс сканирования директорий сайта:

![ScreenShot](screenshots/4.png)

Перейдем по пути **/robots**:

![ScreenShot](screenshots/5.png)

Тут находится строка в формате base64, сразу же декодируем ее:

![ScreenShot](screenshots/6.png)

Тут пришлось долго помучиться... В итоге приходим к тому, что полученная ссылка и есть пароль от SSH:

![ScreenShot](screenshots/7.png)

**SSH User: itsskv**

**SSH Password: cybersploit{youtube.com/c/cybersploit}**

### Question 1: User flag?

Получаем первый флаг:

![ScreenShot](screenshots/8.png)

### Question 2: Root flag?

Проверим, sudo-привилегии пользователя **itsskv**:

![ScreenShot](screenshots/9.png)

Мы не можем использовать sudo. Проверим, есть ли в системе директории других пользователей:

![ScreenShot](screenshots/10.png)

Обнаруживаем директорию пользователя **cybersploit**, но эта находка не привела к какому-либо результату.

Далее пришлось долго копаться, к тому же воспользоваться подсказкой... Проверим версию ядра системы:

![ScreenShot](screenshots/11.png)

Ищем эксплойты для данной версии ядра (3.13.0):

![ScreenShot](screenshots/12.png)

![ScreenShot](screenshots/13.png)

Качаем найденный эксплойт, который позволяет повысить привилегии до root'а в системе. Затем открываем http-сервер средствами Python:

![ScreenShot](screenshots/14.png)

Скачиваем файл через **wget** на хост-жертву:

![ScreenShot](screenshots/15.png)

Назначаем права на выполнение скачанного файла:

![ScreenShot](screenshots/16.png)

Проверяем наличие gcc:

![ScreenShot](screenshots/17.png)

Компилируем:

![ScreenShot](screenshots/18.png)

Запускаем скомпилированный эксплойт и получаем root'а:

![ScreenShot](screenshots/19.png)

Забираем последний флаг:

![ScreenShot](screenshots/20.png)
