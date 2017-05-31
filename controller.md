Контроллер
==========

- [Наименование](#Наименование)
- [Настройка](#Настройка)
- [Контроллер Представления](#Контроллер-Представления)
  - [Методы](#Методы)
  - [Параметры метода](#Параметры-метода)
  - [Параметры запроса](#Параметры-запроса)


Наименование
------------

Имя файла и класса:
- `ModuleName`[`DivisionName`]`Type`Controller.php

```php
// url : /blogs | /blogs/<method>
// file: application/modules/blogs/primary/BlogsViewsController.php
class BlogsViewsController extends ViewsController
{
}
```
```php
// url : /admin/blogs | /admin/blogs/<method>
// file: application/modules/blogs/admin/BlogsAdminViewsController.php
class BlogsViewsController extends ViewsController
{
}
```

Настройка
---------

Если в url запроса не указан *модуль*, то Colibri вызовет модуль `main`.  
Также если не указан *action*, то будет вызван `defaultView` для контроллера представлений
и `defaultResponse` для контроллера удалённых вызовов.

Если вы хотите это помянять, то в файле настроек `application.php` поправьте группу настроек
`module`.


Контроллер Представления
------------------------

#### Методы

Каждый метод контроллера представляет собой обработчик action-а.  
Чтобы "проконтроллировать" запрос на `http://example.ru/blogs/list` создайте
метод `list` в классе `BlogsViewsController`
```php
// file: application/modules/blogs/admin/BlogsAdminViewsController.php
class BlogsViewsController extends ViewsController
{
    public function list()
    {
        // ...
    }
}
```

#### Параметры метода

Все сегменты url-а, следующие за названием метода (action-а) до вопроса, Colibri
использует как параметры метода-action-а. Чтобы получить эти значения просто
объявите параметры в вашем методе.

Например, для `/blogs/show/44`:
```php
    public function show($id)
    {
    }
```

#### Параметры запроса

Все `GET` параметры, cледующие после вопроса в url, продолжают оставаться `GET` и параметрами
и вы можете получить их привычным для вас способом.

Для запроса `/blogs/edit/44?title=Отредактированный+заголовок`:
```php
    public function edit($id)
    {
        $title = $_GET['title'];

        // $id передался параметром
	//Blog::getById($id)->save([
	//    'title' => $title
	//]);
    }
```

