Коллекции
=========

- [Intro](#Intro)
- [Создание](#Создание)
  - [Глобальой коллекции](#Глобальой-коллекции)
  - [Один ко многим](#Один-ко-многим)
  - [Многие ко многим](#Многие-ко-многим)


Intro
-----

Коллекция - представление набора записей в таблице БД в удобном для использования виде,
а именно - в виде массива объектов `Object`.

Класс `ObjectCollection` реализует интерфейсы `ArrayAccess`, `Iterator`, `Countable`, -
т.е. можно использовать в циклах `foreach`, обращаться к элементам как в массиве (`$coll[$i]`),
применять ф-цию `count()`.

Коллекция реализует механизм `lazy loading`. Таким образом, элементы коллекции будут подгружаться только при
первом обращении к ней.

Также коллекции могут обеспечивать связи между объектами БД и деляьтся на два типа:
- Один ко многим: `ObjectSingleCollection`
- Многие ко многим: `ObjectMultiCollection`

Точнее говоря, у коллекции может быть предопределена фильтрация по внешнему ключу
(или через таблицу связей многие ко многим),
что в дальнейшем может использоваться в качестве связи в объектах.



Создание
--------

- Унаследоваться от `ObjectSingleCollection` или `ObjectMultiCollection`
- Задать имя таблицы, с которой работаем (`protected $tableName`)
- Задать имя класса объектов, хранящихся в коллекции (`protected $itemClass`)
- Optional: Указать предустановленную фильтрацию по внешнему ключу (`protected $FKName`)
- Optional: Указать таблицу связей, если используется предустановленная фильтрация по FK многие ко многим (`protected $fkTableName`)


### Глобальой коллекции

Глобальная коллекция является общим случаем и предназначена для работы со всей таблицей.

Для создания требуется:
- Унаследоваться от `ObjectSingleCollection`
- Задать имя таблицы
- Задать имя класса элементов

Пример:
```php
	class PostsCollection extends ObjectSingleCollection
	{
		protected	static $tableName = 'prfx_posts';
		protected	$itemClass	  = 'CPost';
		protected	$FKName		  = array(null,null);
	}
```

### Один ко многим

Создание с предустановленной фильтрацией по внешнему ключу при связи *один ко многим*:
- Унаследоваться от `ObjectSingleCollection`
- также имя таблицы
- имя класса
- Указать поле внешнего ключа как первый элемент массива `protected $FKName`


Например, коллекция постов пользователя:
```php
	class UserPostsCollection extends ObjectSingleCollection
	{
		protected	static $tableName = 'prfx_posts';
		protected	$itemClass	  = 'Post';
		protected	$FKName		  = ['user_id',null];
	}
```

### Многие ко многим

Создание с предустановленной фильтрацией через таблицу связей *многие ко многим*:
- от `ObjectMultiCollection`
- имя таблицы
- имя класса
- имя таблицы связей (`protected $fkTableName`)
- массив с двумя элементами - именами столбцов *в таблице связей*:
  - [0]: столбец, ссылающийся на обект-"родитель"
  - [1]: столбец, ссылающийся на выбираемый из БД объект

Например, коллекция ролей пользователя:
```php
	class CUserRolesCollection extends ObjectMultiCollection
	{
		protected	static $tableName   = 'prfx_roles';
		protected	$itemClass	    = 'Role';
		protected	$fkTableName	    = 'prfx_user_roles';
		protected	$FKName		    = array('user_id','role_id');
	}
```
