Colibri "с полпинка" или Quick Start.
======================================

- [Установка](#Установка)
- [Настройка Веб-сервера](#Настройка-Веб-сервера)
  - [Apache](#apache)
  - [Nginx](#nginx)
- [Минимальная настройка](#Минимальная-настройка)

Установка
---------

Установить Colibri можно с помощью популярного менеджера пакетов [composer](https://getcomposer.org/):
    
    composer create-project colibri/application <your-project-name> --prefer-dist


Настройка Веб-сервера
---------------------

### Apache

```
<VirtualHost *:80>
	ServerName  example.ru
	ServerAlias www.example.dev
	#ServerAdmin admin-mail@yandex.ru

	DocumentRoot /home/<user>/projects/<project-name>/application/public
	ErrorLog     /home/<user>/projects/<project-name>/application/logs/error.log
	CustomLog    /home/<user>/projects/<project-name>/application/logs/access.log common

	#AddDefaultCharset UTF-8
	#AddCharset utf-8 .js

	Alias /modules   /home/<user>/projects/<project-name>/application/modules

	<Directory / >
		Allow from all
		Require all granted
		AllowOverride All
		Options -Indexes +FollowSymLinks
	</Directory>
</VirtualHost>
```

### Nginx

```
    server {
        listen            80;
        server_name       example.dev;
        root              /home/<user>/projects/<project-name>/application/public;  # whatever is yours
        index             index.php index.html index.htm;

        client_body_buffer_size 10m;

        #error_log /var/log/nginx/<project-name>.error.log notice;
        error_log /home/<user>/projects/<project-name>/application/logs/error.log;
        location / {
            try_files   $uri $uri/ @handler;
        }
 
        location ~ ^/modules.*(png|jpg|jpeg|gif|css|js)$ {
                root /home/<user>/projects/<project-name>/application;
        }

        location @handler {
            rewrite ^ /index.php?$request_uri;
        }
 
        location  = /index.php {
            fastcgi_pass        unix:/var/run/php5-fpm.sock;
            fastcgi_index       index.php;
            fastcgi_param       SCRIPT_FILENAME $document_root/index.php;
            include             fastcgi_params;
        }
    }
```


Минимальная настройка
---------------------

Создайте папку `local` внутри `application/configs/` и скопируйте в неё содержимое
папки `application/configs/local.example`. Всё!

Ах, да! Нужно поменять только опцию `domain` в `application.php` (в обоих файлах - и в `application/configs/`, и в `application/configs/local/`, и в `application/configs/local.example/`)

> Вы можете пройтись по настройкам и поправить то, что вам требуется.
> 
> Подробнее о использовании настроек и о локальных настройках разработчика см. [Настройки](/config.md)


