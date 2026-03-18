---
layout: default
title: Quick Start
nav_order: 2
---

# Quick Start
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Welcome! We are thrilled to have you here.

"From zero to hero." Every journey begins with a first step, and to make that step with confidence, you need a reliable manual to rely on. 
This section will guide you through a ~15-minute tutorial: **"Creating a simple TODO web application"** using the Puko Framework. 

<small>Prerequisites: PHP version >= 7.0, Composer, and MySQL installed on your machine.</small>

## Prerequisites

Import the following table into your MySQL database:

```sql
CREATE TABLE activities (
    id INT(11) NOT NULL AUTO_INCREMENT,
    task VARCHAR(255) NOT NULL,
    status TINYINT(1) DEFAULT 0,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

## Setup Project

First, create a new Puko project using Composer and start the development server:

```bash
composer create-project pukoframework/pukoframework todo-app
cd todo-app
php puko serve 8080
```

Visit `http://localhost:8080` in your browser to verify that the framework is running correctly.

## Setup Database

Configure your database connection in the `.env` file at the root of your project:

```text
DB_DRIVER=mysql
DB_HOST=localhost
DB_PORT=3306
DB_NAME=todo_db
DB_USER=root
DB_PASS=yourpassword
```

Next, run the database setup wizard to generate the **Data Object Wiring** for our `activities` table:

```bash
php puko setup db
```
Follow the prompts (choose `mysql`, `localhost`, and the `primary` schema). Puko will automatically generate the model classes in the `plugins/model/primary/` directory.

## Backend API

Let's create a Service controller to handle task data. Use the Puko CLI to scaffold a new route:

```bash
php puko routes service add api/tasks
```

Open `controller/api/tasks.php` and update the code to fetch all tasks from the database:

```php
namespace controller\api;

use pukoframework\middleware\Service;
use plugins\model\primary\activitiesContracts;

class tasks extends Service {
    public function main() {
        return activitiesContracts::GetAll();
    }
}
```

## Frontend

Now, let's create a View controller to display our Todo list on a web page:

```bash
php puko routes view add tasks
```

### 1. Controller Logic
Open `controller/tasks.php` and pass the database data to the view:

```php
namespace controller;

use pukoframework\middleware\View;
use plugins\model\primary\activitiesContracts;

class tasks extends View {
    public function main() {
        return [
            'title' => 'My Todo List',
            'activities' => activitiesContracts::GetAll()
        ];
    }
}
```

### 2. HTML Template
Open the generated template at `assets/html/en/tasks/main.html` and use **PTE tags** to render the dynamic list:

```html
<h1>{!title}</h1>

<ul class="task-list">
    <!--{!activities}-->
    <li>
        <strong>{!task}</strong> 
        <span class="badge">{!status}</span>
    </li>
    <!--{/activities}-->
</ul>
```

## Summary

In just 15 minutes, you have:
1.  **Scaffolded** a new project and routing system.
2.  **Wired** a database table to a PHP Data Object.
3.  **Built** a functional JSON API endpoint.
4.  **Rendered** a dynamic frontend using PTE tags.

We hope this provides a clear picture of what the Puko Framework is, its mechanism, and its strengths. May the **Puko Framework** be the perfect match for your upcoming projects! 😊
