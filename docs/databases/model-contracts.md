---
layout: default
title: Model Contracts
parent: Databases
nav_order: 1
---

# Model Contracts
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

When working with database queries, it is common to encounter repetitive or redundant query patterns. The Puko Framework provides **Model Contracts** to help you build standard, reusable building blocks for your SQL queries.

Puko automatically generates these standard methods when you execute the `php puko setup db` command.

### Standard Contract Methods

The following methods are pre-defined in the model contracts to serve as your starting point for common database operations:

*   `GetData()`: Retrieves all records from the database.
*   `GetById($id)`: Retrieves a specific record by its primary ID.
*   `IsExists($id)`: Checks if a record exists for a given ID.
*   `IsExistsWhere($column, $value)`: Checks if a record exists matching a custom condition.
*   `GetDataSize()`: Returns the total number of records in the table.
*   `GetDataSizeWhere($condition = [])`: Returns the count of records based on a specified condition.
*   `GetLastData()`: Retrieves the most recently inserted record.
*   `SearchData($keyword = [])`: Performs a search based on provided keywords.
*   `GetDataTable($condition = [])`: Formats search results specifically for DataTables.

By utilizing these pre-defined methods, you can ensure consistency and reduce boilerplate code across your application's models.
