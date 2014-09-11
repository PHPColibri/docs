Colibri "с пол пинка" или Quick Start.
======================================

- [Установка](#Установка)
- [Настройка Веб-сервера](#Настройка Веб-сервера)
  - [Apache](#Apache)
  - [Nginx](#Nginx)


Установка
---------

Установить Colibri можно с помощью популярного менеджера пакетов [composer](https://getcomposer.org/):
    
    composer create-project colibri/application -s dev

> На данный момент нет стабильного релиза, поэтому `-s dev`

Настройка Веб-сервера
---------------------

### Apache

```
<VirtualHost *:80>
	ServerName  example.ru
	ServerAlias www.example.dev
	#ServerAdmin admin-mail@yandex.ru

	DocumentRoot /home/<user>/projects/project-name/application/public
	ErrorLog     /home/<user>/projects/project-name/application/logs/error.log
	CustomLog    /home/<user>/projects/project-name/application/logs/access.log common
	#RewriteLog  /home/<user>/projects/project-name/application/logs/rewrite.log
	#RewriteLogLevel 0

	#AddDefaultCharset UTF-8
	#AddCharset utf-8 .js

	Alias /modules   /home/<user>/projects/<project-name>/application/modules

	<Directory / >
		Allow from all
		AllowOverride All
		Options Indexes FollowSymLinks
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
        error_log /home/<user>/projects/<project-name>/application/logs/error.log
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
