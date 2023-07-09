---
layout: default
title: Authentication
parent: Basics
nav_order: 6
---

# Authentication
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The first step to set up the Authentication feature in the Puko Framework is to type the following command in the console/terminal:

```text
php puko setup auth <auth_name>
```

You can choose any name related to the entity, for example, **StudentAuth** or **GuestAuth**. You can have more than one authentication feature.

For example, the command should look like this:

```text
php puko setup auth StudentAuth
```

After executing the command, a file class will be automatically generated and placed in the _plugins/auth_ directory:

```text
- plugins/
  - auth/
    - StudentAuth.php
```

If you open the _file_, you will see the following empty methods:

```php
public function Login($username, $password)
{
    //todo: your custom login code here
}

public function Logout()
{
    //todo: create or clear log in databases
}

public function GetLoginData($id)
{
    //todo: return your user data here
}
```

These methods are meant to be filled with your own custom authentication implementation.

---

**Login($username, $password)**

For the login function, you can validate the username and password received from the function parameters and validate them through models or CURL requests.

```php
public function Login($username, $password)
{
    $student = model\primary\StudentModel::GetByUsernamePassword($username, $password);
    $dataToSecure = [
        "id" => $student['id'],
        "username" => $student['user'],
        "class" => $student['class']
    ];
    $permission = ["STUDENT"];
    return new PukoAuth($dataToSecure, $permission);
}
```

<small>The login function must return a **PukoAuth** class object.</small>

To implement the login code in the controller, use the following syntax as an example:

```php
$login = Session::Get(StudentAuth::Instance())->Login($username, $password);
```

**StudentAuth::Instance()** is an object built automatically by Puko to handle the login process.

For **Session** and **Cookies**, if the login process is successful, the **$login** variable will be assigned a value of **true**, and **false** if the login process fails.

For **Bearer**, if the login process is successful, the **$login** variable will hold the encrypted string data. If it fails, the **$login** variable will be _false_.

**Logout()**

This is the logout callback. You can write any code to clean up the logout process.

```php
public function Logout()
{
    //cleanup process here
    return true;
}
```

**GetLoginData($id)**

To validate an authentication, you must decode the encryption string generated during the login process. 
To simplify that process, Puko uses the GetLoginData callback function after decoding the string information, which contains two parameters. 
The first parameter, **$secure**, contains all the **$dataToSecure** supplied during the login process. 
Similarly, the second parameter, **$permission**, will contain all the permission codes supplied during the login process.
 
```php
public function GetLoginData($secure, $permission)
{
    return [
        "data" => $secure,
        "authorization" => $permission
    ];
}
```

---

If you want to protect a controller function from being accessed by an unauthenticated user, 
you can use the following authentication documentation command:

```php
/**
 * #Auth session true
 */
public function create() {}
```
