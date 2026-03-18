---
layout: default
title: Model
parent: Basics
nav_order: 5
---

# Model
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The **Model (M)** layer in the Puko Framework's HMVC architecture is responsible for connecting your application to one or more databases for CRUD operations. All database operations are handled by the **DataBase Interface (DBI)** singleton object.

### Database Setup

To simplify the connection setup, use the `pukoconsole` tool:

```bash
php puko setup db
```

#### Configuration Parameters

| Parameter | Description | Example |
| :--- | :--- | :--- |
| **Database Type** | Currently supports MySQL/MariaDB. | `mysql` |
| **Hostname** | Database server IP address or hostname. | `localhost` |
| **Port** | Database server port. | `3306` |
| **Schema Name** | A unique identifier for the connection. | `primary` |
| **Database Name** | The name of the database. | `inventory` |
| **Username** | The database user. | `root` |
| **Password** | The database password. | `******` |

<small>*Note: You can configure additional connections as needed.*</small>

Puko stores the connection settings in `config/database.php` and generates corresponding PHP class models (Data Object Wiring) in the `plugins/model/<schema>` directory. These generated files should not be modified.

---

### Data Object Wiring

Data object wiring is inspired by the **Data Access Object (DAO)** pattern. It provides an intuitive way to interact with your database columns through object properties, including full IDE auto-completion.

Assume you have a database table named `inventory`:

| id | name | created | descriptions |
| :--- | :--- | :--- | :--- |
| 1 | Chair | 2020-08-15 | Minimalist chair made from pine wood. |
| 2 | Laptop | 2020-08-16 | Gaming notebook with core i7 and RTX2070 Max-Q. |

<small>**Important:** Table names must only contain letters without special characters or spaces.</small>

#### Basic CRUD Operations

*   **Create/Save:**

```php
$inventory = new plugins\model\primary\inventory();
$inventory->id = $_POST['id'];
$inventory->name = $_POST['name'];
$inventory->created = date('Y-m-d H:i:s');
$inventory->descriptions = $_POST['descriptions'];

$inventory->save();
```

*   **Read:**

```php
$inventory = new plugins\model\primary\inventory(1);
echo (array) $inventory;
```

*   **Update/Modify:**

```php
$inventory = new plugins\model\primary\inventory(1);
$inventory->name = "Updated Name";
$inventory->modify();
```

*   **Delete/Remove:**

```php
$inventory = new plugins\model\primary\inventory(1);
$inventory->remove();
```

*   **Get All Data:**

```php
$all = plugins\model\primary\inventoryContracts::GetAll();
```

---

### Custom Models & Complex Queries

For more complex scenarios, such as joins or stored procedures, you can extend the generated model and implement **ModelContracts**.

Create a new model: `model/InventoryModel.php`

```php
class InventoryModel extends inventory implements ModelContracts {
    // Implement the 9 abstract methods from ModelContracts
}
```

#### ModelContracts Methods

*   `GetData()`: Returns all database entries in an array format.
*   `GetById($id)`: Returns a single row specified by its ID.
*   `IsExists($id)`: Returns true if a record exists for the given ID.
*   `IsExistsWhere($column, $value)`: Returns true if a record exists matching a custom condition.
*   `GetDataSize()`: Returns the total count of records.
*   `GetDataSizeWhere($condition)`: Returns the count of records based on a specific condition.
*   `GetLastData()`: Returns the most recently inserted record.
*   `SearchData($keyword)`: Returns search results in an array format.
*   `GetDataTable($condition)`: Returns search results formatted for DataTables JSON.

#### Implementation Example

```php
public static function SearchData($keyword = []) {
    $strings = "";
    foreach ($keyword as $column => $values) {
        $strings .= sprintf(" AND (%s = '%s') ", $column, $values);
    }

    $sql = sprintf("SELECT i.id, i.created, i.name, i.descriptions
                    FROM inventory i
                    WHERE (i.created IS NOT NULL) %s;", $strings);

    return DBI::Prepare($sql)->GetData();
}
```

---

### The DataBase Interface (DBI)

The DBI class offers powerful features for interacting with your database directly.

*   `Prepare`: Passes a SQL query to the DBI class.
*   `GetData($params)`: Retrieves all data in an indexed array format. Supports prepared statements (e.g., `@1`, `@2`).
*   `FirstRow($params)`: Retrieves only the first row in a single array format.
*   `Run($params)`: Executes queries that do not return a result set (e.g., `UPDATE`, `DELETE`, or Stored Procedures).

Example with Prepared Statements:

```php
$sql = "SELECT * FROM inventory WHERE (id = @1) AND (name = @2);";
$response = DBI::Prepare($sql)->GetData($id, $name);
```
