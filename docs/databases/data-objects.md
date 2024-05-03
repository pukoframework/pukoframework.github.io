---
layout: default
title: Data Object
parent: Databases
nav_order: 1
---

# Data Object
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


Puko framework primarily makes use of `plugins\model` object instantiation to perform CRUD processing.
This feature called Object Data. 
In order to see how this Data Object works, you can look at the following syntax example:

```php
$tree_seeds = new tree_seeds();
$tree_seeds->price = 35000;
$tree_seeds->tree_type = "Manggo";
$tree_seeds->amount = 5;
$tree_seeds->descriptions = "From seeds";

$tree_seeds->save();
```

```php
$tree_seeds->modify();
```

```php
$tree_seeds->remove();
```

From the code snippet above we can see that:
* **tree_seeds** is the intantiation of a class.
* **tree_seeds** have properties in the form of _price, tree_type, amount_ and _descriptions_.
* **tree_seeds** perform the data storage process by calling the `save()` function.

If you are curious about what the tree seed class itself looks like, then here's the code:

```php
<?php

namespace plugins\model;

use pukoframework\pda\DBI;
use pukoframework\pda\Model;

/**
 * #Table tree_seeds
 * #PrimaryKey id
 */
class tree_seeds extends Model
{
    
    /**
     * #Column id int(10)
     */
    var $id = null;

    /**
     * #Column tree_type varchar(45)
     */
    var $tree_type = null;

    /**
     * #Column amount int(5)
     */
    var $amount = null;

    /**
     * #Column price int(8)
     */
    var $price = null;

    /**
     * #Column descriptions varchar(225)
     */
    var $descriptions = null;

}
```

<small>Attention: you don't need to create this class because it is automatically generated in the scaffolding `php puko setup db`</small>

The first part to pay attention to is the class declaration:

```php
/**
 * #Table tree_seeds
 * #PrimaryKey id
 */
class tree_seeds extends Model
```

Where there is a `#Table tree_seeds` and `#PrimaryKey id` which shows that the class connected to a table in the database
with the name of the table **tree_seeds** and have a primary key with the name of the column **id**. 

During the scaffolding process, the puko reads the data structure down to the column level.
We can also pay attention to the properties that are formed in the class:

```php
/**
 * #Column id int(10)
 */
var $id = null;
```

Using `#Column id int (10)` let the puko framework know that the variable associated with the same named column in the database.
So what's so interesting about this method?

* You can do CRUD processing without writing SQL queries.
* Create code Completion-able database fields in PHP code.
* Produces a clean and easy syntax.

Transactional

Example use of transactional features described:

```php
$v = new vendors();
$v->created = $this->GetServerDateTime();
$v->cuid = 2;

$v->vendors = "Didit Velliz";
$v->phone = "081389001110";
$v->city = "New York";
$v->address = "Testing Street";

//transaction blocks
$transaction = DBI::Transactional('primary', function($dbi) use ($v) {
    //save, delete or update process
    $v->save($dbi);

    //don't forget to return true or false as commit flag for transaction.
    return true;
});
```

The transaction object will evaluated as true or false to determinate is the transactional process sucess for all or failure for all.