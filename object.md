DBObjects
=========

- [Создание](#Создание)
- [Функционал](#Функционал)

Создание
--------

- Унаследовать от CObject. (`extends CObject`)
- Задать название таблицы. (`protected $tableName='table_name';`)
- Указать первичный ключ. (`protected $PKFieldName=array('primary_key_name_1', 'primary_key_name_2', ...);`)
- Преречислить поля.
- Указать коллеции.

    class CUser extends CObject
    {
        protected	$tableName	='pfx_users';
        protected	$PKFieldName=array('id');
        
        public	$id;
        public	$login;
        public	$password;
        public	$email;

        protected	$collections=array('users'=>array('CGameUsersCollection',null));
    }


Функционал
----------

