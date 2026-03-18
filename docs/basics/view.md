---
layout: default
title: View
parent: Basics
nav_order: 3
---

# View
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The **View** in the Puko Framework is composed of a master layout and hierarchical content, generated automatically when you run the command `php puko routes view add ...`. A key principle in Puko is that the view layer does not contain business logic. Its role is solely to present data, including rendering lists using loop tags.

<small>*Note: As the view layer in HMVC, its primary responsibility is **Presenting Data**. For advanced logic or animations, you can utilize the full power of JavaScript within your views.*</small>

### The Master Layout

When Puko is installed with Composer, you receive a `master.html` file as your starting point:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{!title}</title>
    <style>
        body {
            background-color: #039BE5;
            color: white;
        }
    </style>
</head>
<body>
<div>
    {CONTENT}
</div>
<br>
<br>
{!part(css)}
{!part(js)}
</body>
</html>
```

<small>*Puko utilizes pure HTML for its View layer, ensuring the file structure remains clean and is not disrupted by PHP syntax as it grows.*</small>

Puko uses a template syntax known as **PTE** tags. For more information, please see the [official PTE project documentation](https://github.com/Velliz/pte).

#### Common PTE Tags

*   `{!title}`: Renders atomic data (in this case, the value of the `title` variable).
*   `{CONTENT}`: Dynamically loads the content HTML file at runtime.
*   `{!part(css)}`: Places CSS files declared in the content HTML file.
*   `{!part(js)}`: Places JavaScript files declared in the content HTML file.

You can have multiple master HTML files (e.g., `master.html`, `master-admin.html`, `master-guest.html`, `master-owner.html`). This is particularly useful for providing different layouts for different user roles.

---

### Hierarchical Content

Content is organized in a hierarchical structure synchronized with your routing definitions. This enforces the HMVC pattern across your application.

Example: `php puko routes view add accounts/user/{?}/links`

*   **Controller:** `accounts\user`
*   **Function:** `links`
*   **Accept:** `GET`, `POST`

#### Corresponding Files

*   **HTML File:** `assets/html/id/accounts/user/links.html`
*   **Controller File:** `controller/accounts/user.php`

### Specifying a Master Layout

If you have multiple master layouts, you can switch between them using a **Doc Tag** in your controller:

```php
/**
 * #Master master-admin.html
 */
class user extends View {
    public function links() {
        // ...
    }
}
```

<small>*Note: if the **#Master** tag is not declared, Puko will automatically use the default **master.html** file.*</small>
