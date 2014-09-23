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

Правила
-------
