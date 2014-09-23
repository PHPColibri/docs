Utils
=====

- [String](#String)
- [RegExp](#RegExp)
- [Arr](#Arr)
- [File](#File)
- [Image](#Image)


String
------

####
```php
    String::isInt()
```

####
```php
    String::isEmail()
```

####
```php
    String::isJSON()
```

####
```php
    String::random()
```

####
```php
    String::generateGUID()
```

####
```php
    String::beginsWith()
```

####
```php
    String::contains()
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
