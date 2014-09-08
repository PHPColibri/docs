Настройки
=========

Настройки хранятся в папке `application/configs`.
Для удобства разнесены по смыслу в разные файлы:
    application.php
    cache.php
    database.php
    ...

Обращение к значению

    $setting = Config::<config_name>('setting.name.dot.separated'[, <default_value>]);
