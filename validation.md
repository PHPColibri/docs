Валидация
=========


Типовое использование
---------------------

```php
$post = new Validation($_POST);
$post
	->required('blog_id')
	->required('title')
	->required('body')
	->isIntGt0 ('blog_id')
	->maxLength('title', 128)
	->maxLength('body', 65536)
	->is('CBlogMessage::preambleLenValid', 'body', 'преамбула не должна превышать 1200 символов')
;
if (!$post->valid())
```

Набор проверяемых данных
------------------------
Первоначально вы можете задать набор проверяемых данных в конструкторе:
```php
$validator = new Validation($dataToValidate);
```
или можете опустить при создании и задать данные позже:
```php
$validator = new Validation();
$validator->setScope($dataToValidate);
```
Используя метод `setScope()` вы можете использовать валидатор дважды:
```php
$get = new Validation($_GET);

// проверим $_GET
$get
	->required('id')
	->validate()

// проверим $_POST
->setScope($_POST)
	->required('title')
	->required('body')
	->maxLength('title', 128)
	->maxLength('body', 65536)
	->validate();
```
Если вы хотите проверить несколько наборов сразу:
```php
$input = new Validation($_GET);
$input->extendScope($_POST);
```

Проверка
--------
Если вы привыкли работать с исключениями, то просто вызовите метод `validate()`,
который в случае не вылидных данных выбросит `\Colibri\Validation\Exception`.
Это исключение содержит все произошедшие ошибки, которые можно получить с помощью
метода `getErrors()`.
```php
use Colibri\Validation\Exception as ValidationException;
try {
	$scope = Validation([...]);
	$scope
		->requred(...)
		...
	
		->validate(); // this method throws exception !!!
		
} catch (ValidationException $e) {
	var_dump($e->getErrors());
}
```
или
```php
$scope
	->req...
	...
	
	->ifNotValid(function(array $errors) {
		... // redirect for example
	})
```
или
```php
if ($scope->valid()) {
	
}
```

Правила
-------

#### required

#### minLength

#### maxLength

#### regex

#### isIntGt0

#### isJSON

#### isEmail

#### isEqual

#### is / isNot

