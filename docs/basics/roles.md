---
layout: default
title: Roles
parent: Basics
nav_order: 7
---

# Search
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Role is part-of the Auth. For specifying what user can do, what user cannot do. and so on. 
At implementation level, you can see it in `PukoAuth` class instance as seconds parameter:

```php
public function Login($username, $password)
{
    ...
    $permission = [];
    return new PukoAuth([], $permission);
}
```

From code sample above, you have permission wrapped as array of 'strings', so any authentication can consists of
no permission, one permission, or many permissions as needed.

Puko also have a doc command to seal a function with an permission code like this:

```php
/**
 * #Auth session true
 * #Permission \pukoframework\auth\Bearer@\plugins\auth\UserAuth permissions@MANAGER
 */
public function profile()
```

Permission defined as `MANAGER`. 
So only user with this permission assigned can access the functions.

> You need to write full path with PSR-4 directory structure to your plugin auth class. 
> `\pukoframework\auth\Bearer@\plugins\auth\UserAuth`

