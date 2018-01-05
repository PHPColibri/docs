Caching
=======

- [Configuring](#configuring)
- [Functionality](#functionality)

> For caching supports all known [Memcached](http://memcached.org/).


Configuring
-----------

Everything is ready to work, if you have installed Memcached with default settings.
If your Memcached is listening another port or installed on another machine,
open the config file `cache.php` and you will quickly understand what needs to be corrected.

After installing and configuring the cache set `useCache` in` application.php` to `true`
and Colibri will use it for internal purposes.


Functionality
-------------

#### Write to cache
```php
Cache::set('key', 'value', $seconds = null);
```
`$seconds === null`  - use `cache.default-ttl` setting;  
`$seconds === 0`     - never expired;  
`$seconds > 2592000` - Unix timestamp (2592000 - 30 days).  

#### Get the value
```php
Cache::get('key', $default);
```

#### Delete
```php
Cache::delete('key');
```

#### Get if exists or set
If you need to cache something for a little (or not) time,  
then very often the following manipulations are used:  
\- see if already there is a value in the cache;  
\- if there is, then use it;  
\- if it does not exist, then:  
\- \- produce "calculations" of the value;  
\- \- save it in the cache;  
\- \- and then use.  
instead, you can simply use the `::remember()` method:  
```php
Cache::remember($key, function() {
	return $mySomeNewValue;
}, $seconds = null);
```
