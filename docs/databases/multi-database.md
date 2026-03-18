---
layout: default
title: Multi Database Connection
parent: Databases
nav_order: 3
---

# Multi Database Connection
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

By default, the Puko Framework uses the `primary` schema label for your main database connection. However, you can easily connect to multiple databases by assigning different schema labels to each connection.

### Setting Up Additional Connections

To add a new database connection, run the `php puko setup db` command and specify a unique schema name during the configuration process.

Example `php puko setup db` session:

```text
Database Type (mysql, oracle, sqlsrv, mongo) : mysql
Hostname (Default: localhost)               : localhost
Port (Default: 3306)                         : 3306
Schema Name (primary)                        : dashboard  <-- Use a unique label here
Database Name                                : dashboardbbn
Username                                     : root
Password                                     : *******
```

After configuration, your `config/database.php` file will contain multiple connection settings:

```php
$db['primary'] = [
    'dbType' => 'mysql',
    'host'   => 'localhost',
    'user'   => 'root',
    'pass'   => '******',
    'dbName' => 'primary',
    'port'   => '3306',
    'cache'  => false,
];

$db['dashboard'] = [
    'dbType' => 'mysql',
    'host'   => 'localhost',
    'user'   => 'root',
    'pass'   => '******',
    'dbName' => 'dashboard',
    'port'   => '3306',
    'cache'  => false,
];
```

<small>**Note:** Each **Schema Name** must be unique within your application.</small>

---

### CRUD Operations with Multiple Connections

When performing CRUD operations on a non-primary schema, you must specify the schema name in your code.

#### Insert/Save

```php
// Pass the schema name ('dashboard') as the second parameter to the constructor
$object = new models(null, 'dashboard');
// ... set properties ...
$object->save();
```

#### Update/Modify

```php
// Pass the ID and the schema name ('dashboard')
$object = new models(20, 'dashboard');
// ... update properties ...
$object->modify();
```

#### Using the DBI Directly

When using the `DBI` class, pass the schema name as the second parameter to `Prepare()`:

```php
// Perform a query against the 'dashboard' schema
$result = DBI::Prepare($sql, 'dashboard')->GetData();
$result = DBI::Prepare($sql, 'dashboard')->FirstRow();
```
