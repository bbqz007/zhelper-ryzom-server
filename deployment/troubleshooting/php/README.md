原始代码web使用php5x，代码版本较新的web使用php7x。

本文档的问题基于在centos altarch 7平台上部署，可能x64平台上或其它linux发行版平台不一定会发生。

1。socket连接的权限，不论nginx运行在什么用户，在upstream时仍会以nginx身份进行连接，所以必须保证nginx在inet组。否则connect failed: 13 permission denied。

2。网站的目录的访问权限，必须允许nginx写，php-fpm也使用nginx身份，即使使用了-R选项（run as root）。

3。pdo_sqlite默认使用sqltie3_column_table_name，这是3.32.x版本的sqlite3，centos7只使用到3.2.x的版本。所以编译时要手动注释掉该函数的调用的地方。

4。php53跟php54的扩展接口有变化，不可以直接使用pecl进行安装，可以在github找到对应版本的php源代码，下载ext对应的扩展代码，然后，phpize，configure，make。

5。
```php
Warning: mysqli_close(): It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected the timezone 'UTC' for now, but please set date.timezone to select your timezone. in /opt/tmp/ryzom/nel/web/public_php/setup/install.php on line 127

Warning: mysqli_close(): Couldn't fetch mysqli in /opt/tmp/ryzom/nel/web/public_php/setup/install.php on line 127

Disconnected from the Support SQL server
```
在php.ini设置date.timezone=

6。同一台机器多版本php同时并存，编译配置ext时必须使用选项--with-php-config指定php版本对应的php-config路径。

