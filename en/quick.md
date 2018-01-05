Quick Start
===========

- [Installation](#installation)
- [Configure Web-server](#configure-web-server)
  - [Apache](#apache)
  - [Nginx](#nginx)
- [Minimal Configuration](#minimal-configuration)

Installation
------------

You can install Colibri with the popular package manager [composer](https://getcomposer.org/):
    
    composer create-project colibri/application <your-project-name> --prefer-dist -s dev


Configure Web-server
--------------------

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


Minimal Configuration
---------------------

Create folder `local` inside `application/configs/` and copy into this folder
contents of `application/configs/local.example` folder. That's all!

Oh yes! You need to change setting `domain` in `application.php` (in this folders: `application/configs/`, `application/configs/local/`, & `application/configs/local.example/`)

> You can go through the settings in config files and fix what you need.
> 
> For more information about using configs and about the developer's local configs see in [Config](/en/config.md)


