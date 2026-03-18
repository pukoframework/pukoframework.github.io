---
layout: default
title: Internationalization
parent: Utilities
nav_order: 4
---

# Internationalization
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Managing multiple languages for a website or API without sacrificing performance is a common challenge. The Puko Framework addresses this by dividing internationalization into two main concerns: the frontend and the backend.

### Language Detection

The framework determines the language to use based on the client's HTTP request. Clients (web browsers, mobile apps, or desktop applications) should send a custom `X_LANG` header containing a two-digit lowercase country code (e.g., `en`, `id`, `jp`). If this header is missing, the framework defaults to `id`.

### Frontend Localization (HTML)

To maintain optimal performance, the Puko Framework uses separate HTML files for different languages. This approach prevents unnecessary runtime processing, though it requires maintaining copies of your templates for each supported language.

Organize your HTML files in the following directory structure:

```text
assets/
  html/
    en/
      function/
        template.html
    id/
      function/
        template.html
```

<small>*Note: You can add any ISO 639-1 language code as needed (e.g., `cn`, `kr`, `jp`).*</small>

---

### Backend Localization (PHP)

On the backend, localization is managed through the `say()` helper method within your controllers.

#### Basic Usage

```php
throw new Exception($this->say('NOT_FOUND'));
```

The key (e.g., `NOT_FOUND`) is stored in JSON files located in `assets/master/`. For example, `id.master.json` or `en.master.json`.

#### Dynamic Values

You can pass custom variables to your localized strings using the second parameter of the `say()` method.

```php
throw new Exception($this->say('WRONG_ADDR', [$username, $email]));
```

In your JSON language file, use `%s` (standard `sprintf` format) to define where the variables should be inserted:

```json
{
    "NOT_FOUND": "No data found",
    "WRONG_ADDR": "The address for %s was not found for the email: %s"
}
```

This centralized approach allows your application's language resources to be easily translated and maintained.
