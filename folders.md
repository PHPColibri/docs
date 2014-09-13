Структура каталогов
===================

- [Верхний уровень](#Верхний уровень)
- [Application](#Application)

Верхний уровень
---------------

Здесь всё просто:  
- всё, что относится к приложению находится в папке `application`,
- все сторонние библиотеки и компоненты, включая Colibri, - в `vendor`;
- также здесь находится `composer.json`, в котором вы можете прописывать свои зависимости
  для подключаемых библиотек.

> Папка `vendor` исключена из коммитов в файле `.gitignore`. Если вы используете другую
> систему контроля версий, добавьте эту папку в исключения.  
> `user@svn$ svn propset svn:ignore vendor .`  
> `user@hg$ echo vendor >> .hgignore`

Application
-----------

- `classes`
- `configs`
- `logs`
- `modules`
- `public`
- `templates`

### `classes`

### `configs`

### `logs`

### `modules`

### `public`

### `templates`

