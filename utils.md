Utils
=====

- [String](#String)
- [RegExp](#RegExp)
- [Arr](#Arr)
- [File](#File)
- [Image](#Image)


String
------

#### `::isInt()`
Проверяет является ли строка целым числом.
```php
    Str::isInt('100')  // true
    Str::isInt('10.0') // false
```

#### `::isEmail()`
Проверяет является ли строка почтовым адресом.
```php
    Str::isEmail('a@a.ru')     // true
    Str::isEmail('@ya.com.ru') // false
```

#### `::isJSON()`
Проверяет является ли строка валидным JSON-ом.
```php
    Str::isJSON('{}') // true
```

#### `::random()`
Генерирует случайную строку.
```php
    Str::random()            // string( 8) "uHS57IDP"
    Str::random('alnum')     // string( 8) "uHS57IDP"
    Str::random('alnum', 16) // string(16) "V73WeufIbvcnsCRO"
    Str::random('numeric')   // string( 8) "19520780"
    Str::random('nonzero')   // string( 8) "19527787"
    Str::random('unique')    // string(32) "f6a195bd32d4a195b55eeed7acf4ce9f"
```

#### `::generateGUID()`
Генерирует GUID.
```php
    Str::generateGUID() // 6acd8c8f-6454-4295-acf7-998b96fe2c0b
```

#### `::beginsWith()`
Проверяет начинается ли строка с указанной.
```php
    Str::beginsWith('Hello World', 'Hell') // true
    Str::beginsWith('Hello World', 'hell') // false
```

#### `::endsWith()`
Проверяет заканчивается ли строка указанной.
```php
    Str::endsWith('Hello World', 'World') // true
    Str::endsWith('Hello World', 'world') // false
```

#### `::contains()`
Проверяет содержит ли строка указанную подстроку.
```php
    Str::contains('Hello World', 'lo Wo') // true
    Str::contains('Hello World', 'lo wo') // false
```


RegExp
------
```php
    RegExp::isDate;
    RegExp::isEmail;
```

Arr
---
```php
    Arr::overwrite($array, $with)
```
#### Через точку
```php
    Arr::get(array $array, $key, $default = null)
```

File
----
```php
    File::getMimeType()
```

Image
-----
```php
    Image::createThumbnail($path, $target_width=100, $target_height=100, $bgColor=0xdddddd)
```
