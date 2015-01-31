DBObjects
=========

- [Создание](#Создание)
- [Функционал](#Функционал)
  - [CObject::create()](#cobjectcreate)
  - [CObject::load()](#cobjectload)
  - [CObject::save()](#cobjectsave)
  - [CObject::delete()](#cobjectdelete)

Создание
--------

- Унаследовать от CObject (`extends Object`)
- Задать название таблицы (`protected static $tableName   = 'table_name';`)
- Указать первичный ключ  (`protected static $PKFieldName = ['primary_key_name_1', 'primary_key_name_2', ...];`)
- Преречислить поля
- Указать коллеции (подробнее)

Пример:
```php
	class User extends Object
	{
		protected static $tableName   = 'pfx_users';
		protected static $PKFieldName = array('id');
		
		public	$id;
		public	$login;
		public	$password;
		public	$email;
		
		protected	$collections=array('posts'=>array('CUserPostsCollection',null));
	}
```

Функционал
----------

- [Object::create()](#objectcreate)
- [Object::load()](#objectload)
- [Object::save()](#objectsave)
- [Object::delete()](#objectdelete)


### Object::create()

#### Сохраняем все поля, даже если не были заполнены:
```php
	$user=new User();
	$user->login    =     'my_login';
	$user->password = md5('my_password');
	$user->email    =     'user.mail@example.com';

	$user->create(); // при неудаче бросает исключение
	
	echo('inserted id: '.$user->id);
```
Если в базе есть обязательное поле (например birthday), которое не было заполнено `$user->birthday = new DateTime();`,
будет выброшено исключение.

#### Сохраняем только часть полей

Если есть НЕобязательные поля, можно сохранить только обязазательные:
```php
	$user = new User();
	$user->create( ['login'=>'my_login', 'password'=>md5('my_password')] );
```
#### Id вставленной записи

После создания записи в базе посредством вызова метода `Object::crete()`
в поле первичного ключа, которое было указано в `protected static $PKFieldName` появляется `id` вставленной записи
```php
	// ...
	...->create();
	echo('inserted id: '.$user->id);
```

### Object::load()

`Object::load` загружает запись из БД и записывает значения полей в свосвта Модели.
Выбрасывает исключение при ошибке.

#### Загрузка по id
```php
	$user = User::getById(44);
```
или
```php
	$user = new User();
	// ... ...
	$user->load(44);
```
#### Автоматическая загрузка при инициализации
```php
	$user_id = 44;
	$user = new User($user_id);
```
#### Загрузка по условию
```php
	$user = new User();
	$user->load( ['login'=>'admin','password'=>'admin'] );
```


### Object::save()

Эти примеры не загружают запись из БД ( _не делают_ `SELECT`),
а сразу делают запрос на изменение записи (`UPDATE`):

#### Сохраниение всех свойств Модели.
```php
	$user = new User();
	$user->login    =     'new login';
	$user->password = md5('new password');
	$user->email    =     'user.mail@example.com';

	$user->id = 44;
	$canSave = $user->save();
```
#### Сохранение выборочных полей
```php
	$user = new User(); // not loaded

	$user->id = 44;
	$canSave  = $user->save( ['password'=>md5('new password')] );
```


### Object::delete()
```php
	$user=new User();

	$user->id  = 44;
	$canDelete = $user->delete();
```

