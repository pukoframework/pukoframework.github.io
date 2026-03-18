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

The Puko Framework primarily utilizes `plugins\model` object instantiation to perform CRUD (Create, Read, Update, Delete) operations. This feature is known as **Data Objects**.

To understand how Data Objects work, consider the following syntax example:

```php
$tree_seeds = new tree_seeds();
$tree_seeds->price = 35000;
$tree_seeds->tree_type = "Mango";
$tree_seeds->amount = 5;
$tree_seeds->descriptions = "Grown from seeds";

// Save the data to the database
$tree_seeds->save();
```

```php
// Update existing data
$tree_seeds->modify();
```

```php
// Remove data from the database
$tree_seeds->remove();
```

From the code snippet above, we can observe:
*   **tree_seeds** is an instantiation of a class.
*   The object has properties representing database columns, such as `price`, `tree_type`, `amount`, and `descriptions`.
*   Data persistence is performed by calling the `save()`, `modify()`, or `remove()` methods.

### Data Object Structure

Below is an example of what the `tree_seeds` class looks like internally:

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

<small>**Note:** You do not need to manually create these classes; they are automatically generated during the scaffolding process via `php puko setup db`.</small>

### How it Works

The class declaration uses **Doc Tags** to map the class to a database table:

```php
/**
 * #Table tree_seeds
 * #PrimaryKey id
 */
class tree_seeds extends Model
```

*   `#Table tree_seeds`: Specifies the database table name.
*   `#PrimaryKey id`: Identifies the primary key column.

Similarly, individual properties are mapped to table columns:

```php
/**
 * #Column id int(10)
 */
var $id = null;
```

*   `#Column id int(10)`: Links the class property to the corresponding database column.

### Key Benefits

*   **No Manual SQL:** Perform CRUD operations without writing raw SQL queries.
*   **Auto-Completion:** Benefit from IDE code completion for database fields.
*   **Clean Syntax:** Produces readable and maintainable code.

---

### Transactions

The Puko Framework supports transactional database operations to ensure data integrity.

Example of using the transactional feature:

```php
$v = new vendors();
$v->created = $this->GetServerDateTime();
$v->cuid = 2;

$v->vendors = "Didit Velliz";
$v->phone = "081389001110";
$v->city = "New York";
$v->address = "Testing Street";

// Transaction block
$transaction = DBI::Transactional('primary', function($dbi) use ($v) {
    // Perform save, delete, or update operations
    $v->save($dbi);

    // Return true to commit the transaction, or false to roll back
    return true;
});
```

The `$transaction` variable will evaluate to `true` or `false`, indicating whether the entire transactional process succeeded or failed.
