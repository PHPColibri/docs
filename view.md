Представление
=============

- [Именование](#Именование)
  - [Каркас / Layout](#Каркас)
  - [Шаблон](#Шаблон)
- [Переменные](#Переменные)
  - [Передать из контроллера](#Передать-из-контроллера)
  - [Использовать в шаблоне](#Использовать-в-шаблоне)

Каркас / Layout
---------------
application/templates/`layout[.<division>].php`

Шаблон
------
application/modules[/\<division\>]/`templates/module[<Division>]Views.php`

Работа с шаблонами
------------------
### Каркас
```php
Layout::title('Acme: О холдинге');
```
```php
Layout::description('Acme -  вымышленная торговая марка, фигурирующая во вселенной мультфильмов и др. Название расшифровывается как «A Company that Makes Everything», то есть «Фирма, делающая всё, что угодно».');
```
```php
Layout::keywords('acme холодильники микрочипы туалетная бумага всё что угодно');
```
```php
Layout::addCss('prettify.css');
```
```php
Layout::delCss('prettify.css');
```
```php
Layout::addJs('prettify.js');
```
```php
Layout::addJsText('var userLogin = ' . $user->login . ';');
```
```php
```
### Шаблон страницы
#### Передать из контроллера
```php
public function show()
{
    $this->template->vars['pageTitle'] = 'Добро пожаловать!';
}
```

#### Использовать в шаблоне
```php
<h1><?= $pageTitle ?></h1>
```

