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

- Унаследовать от CObject (`extends CObject`)
- Задать название таблицы (`protected $tableName='table_name';`)
- Указать первичный ключ (`protected $PKFieldName=array('primary_key_name_1', 'primary_key_name_2', ...);`)
- Преречислить поля
- Указать коллеции (подробнее)

Пример:
```php
	class CUser extends CObject
	{
		protected	$tableName	='pfx_users';
		protected	$PKFieldName=array('id');
		
		public	$id;
		public	$login;
		public	$password;
		public	$email;
		
		protected	$collections=array('posts'=>array('CUserPostsCollection',null));
	}
```

Функционал
----------

- [CObject::create()](#cobjectcreate)
- [CObject::load()](#cobjectload)
- [CObject::save()](#cobjectsave)
- [CObject::delete()](#cobjectdelete)


### CObject::create()

#### Сохраняем все поля, даже если не были заполнены:
```php
	$user=new CUser($your_db);
	$user->login    =    'my_login';
	$user->password =md5('my_password');
	$user->email    =    'user.mail@example.com';

	$user->create(); // при неудаче бросает исключение
	
	echo('inserted id: '.$user->id);
```
Если в базе есть обязательное поле (например birthday), которое не было заполнено `$user->birthday = new DateTime();`,
будет выброшено исключение.

#### Сохраняем только часть полей

Если есть НЕобязательные поля, можно сохранить только обязазательные:
```php
	$user=new CUser($your_db);
	$user->create( array('login'=>'my_login', 'password'=>md5('my_password')) );
```
#### Id вставленной записи

После создания записи в базе посредством вызова метода `CObject::crete()`
в поле первичного ключа, которое было указано в `protected $PKFieldName` появляется `id` вставленной записи
```php
	// ...
	...->create();
	echo('inserted id: '.$user->id);
```

### CObject::load()

`CObject::load` загружает запись из БД и записывает значения полей в свосвта Модели.
Выбрасывает исключение при ошибке.

#### Загрузка по id
```php
	$user = new CUser($your_db);
	$user->load(44);
```
#### Автоматическая загрузка при инициализации

	$user_id = 44;
	$user = new CUser($your_db, $user_id);

#### Загрузка по условию
```php
	$user = new CUser($your_db);
	$user->load( array('login'=>'admin','password'=>'admin') );
```


### CObject::save()

Эти примеры не загружают запись из БД ( _не делают_ `SELECT`),
а сразу делают запрос на изменение записи (`UPDATE`):

#### Сохраниение всех свойств Модели.
```php
	$user = new CUser($your_db);
	$user->login    =     'new login';
	$user->password = md5('new password');
	$user->email    =     'user.mail@example.com';

	$user->id=44;
	$canSave=$user->save();
```
#### Сохранение выборочных полей
```php
	$user = new CUser($your_db); // not loaded

	$user->id = 44;
	$canSave  = $user->save( array('password'=>md5('new password')) );
```


### CObject::delete()
```php
	$user=new CUser($your_db);

	$user->id=44;
	$canDelete=$user->delete();
```

