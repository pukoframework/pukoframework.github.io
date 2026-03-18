---
layout: default
title: Framework
parent: Utilities
nav_order: 6
---

# Framework
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

When writing controller logic, you may often need to generate random strings, retrieve the current server time, or access configuration constants. This section lists the available helper methods provided by the Puko Framework to address these needs. These methods are directly accessible within your **View**, **Service**, or **Console** controllers.

### Helper Methods

#### Get Server Date Time
Retrieves the current server date and time in the format `Y-m-d H:i:s`.

```php
$dateTime = $this->GetServerDateTime();
```

#### Generate Random Token
Generates a random alphanumeric string (A-Z, a-z, 0-9). By default, it produces a 6-character string.

```php
// Generates a 6-character token, e.g., "p4vRXo"
$token = $this->GetRandomToken();

// Generates a custom-length token
$longToken = $this->GetRandomToken(16);
```

#### App Constant
Retrieves a constant defined in the `const` section of your `config/app.php` file.

```php
$apiEndpoint = $this->GetAppConstant('API_ENDPOINT');
```

#### Redirect
Redirects the user to a specified routing path.

```php
// Redirects to the 'login' route
$this->RedirectTo('login', false);
```

*   **First Parameter:** The destination route string.
*   **Second Parameter:** A boolean. If `true`, the browser's history is replaced (similar to `location.replace`). If `false`, the redirection is treated as a standard link click.

#### Say (Internationalization)
Fetches a localized string from the JSON schema files located in `assets/master`.

```php
$welcomeMessage = $this->say('WELCOME_GUEST');
```

#### Base URL
Retrieves the base URL of your application (e.g., `http://localhost:3000/`).

```php
$baseUrl = Framework::$factory->getBase();
```

#### Root Path
Retrieves the absolute server file system path to your project's root directory (e.g., `/var/www/html/puko`).

```php
$rootPath = Framework::$factory->getRoot();
```

#### Get Environment
Retrieves the current application environment setting (e.g., `DEVELOPMENT`, `STAGING`, or `PROD`).

```php
$env = Framework::$factory->getEnvironment();
```

#### Get Start
Retrieves the precise PHP execution start time (microtime).

```php
$startTime = Framework::$factory->getStart();
```

---

### Controller Lifecycle Overrides

You can hook into the controller lifecycle by overriding the following methods:

```php
public function BeforeInitialize()
{
    // Executed before the primary controller function
}

public function AfterInitialize()
{
    // Executed after the primary controller function
}
```
