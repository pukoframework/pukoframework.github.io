---
layout: default
title: Configuration
nav_order: 3
---

# Configuration
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Primary configuration settings are bound to the `.env` file. For more granular control over your workflow, configuration files are grouped within a single directory. You can view an example of the configuration structure in the [official GitHub repository](https://github.com/velliz/puko/tree/master/config).

## app.php

The `config/app.php` file is used to store read-only variables that can be retrieved within your controllers. This file is structured as a PHP array and consists of three root identifiers: `const`, `cache`, and `logs`.

### const

You can register application-wide constants in the `const` section. For example:

```php
<?php return [
    'const' => [
        'API' => 'https://localhost:3000/api/',
    ]
    // ...
];
```

### cache

By default, the Puko Framework utilizes Memcached as its caching driver. You can configure the connection settings here.

```php
<?php return [
    // ...
    'cache' => [
        'kind' => 'MEMCACHED',
        'expired' => 100,
        'host' => 'localhost',
        'port' => 11211,
    ]
    // ...
];
```

<small>*Note: Advanced cache implementations are coming soon.*</small>

### logs

Puko utilizes logs as a custom hook with built-in **Slack Incoming WebHooks** for error reporting (disabled by default). If you have a Slack account, you can set up an **Incoming WebHook** URL and provide it in the configuration. Set the `active` state to `true` to enable Slack notifications.

<small>*Note: Support for custom logs is coming soon.*</small>

## database.php

Database configurations are located in the `config/database.php` directory. You can specify multiple connections by using unique schema names as identifiers. Puko simplifies the database configuration process via the `pukoconsole` tool, which is included as a development dependency.

<small>*As of version 1.1.6, Puko supports MySQL and MSSQL database engines.*</small>

To set up your database, run:

```bash
php puko setup db
```

To refresh the database without overwriting existing configurations, use: `php puko refresh db`.

### Configuration Parameters

| Parameter | Description | Example |
| :--- | :--- | :--- |
| **Database Type** | Supported engines: `mysql` or `mssql`. | `mysql` |
| **Hostname** | The IP address or hostname of the database server. | `localhost` |
| **Port** | The port used by the database server. | `3306` |
| **Schema Name** | A unique identifier for the connection (e.g., for multiple databases). | `primary` |
| **Database Name** | The name of the database. | `inventory` |
| **Username** | The database user. | `root` |
| **Password** | The password for the database user. | `******` |
| **Driver** | The specific database driver to use. | `mysql` |

<small>*The setup wizard will ask if you would like to configure additional connections (y/n).*</small>

For more detailed information, please refer to the [Database]({{ site.baseurl }}{% link docs/databases/index.md %}) section.

## encryption.php

Encryption is essential for securing various aspects of your application. The Puko Framework uses encryption primarily to protect user authentication data across Sessions, Cookies, and Bearer tokens. This can be configured using the `pukoconsole` tool.

<small>*Puko utilizes the AES-256-CBC encryption algorithm.*</small>

To configure encryption, run the following command in your terminal:

```bash
php puko setup secure
```

## routes.php

The `config/routes.php` file contains all routing information. This file is intended to be read-only, as most routing is managed automatically by the `pukoconsole` tool.

For more details, see the [Routing]({{ site.baseurl }}{% link docs/basics/routing.md %}) documentation.

## custom.php

Puko allows for the creation of custom configuration files. For example, if you wish to integrate RabbitMQ, you can create a `config/rabbitmq.php` file:

```php
<?php return [
    'username' => 'administrator',
    'password' => '*******',
    'host' => '192.16.60.31',
    'port' => '5678'
];
```

Custom configuration files are accessible throughout the application lifecycle via `Config::Data('<filename>')`. This provides a flexible way to extend the framework's functionality.

### Usage Example

You can retrieve values using the following code snippet:

```php
// Initialize the config data
$config = pukoframework\config\Config::Data('rabbitmq');

// Retrieve values
$username = $config['username'];
$password = $config['password'];
$host     = $config['host'];
$port     = $config['port'];
```
