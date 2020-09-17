Model
=====

- [Введение](#Введение)
- [Создание](#Создание)
- [Функционал](#Функционал)
  - [Model::create()](#modelcreate)
  - [Model::load()](#modelload)
  - [Model::save()](#modelsave)
  - [Model::delete()](#Modeldelete)

Введение
--------

Объектом базы данных (DbObject) в Colibri называется Модель. Это не что иное как представление
одной записи из таблицы ввиде экземпляра класса, свойства которого являются поля из БД.

Создание
--------

- Унаследовать от Model (`extends Model`)
- Задать название таблицы (`protected static $tableName   = 'table_name';`)
- Указать первичный ключ  (`protected static $PKFieldName = ['primary_key_name_1', 'primary_key_name_2', ...];`)
- Преречислить поля из БД в виде свойств
- Указать коллеции (подробнее)

Пример:
```php
	class User extends Model
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

- [Model::create()](#modelcreate)
- [Model::load()](#modelload)
- [Model::save()](#modeltsave)
- [Model::delete()](#modeldelete)


### Model::create()

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
    $user->create(['login'=>'my_login', 'password'=>md5('my_password')]);
```
или
```php
    User::saveNew(['login'=>'my_login', 'password'=>md5('my_password')]);
```
#### Id вставленной записи

После создания записи в базе посредством вызова метода `Model::crete()`
в поле первичного ключа, которое было указано в `protected static $PKFieldName` появляется `id` вставленной записи
```php
	// ...
	...->create();
	echo('inserted id: '.$user->id);
```

### Model::load()

`Model::load` загружает запись из БД и записывает значения полей в свойства Модели.
Выбрасывает исключение при ошибке.

#### Загрузка по id
```php
    $user = User::getById(44);
```
если запись не найдена, выбрасывает `Colibri\Database\Exception\NotFoundException`

или
```php
    $user = new User();
    $user->load(44);
    // или:
    $user = (new User())->load(44);
```
`load()` возвращает сам объект или `null`, если запись не найдена.

То же самое проще:
```php
    $user = User::find(44);
```
`find()` возвращает сам объект или `null`, если запись не найдена.
`get()` возвращает сам объект или выбрасывает `Colibri\Database\Exception\NotFoundException`, если запись не найдена.

#### Автоматическая загрузка при инициализации
```php
    $user = new User(44);
```
#### Загрузка по условию
```php
    $user = new User();
    $user->load( ['login'=>'admin','password'=>'admin'] );
```
`load()` возвращает сам объект или `null`, если запись не найдена.

или
```php
    $user = User::find(['login'=>'admin','password'=>'admin']);
```
`find()` возвращает сам объект или `null`, если запись не найдена.

### Model::save()

Эти примеры не загружают запись из БД ( _не делают_ `SELECT`),
а сразу делают запрос на изменение записи (`UPDATE`):

#### Сохраниение всех свойств Модели
```php
    $user = new User();
    $user->login    =     'new login';
    $user->password = md5('new password');
    $user->email    =     'user.mail@example.com';

    $user->id = 44;
    $user->save();
```
#### Сохранение выборочных полей
```php
    $user = new User(); // not loaded

    $user->id = 44;
    $user->save( ['password'=>md5('new password')] );
```


### Model::delete()
Метод `delete()` принимает `id` или `array` с условием, либо вызывается без параметров и берёт `id` из PK-пол(я/ей)
#### Удаление записи по id
```php
    // запись из БД предварительно не загружается:
    $user=new User();

    $user->id  = 44;
    $user->delete();
```
или
```php
    (new User())->delete(44); // запись из БД предварительно не загружается
```
#### Удаление записи по условию
или
```php
    // запись из БД предварительно не загружается:
    (new Event())->delete([
        'type'   => 'system',
        'date <' => Carbon::now()->subMonth()
    ]);
```
