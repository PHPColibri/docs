Utils
=====

- [String](#string)
- [Arr](#arr)
- [File](#file)
- [Image](#image)


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

#### `::firstPart()`
Возвращает первую часть строки, разделяя её указанным символом (разелителем).  
По умолчанию разделитель - пробел, и хелпер возвращает первое слово.
```php
    Str::firstPart('apple mango pear kiwi')          // apple
    Str::firstPart('apple,mango,pear,kiwi', ',')     // apple
    Str::firstPart('apple, mango, pear, kiwi', ', ') // apple
```

#### `::lastPart()`
Аналогично `firstPart`. Возвращает последнюю часть строки.
```php
    Str::lastPart('apple mango pear kiwi')          // kiwi
    Str::lastPart('apple,mango,pear,kiwi', ',')     // kiwi
    Str::lastPart('apple, mango, pear, kiwi', ', ') // kiwi
```

#### `::snake()`
Конвертирует строку в "змеинный"(snaked) стиль.
```php
    Str::snake('camelCase')                // camel_case
    Str::snake('Some Title String')        // some_title_string
    Str::snake('spaced words string')      // spaced_words_string
    Str::snake('spaced words string', '!') // spaced!words!string
```

#### `::studly()`
Конвертирует строку в studly-стиль.
```php
    Str::studly('snake_case')    // SnakeCase
    Str::studly('camelCase')     // CamelCase
    Str::studly('slug-style')    // SlugStyle
    Str::studly('spaced string') // SpacedString
```

#### `::camel()`
Конвертирует строку в "верблюжий"(camel) стиль.
```php
    Str::camel('snake_case')    // SnakeCase
    Str::camel('camelCase')     // CamelCase
    Str::camel('slug-style')    // SlugStyle
    Str::camel('spaced string') // SpacedString
```

#### `::part()`
Возвращает `$i`-ую часть строки, раделяя её указанным разделителем `$delimiter`  
или возвращает указанное `$default` значение.
```php
    Str::part('hello world', 0, ' ')                  // 'hello'
    Str::part('some spaced string', 1, ' ')           // 'spaced'
    Str::part('snake_case', 0, '_')                   // 'snake'
    Str::part('use default default', 3, ' ')          // <null>
    Str::part('use my default', 3, ' ', 'my-default') // 'my-default'
```

#### `::word()`
Возвращает `$i`-ое слово. Работает также как `::part()` с пробелом (`' '`) в качестве разделителя. 
```php
    Str::word('hello world', 0)                  // 'hello'
    Str::word('some spaced string', 1)           // 'spaced'
    Str::word('use default default', 3)          // <null>
    Str::word('use my default', 3, 'my-default') // 'my-default'
```

#### `::hasDigits()`
Проверяет содержит ли строка цифры.
```php
    Str::('some string')   // false
    Str::('some string 2') // true
    Str::('some2string')   // true
```


Arr
---

#### `::overwrite()`
> ```php
> Arr::overwrite(array $array, array $with)
> ```

Рекурсивно перезаписывает значения массива.
```php
    $array = [
        'key1'   => 'value1',
        'key2'   => 'value2',
        'nested' => [
            'n1' => 123,
            'n2' => 'nested value 2',
        ],
    ];
    $with = [
        'key2'   => 'new value for key2',
        'nested' => [
            'n2' => 'new value for nested.n2',
        ],
    ];

    $result = Arr::overwrite($array, $with);

    // После чего в `$result` будет следующее:
    $result == [
        'key1' => 'value1',
        'key2' => 'new value for key2',
        'nested' => [
            'n1' => 123,
            'n2' => 'new value for nested.n2',
        ],
    ]
```

#### `::get()`
> ```php
> Arr::get(array $array, string $dottedKey, mixed $default = null)
> ```

Достаёт значение из массива, используя нотацию "через точку" (вложенные ключи записываются через точку).  
Если ключ отсутствует, возвращает `$default` значение.
```php
    $array = [
        'key1'   => 'value1',
        'key2'   => 'value2',
        'nested' => [
            'n1' => 123,
            'n2' => 'nested value 2',
        ],
    ];

    Arr::get($array, 'key2')       // 'value2'
    Arr::get($array, 'key3')       // null
    Arr::get($array, 'nested.n1')  // 123
    Arr::get($array, 'nested.n2')  // 'nested value 2'
```


#### `::set()`
> ```php
> Arr::set(array $array, string $dottedKey, mixed $value)
> ```

Задаёт значение элемента массива, используя нотацию "через точку" (вложенные ключи записываются через точку).  
Если ключ отсутствует, то элемент с соответствующим ключом будет создан.
```php
    $array = [
        'key1'   => 'value1',
        'nested' => [
            'n1' => 123,
            'n2' => 'nested value 2',
        ],
    ];

    Arr::set($array, 'key2', 'value2')
    Arr::set($array, 'nested.n2', 'new nested value 2')

    // После чего в `$array` будет следующее:
    $array == [
        'key1'   => 'value1',
        'key2'   => 'value2',
        'nested' => [
            'n1' => 123,
            'n2' => 'new nested value 2',
        ],
    ];
```

#### `::remove()`
> ```php
> Arr::remove(array &$array, string $dottedKey)
> ```

Удаляет элемент из массива по ключу с нотацией "через точку".

```php
    $array = [
        'key1'   => 'value1',
        'nested' => [
            'n1' => 123,
            'n2' => 'nested value 2',
        ],
    ];

    $result = Arr::remove($array, 'nested.n2');

    // После чего в `$result` будет следующее:
    $result == [
        'key1' => 'value1',
        'nested' => [
            'n1' => 123,
        ],
    ]
```

#### `::only()`
> ```php
> Arr::only(array $array, array $keys)
> ```

Возвращает массив только с теми значениями, ключи которых указаны в $keys.
```php
    $array = [
        'key1' => 'value1',
        'key2' => 'value2',
        'key3' => 'value3',
        'key4' => 'value4',
    ];

    $result = Arr::only($array, ['key2', 'key4']);

    // После чего в `$result` будет следующее:
    $result == [
        'key2' => 'value2',
        'key4' => 'value4',
    ]
```

#### `::contains()`
> ```php
> Arr::contains(array $array, mixed $value)
> ```

Проверяет содержится ли значение в массиве
```php
    $array = [
        'key1' => 'value1',
        'key2' => 123,
    ]

    Arr::contains($array, 'key1')   // false
    Arr::contains($array, 'key2')   // false
    Arr::contains($array, 'value1') // true
    Arr::contains($array, 123)      // true
    Arr::contains($array, 375)      // false
```


File
----
Возвращает MIME-Type фала.
```php
    File::getMimeType('some-code.php')   // 'text/x-php'
    File::getMimeType('some-image.jpeg') // 'image/jpeg'
```

Image
-----
```php
    Image::createThumbnail($path, $target_width=100, $target_height=100, $bgColor=0xdddddd)
```
