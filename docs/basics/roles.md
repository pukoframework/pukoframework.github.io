---
layout: default
title: Roles and Permissions
parent: Basics
nav_order: 7
---

# Roles and Permissions
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Roles and permissions are integral components of the Authentication (Auth) system in the Puko Framework. They define what an authenticated user can or cannot do within the application. At the implementation level, you specify permissions within the `PukoAuth` class instance:

```php
public function Login($username, $password)
{
    // ...
    $permissions = ["MANAGER"];
    return new PukoAuth([], $permissions);
}
```

From the example above, permissions are passed as an array of strings. An authentication result can include no permissions, one permission, or multiple permissions.

### Restricting Access via Permissions

Puko provides a **Doc Tag** to protect a function based on specific permission codes:

```php
/**
 * #Auth session true
 * #Permission \pukoframework\auth\Bearer@\plugins\auth\UserAuth permissions@MANAGER
 */
public function profile()
```

In this case, the function is sealed with the `MANAGER` permission. Only users with this permission assigned will be able to access the function.

<small>**Important:** You must use the full PSR-4 directory path to your plugin auth class when defining the permission: `\pukoframework\auth\Bearer@\plugins\auth\UserAuth`.</small>
