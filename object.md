DBObjects
=========

- [Создание](#Создание)
- [Функционал](#Функционал)

Создание
--------

- Унаследовать от CObject (`extends CObject`)
- Задать название таблицы (`protected $tableName='table_name';`)
- Указать первичный ключ (`protected $PKFieldName=array('primary_key_name_1', 'primary_key_name_2', ...);`)
- Преречислить поля
- Указать коллеции (подробнее)

Пример:

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


Функционал
----------

- create()
- load()
- save()
- delete()

### Create()

#### Сохраняем все поля, даже если не были заполнены:

	$user=new CUser($your_db);
	$user->login    =    'my_login';
	$user->password =md5('my_password');
	$user->email    =    'user.mail@example.com';

	$user->create(); // при неудаче бросает исключение
	
	echo('inserted id: '.$user->id);

Если в базе есть обязательное поле (например birthday), которое не было заполнено `$user->birthday = new DateTime();`,
будет выбрашоно исключение.

#### Сохраняем только часть полей

Если есть НЕобязательные поля, можно только обязазательные:

	$user=new CUser($your_db);
	$user->create( array('login'=>'my_login', 'password'=>md5('my_password')) );

#### Id вставленной записи

После создания записи в базе посредством вызова метода `CObject::crete()`
в поле первичного ключа, которое было указано в `protected $PKFieldName` появляется `id` вставленной записи

	// ...
	...->create();
	echo('inserted id: '.$user->id);

#### TODO:
- Есть ли observers/listeners/subscribers ?
- Заполнение из массива аля ->from_array($array_of values) ?
