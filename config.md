Настройки
=========

- [Использование](#Использование)
- [Локальные настройки](#Локальные-настройки)

Использование
-------------

Настройки хранятся в папке `application/configs/`.
Для удобства разнесены по смыслу в разные файлы:

    application.php
    cache.php
    database.php
    ...

### Обращение к значению

В общем виде обращение выглядит так:
```php
$setting = Config::<config_name>('setting.name.dot.separated'[, <default_value>]);
```
Например для обращения к значению настройки `debug` из файла `application.php`:
```php
if (Config::application('debug'))  {
}
```
или для получения настройки `folder` из `log.php`:
```php
$logFolder = Config::log('folder');
```

Для получения вложенной настройки используется точка:
```php
$mysqlConnectionSettings = Config::database('connection.mysql'); // вернёт массив (как это прописано в database.php)
```
или ещё глубже:
```php
$mysqlPassword = Config::database('connection.mysql.password');
```

### Значение по умолчанию

Если какая-то настройка _не обязательна_, то для указания значения **по умолчанию** укажите его вторым параметром:
```php
$encoding = Config::application('encoding', 'utf-8');
```


Локальные настройки
-------------------

Часто требуется установить свои настройки для локальной машины разработчика или для `production` сервера.
Для этого создайте папку `local/` внутри `application/configs/` (или скопируйте её из папки `local.example`)
и расположите в ней те файлы, которые вы хотите кастомизировать.

Указывать в этих файлах стоит только те настройки, которые нужно переопределить.

```php
// /application/configs/local/application.php

return [
    'debug' => true,
    'domain' => 'my-domain.local',
];
```
Остальные настройки будут использоваться из основного файла.

> Храните папку `local.example` (и поддерживайте в актуальном состоянии) в вашей VCS (системе контроля версий)
> как подсказку для вас и вашей команды какие настройки следует указать для работы проекта на локальной машине.
> Если вы добавляете какие-то настройки, которые будут специфичны для каждого разработчика, то укажите их в
> `local.example` и ваша команда будет вам благодарна.

