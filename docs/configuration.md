---
layout: default
title: Configuration
nav_order: 2
---

# Configuration
{: .no_toc }


Puko Framework has some specific configuration parameters that can be defined in your project config directory.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


All of configuration needed is already bind into `.env` file, but if you want to control more configuration in your workflow, all file is grouped together in one directory. View via github repo site's [config](https://github.com/velliz/puko/tree/master/config) as an example.


## app.php

App `config/app.php` used to hold read-only variable and later can retrieved in controller for usages.
App file structured as PHP array and consists 3 root identifier `const`, `cache` and `logs`.

- const

You can register your constant in `const` sections of file. For example:

```php
<?php return [
    'const' => [
        'API' => 'https://localhost:3000/api/',
    ]
    ...
];
```

- cache

By default, Puko Framework utilize memcached as cache driver. You can configure the connection settings here.

```php
<?php return [
    ...
    'cache' => [
        'kind' => 'MEMCACHED',
        'expired' => 100,
        'host' => 'localhost',
        'port' => 11211,
    ]
    ...
];
```

<small>cache implementation coming soon</small>

- logs

Puko utilize logs as a custom hook with build in **Slack Incoming WebHooks** for error reporting, by default set to false. If you have slack accounts, you can setup a **Incoming WebHook** URL and paste it in Slack url. Then in the Puko Framework Slack section, set the `active` state to `true`.

<small>custom logs is coming soon</small>

## database.php

Database configurations located at `config/database.php` folder. You can specify more than one connection because puko uses schema name for each connection string. Puko also scaffolds database configuration process with `pukoconsole` tools included as _dev-dependency_.

<small> as version 1.1.6 puko only supports MySQL and MSSQL database engine</small>

```bash
php puko setup db
```

or if you only refresh the database (without re-write the database configuration): `php puko refresh db`

Items asked:

|Items|Description|Examples|
|---|---|---|
|Database Type|Only supports MySQL and MSSQL for now|mysql|
|Hostname|Databaase IP address|localhost|
|Port|Databaase port address|3306|
|Schema Name|Schema name as identifier for multiple database|primary|
|Database Name|Name of databases|inventory|
|Username|User databases|root|
|Password|Password databases|******|
|Driver|Database Driver|mysql|

<small>At the end wizard is asking for another connection you can answer with y/n</small>

You can refer to database section for detailed information.

## encryption.php

Encryption required to secure many aspects in our applications. Puko framework mostly using this to secure user authentication data in several forms. Sessions, Cookies, and Bearer. Puko can scaffolds this process with `pukoconsole` tools included as _dev-dependency_.

<small>Puko using AES-256-CBC as encryption algorithms</small>

To start configure Encryption, you can type in _console/terminal/powershell_:

```text
php puko setup secure
```

## routes.php

Routes located in `config/routes.php` and holds all routing information. This file supposed as read-only because most of the puko routing done with `pukoconsole` tools included as _dev-dependency_.

See [Routing]({{ site.baseurl }}{% link docs/basics/routing.md %}) for more information.

## Custom <file>.php

Puko framework can also create custom config file. for example if you want to add RabbitMQ message queue you can create new `config/rabbitmq.php` file

```php
<?php return [
    'username' => 'administrator',
    'password' => '*******',
    'host' => '192.16.60.31',
    'port' => '5678'
];
```

_note: Custom `<file>.php` is available trough app lifecycles via `Config::Data('<file>')`. Which will allow for more control and expansion of all the functionality of the framework itself._

You can retrieve the value with this code snippet:

```php
//initialize config skeleton
$config = pukoframework\config\Config::Data('rabbitmq');

//retrieve the value
$config['username'];
$config['password'];
$config['host'];
$config['port'];
```
