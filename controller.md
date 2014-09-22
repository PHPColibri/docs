Контроллер
==========

- [Наименование](#Наименование)
- [Настройка](#Настройка)
- [Контроллер Представления](#Контроллер Представления)
  - [Методы](#Методы)
  - [Параметры метода](#Параметры метода)
  - [Параметры запроса](#Параметры запроса)
  - [Шаблон и Layout](#Шаблон и Layout)
- [Контроллер удалённого вызова](#Контроллер удалённого вызова)

Контроллеры в Colibri делятся на 2 типа:
- Контроллеры представлений (ViewsController)
- Контроллеры удалённого вызова (MethodsController)

Наименование
------------

Имя файла:
- `moduleName`[`DivisionName`]`Type`.php

Имя класса:  
- С`ModuleName`[`DivisionName`]`Type`.php

```php
// url : /blogs | /blogs/<method>
// file: application/modules/blogs/blogsViews.php
class CBlogsViews extends ViewsController
{
}
```
```php
// url : /admin/blogs | /admin/blogs/<method>
// file: application/modules/blogs/admin/blogsAdminViews.php
class CBlogsViews extends ViewsController
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
метод `list` в классе `CBlogsViews`
```php
// file: application/modules/blogs/admin/blogsAdminViews.php
class CBlogsViews extends ViewsController
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
        $title = (int)$_GET['title'];
        // или:
        $title = filter_input(INPUT_GET, 'title', FILTER_VALIDATE_INT);
        
        // $id передался параметром
		//CBlog::getById(API::$db, $id)->save([
		//    'title' => $title
		//]);
    }
```

#### Шаблон и Layout
```php
    $this->backboneTplName
```
```php
    $this->template
```
```php
    $this->useTemplate
```
```php
    $this->useBackbone
```

Контроллер удалённого вызова
----------------------------
