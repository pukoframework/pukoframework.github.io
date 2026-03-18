---
layout: default
title: System Class
parent: Basics
nav_order: 9
---

# System Class
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

### Global States

The Puko Framework provides built-in templates and controllers for managing various system-wide error states.

#### Error Display

The template for this page is located at `assets/html/id/error/display.html`, and the corresponding controller function is `controller/error.php` at `display()`. This is triggered when the HTTP `ACCEPT` header does not match the verb opened from the web browser (e.g., if a route is registered with `POST` and accessed with `GET`).

#### Error Maintenance

This page is displayed when the `environment` index variable is set to `MAINTENANCE`. This takes the website offline and shows the "Under Maintenance" information on the screen. Customize the maintenance page at `assets/html/id/error/maintenance.html` and the controller file `controller/error.php` at `maintenance()`.

#### Error Not-Found

This page is displayed when a non-existent route is accessed. Customize the "Not Found" page at `assets/html/id/error/notfound.html` and the controller file `controller/error.php` at `notfound()`.

<small>*Note: These pages are displayed if the error controller class `error.php` extends the View middleware.*</small>

---

### Function & Doc Command States

The following error states are triggered by specific **Doc Tags** and system exceptions.

#### Not Authenticated

Triggered when a function is called with the doc tag: `#Auth bearer true` and the user is not authenticated. Customize the "Not Authenticated" page at `system/auth.html`. (No controller file is available for this.)

#### Under Construction

Triggered when a function is called with the doc tag: `#UnderConstruction true`. Customize the "Under Construction" page at `system/construction.html`. (No controller file is available for this.)

#### General Error

This error occurs when an internal error is triggered within a controller function, such as an undefined variable. Customize the error page at `system/error.html`. (No controller file is available for this.)

#### Exception

This error occurs when an `Exception` is thrown. Customize the "Exception" page at `system/exception.html`. (No controller file is available for this.)

#### Not Authorized for Permission

This error occurs when a user does not have the required permission defined by a `#Permission` tag. Customize the "Permission Denied" page at `system/permission.html`. (No controller file is available for this.)

<small>*Note: All HTML templates are only available when the controller extends the View middleware.*</small>
