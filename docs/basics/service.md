---
layout: default
title: Services
parent: Basics
nav_order: 4
---

# Services
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Use the `Service` controller to create web services. Puko directly returns the data in JSON format, allowing you to instantly scaffold your service using the `php puko routes service add` command. Query the data and return it at the end of your controller function.

The key difference is in the class extension:

```php
class students extends Service {}
```

A `Service` controller extends the `Service` class, rather than the `View` class used for processing HTML files.

### Service Controller Example

```php
public function students() {
    // Application logic here
    // Database operations can be performed here too

    return [
        'Identification' => 'ID120027103',
        'Name' => 'Didit Velliz',
        'Age' => 26,
        'Skills' => [
            'PHP', 'MySQL'
        ]
    ];
}
```

The data returned from the controller will be represented as JSON:

```json
{
    "Identification": "ID120027103",
    "Name": "Didit Velliz",
    "Age": 26,
    "Skills": [
      "PHP",
      "MySQL"
    ]
}
```

<small>**Dynamic Parameters:** Dynamic data retrieved from URL parameters is supported using the `{?}` keyword during route creation.</small>

---

### Service CRUD Package

The `Service` controller also supports generating multiple endpoints with a single command using the `CRUD` package. Use the following `pukoconsole` command:

```bash
php puko routes service crud [schema]/[table]
```

*   **Schema:** Refers to the database configuration (defaults to `primary`).
*   **Table:** Refers to the table name in the database.

Example: `php puko routes service crud primary/students`

This command generates a controller located at `controller/primary/students.php` with complete CRUD functions. Controller code and routes are automatically configured.
