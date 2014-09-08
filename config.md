Настройки
=========

Настройки хранятся в папке `application/configs`.
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

Если какая-то настройка не обязательна, то для указания значения по умолчание укажите его вторым параметром:
```php
$encoding = Config::application('encoding', 'utf-8');
```
