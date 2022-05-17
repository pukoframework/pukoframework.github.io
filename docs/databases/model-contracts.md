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

When dealing with database and query. We sometimes find the query variations is overwhelming
and become redundant at some part. 
So puko provides modelContracts to help build the necessary building blocks of SQL query.
Puko also generate the starting point after you execute the `php puko setup db` command.

Generated method extracted from contract is:

```php
GetData();
GetById($id);
IsExists($id);
IsExistsWhere($column, $value);
GetDataSize();
GetDataSizeWhere($condition = []);
GetLastData();
SearchData($keyword = []);
GetDataTable($condition = []);
```

Hope above list of method can used as your starting point when dealing with CRUD operation.