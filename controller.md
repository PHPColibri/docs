Контроллер
==========


Контроллеры в Colibri делятся на 2 типа:
- Контроллеры представлений (ViewsController)
- Контроллеры удалённого вызова (MethodsController)

Наименование контроллера
------------------------

Имя файла:
    <moduleName>[<DivisionName>]<Type>.php
Имя класса:
    С<ModuleName>[<DivisionName>]<Type>.php

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
Все сегменты url-а, следующие за названием метода (action-а) до вопроса (`/blogs/show/*44*`), Colibri
использует как параметры метода-action-а. Чтобы получить эти значения просто
объявите параметры в вашем методе:
```php
    public function show($id)
    {
    }
```


Контроллер Представления
------------------------