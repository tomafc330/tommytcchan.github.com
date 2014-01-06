---
layout: post
title: "PHP - Set up XDebug"
date: 2013-02-17 16:24
comments: true
categories: [php,setup,xdebug]
---
So my client is using an older version of PHP (5.2 to be exact), and I wanted to install a debugger to ensure that I can test out the flows and examine variables. Finally after an hour of fiddling with it I got it to work.

### The setup
This guide uses the following:

WAMP Server 2.2
PHP 5.2
Apache 2.2
PHPStorm 5.0.1

### Steps
1. Goto http://xdebug.org/download.php and download the Xdebug version 2.1.2. Note that's the last version where they compiled a VC6 version. I got the Thread Safe (TS) version.

2. Now edit your php.ini file. Make sure to put the following in:
```
zend_extension_ts= "C:\wamp\bin\php\php5.2.2\ext\php_xdebug-2.1.2-5.2-vc6.dll"
xdebug.idekey=PHPSTORM
xdebug.remote_enable=true
;extension=php_xdebug-2.1.2-5.2-vc6.dll
````
Note that we explicitly removed the extension= declaration. We *don't* want it.

3. Now grab the bookmarklet generator from here: http://www.jetbrains.com/phpstorm/marklets/

4. Restart the server

5. Have PHPStorm start listening to the connections.

6. Put a breakdown somethere you know that the execution will hit it.

7. Enjoy!
