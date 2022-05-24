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

Puko framework have different kind of routing clause "as-is" controller functionality, we call it sorted from most ussage in order: `view`, `service`, and `console`. Puko framework can scaffolds Routing process with `pukoconsole` tools included as _dev-dependency_ to speed up your web app develpment process.

## Create New Routing

To add new Routing, you can type in _console/terminal/powershell_:

```text
php puko routes <controller_clause> add <rest_style_urls>
```

<small>**rest_style_urls** can typed with `{?}` or `?` in PowerShell to identify it as dynamic PHP GET parameters.</small>

For example:

```text
php puko routes view add member/?/reports
```

Items asked for the rest of process:

|Items|Description|Examples|
|---|---|---|
|Controller name|File name. You can use \ to place the file in sub-directories|member\reports|
|Function name|Function name|pages|
|Accept?|HTTP verb GET,POST,PUT,PATCH,DELETE multiple by commas|get,post|

<small>Importand: While rotes using / _slash_, You must using \ _backslash_ for separating controller to prevent directory path error on diferent type of Operating Systems.</small>

## File generated after New Routing

After completed, puko is updating and auto-generated these files:

```bash
config/routes.php
controller/member/reports.php
```

If the routes clause was `view` puko will also update and auto-generated these files:

```bash
assets/html/en/member/reports/pages.html
assets/html/id/member/reports/pages.html
```

You can also see all registered routes with this command:

```bash
php puko routes list
```

<small>All generated routes stored in `config/routes.php`. So it's important to not manually edit those file to maintain Routing structure.</small>

## Update Routing

If you want to update or modify the HTTP verb accepted from the Routing url registered, 
you can update it with command:

```bash
php puko routes view update member/?/reports
```

Apply your revision when answering items asked.

## Delete Routing

Puko also can remove unused routes with remove command:

```bash
php puko routes view remove member/{?}/reports
```

<small>For security concerns puko not deleting the `.php` and `.html` file. You must review and remove it manually.</small>
