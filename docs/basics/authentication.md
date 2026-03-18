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

The first step to setting up the Authentication feature in the Puko Framework is to run the following command in your terminal:

```bash
php puko setup auth <auth_name>
```

You can choose any name relevant to the entity, such as **StudentAuth** or **GuestAuth**. You may implement multiple authentication features within a single project.

Example command:

```bash
php puko setup auth StudentAuth
```

After executing the command, a new class file will be automatically generated in the `plugins/auth` directory:

```text
- plugins/
  - auth/
    - StudentAuth.php
```

If you open this file, you will find the following boilerplate methods:

```php
public function Login($username, $password)
{
    // TODO: Implement your custom login logic here
}

public function Logout()
{
    // TODO: Implement logout logic (e.g., clearing database logs)
}

public function GetLoginData($id)
{
    // TODO: Return your user data here
}
```

These methods should be customized to fit your specific authentication requirements.

---

### Login($username, $password)

In the login function, you can validate the credentials received via the parameters using models, cURL requests, or other methods.

```php
public function Login($username, $password)
{
    $student = model\primary\StudentModel::GetByUsernamePassword($username, $password);
    
    $dataToSecure = [
        "id"       => $student['id'],
        "username" => $student['user'],
        "class"    => $student['class']
    ];
    
    $permissions = ["STUDENT"];
    
    return new PukoAuth($dataToSecure, $permissions);
}
```

<small>*Note: The login function must return a **PukoAuth** object.*</small>

To invoke the login process within a controller, use the following syntax:

```php
$login = Session::Get(StudentAuth::Instance())->Login($username, $password);
```

**StudentAuth::Instance()** is a singleton object automatically managed by Puko to handle authentication.

*   **Session/Cookies:** If the login is successful, `$login` will be `true`; otherwise, it will be `false`.
*   **Bearer Tokens:** If successful, `$login` will contain the encrypted token string. If it fails, it will be `false`.

### Logout()

This is the logout callback. You can add any necessary cleanup logic here.

```php
public function Logout()
{
    // Cleanup logic here
    return true;
}
```

### GetLoginData($id)

To validate authentication, Puko decodes the encrypted string generated during login. This process is simplified via the `GetLoginData` callback, which receives the decoded information.

The callback receives two parameters: `$secure` (containing the original `$dataToSecure`) and `$permission` (containing the permission codes).
 
```php
public function GetLoginData($secure, $permission)
{
    return [
        "data"          => $secure,
        "authorization" => $permission
    ];
}
```

---

To protect a controller function from unauthenticated access, use the following doc tag:

```php
/**
 * #Auth session true
 */
public function create() {}
```
