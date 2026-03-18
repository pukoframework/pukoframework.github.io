---
layout: default
title: Docs Command
parent: Basics
nav_order: 10
---

# Docs Command
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Puko's **Docs Command** is designed to simplify controller creation and speed up backend development. It follows an annotation-like pattern, allowing you to define settings directly above a class or function name in a doc comment.

Example:

```php
/**
 * #Master master-admin.html
 * #Value title Welcome
 * #Date before 31-08-2016 12:00:00
 */
public class registrasi extends View {
```

Because these commands are placed within standard PHP doc comments, your function logic remains clean. All commands must be prefixed with a hashtag (#) character; otherwise, they will be skipped by the framework.

```php
/**
 * #...
 */
```

### Complete Catalog of Commands

| Command | Key Identifier | Value | Description |
| :--- | :--- | :--- | :--- |
| **#Value** | `name` | `Didit Velliz` | Sends data named `name` with the specified value directly to the view. |
| **#Template** | `master` | `true/false` | Option to use the master layout. |
| **#Template** | `html` | `true/false` | Option to output as an HTML file in the browser. |
| **#Template** | `cache` | `true/false` | Option to cache HTML output with the PTE cache driver. |
| **#Date** | `before/after` | `d-m-Y H:i:s` | Limits access based on server time. If the server time is outside the specified range, the function cannot be accessed. |
| **#ClearOutput** | `binary` | `true/false` | Option to output results processed through the framework. |
| **#Auth** | `bearer` | `true/false` | Protects access to the function. `true` allows access only to authenticated users. |
| **#Auth** | `session` | `true/false` | Protects access to the function. `true` allows access only to authenticated users. |
| **#Auth** | `cookies` | `true/false` | Protects access to the function. `true` allows access only to authenticated users. |
| **#Master** | `-` | `master.html` | Specifies a custom file for the master layout. |
| **#DisplayException** | `-` | `true/false` | Forwards the process to View or Service middleware when an exception occurs. |
| **#UnderConstruction** | `-` | `true/false` | Displays an "Under Construction" page. |
| **#Permission** | `\pukoframework\auth\Bearer@\plugins\auth\UserAuth` | `permissions@MANAGER` | Displays a "Permission Denied" page if access checks fail. |
