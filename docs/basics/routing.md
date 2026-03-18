---
layout: default
title: Routing
parent: Basics
nav_order: 1
---

# Routing
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The Puko Framework provides different types of routing clauses, categorized by the function of the controller. These are, in order of common usage: `view`, `service`, and `console`. Routing is managed via the `pukoconsole` tool, which is included as a development dependency to speed up web application development.

### Create New Routing

To add a new route, run the following command in your terminal:

```bash
php puko routes <controller_clause> add <rest_style_urls>
```

<small>**URL Structure:** You can use `{?}` or `?` in your REST-style URLs to identify dynamic PHP `GET` parameters.</small>

Example:

```bash
php puko routes view add member/?/reports
```

#### Configuration Parameters

| Parameter | Description | Example |
| :--- | :--- | :--- |
| **Controller Name** | The file name. Use a backslash (`\`) to place the file in a sub-directory. | `member\reports` |
| **Function Name** | The name of the function to be called. | `pages` |
| **Accept?** | The HTTP verbs allowed (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`). Multiple verbs can be comma-separated. | `get,post` |

<small>**Important:** While routes use a forward slash (`/`), you must use a backslash (`\`) to separate controller names to prevent directory path errors across different operating systems.</small>

### Automatically Generated Files

After successfully adding a route, the Puko Framework automatically updates and generates the following files:

```bash
config/routes.php
controller/member/reports.php
```

If the routing clause was `view`, the following additional files will be generated:

```bash
assets/html/en/member/reports/pages.html
assets/html/id/member/reports/pages.html
```

To list all registered routes, use:

```bash
php puko routes list
```

<small>**Important:** All generated routes are stored in `config/routes.php`. To maintain the routing structure, it is recommended not to edit this file manually.</small>

---

### Update Routing

To update or modify the HTTP verbs accepted by a registered route, use the update command:

```bash
php puko routes view update member/?/reports
```

Apply your changes through the prompted wizard.

---

### Delete Routing

Unused routes can be removed with the `remove` command:

```bash
php puko routes view remove member/{?}/reports
```

<small>**Security Note:** For security reasons, Puko does not delete the corresponding `.php` or `.html` files. You must review and remove them manually.</small>
