# Offensive Security: FunboxRookie

Для начала произведем сканирование целевого хоста при помощи Nmap:
```sh
nmap -sC -sV 192.168.181.107
```

![ScreenShot](screenshots/1.png)

Найдено:
- 21 port - FTP (ProFTPD 1.3.5e)
- 22 port - SSH (OpenSSH 7.6p1)
- 80 port - HTTP (Apache httpd 2.4.29)

Проверим сайт, который расположен на 80 порте:

![ScreenShot](screenshots/2.png)

Nmap показал наличие на сайте файла **robots.txt**:

![ScreenShot](screenshots/3.png)

Тут находим **/logs/**, но внутри ничего не обнаружено:

![ScreenShot](screenshots/4.png)

В таком случае смотрим в сторону FTP-сервера. Заходим под аккаунтом **anonymous**:

![ScreenShot](screenshots/5.png)

Ранее Nmap определил список файлов, которые находятся на сервере. Они все похожи, но только один из них - **tom.zip**, отличается от других выданными правами (**-rw-rw-r--**). Скачаем себе данный архив:

![ScreenShot](screenshots/6.png)

Архив имеет пароль, поэтому сначала получаем хэш, а затем брутим его и получаем пароль - **iubire**:

![ScreenShot](screenshots/7.png)

Внутри расположен приватный RSA-ключ для подключения по SSH:

![ScreenShot](screenshots/8.png)

Перемещаем ключ из архива (я поместил на рабочий стол) и выполняем подключение по SSH:
```sh
ssh -i Desktop/id_rsa tom@192.168.181.107
```

![ScreenShot](screenshots/9.png)

### Question 1: User flag?

Собственно, читаем первый флаг:

![ScreenShot](screenshots/10.png)

Далее, в домашнем каталоге пользователя **tom**, находим файл **.mysql_history**, где находим пароль от УЗ **tom** - **xx11yy22!**:

![ScreenShot](screenshots/11.png)

### Question 2: Root flag?

Проверяем, какие команды мы можем исполнять от лица sudo:

![ScreenShot](screenshots/12.png)

Мы можем выполнять любые команды... Просто вызываем bash с использованием sudo и получаем доступ к УЗ **root**. Остается только прочитать второй флаг:

![ScreenShot](screenshots/13.png)
