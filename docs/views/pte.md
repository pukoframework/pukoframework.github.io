---
layout: default
title: Template Engine
parent: Views
nav_order: 1
---

# Template Engine
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The **Puko Template Engine (PTE)** is the standard rendering engine used by the Puko Framework to process and display HTML. It uses a specific set of tags to inject data, handle loops, and manage assets.

### Available Tags

| Tag | Description |
| :--- | :--- |
| `{!x}` | Standard tag for printing values, data, or elements. |
| `<!--{!x}-->` | **Opening** tag for loops. |
| `<!--{/x}-->` | **Closing** tag for loops. |
| `{!fn()}` | Function tag with no parameters. |
| `{!fn(x)}` | Function tag with a single parameter. |
| `{!fn(x,y,z)}` | Function tag with multiple parameters. |
| `{CONTENT}` | Injects the content HTML file into the master layout. |
| `{!css(<link ... />)}` | Defines a **CSS** resource in a content file. |
| `{!js(<script ... ></script>)}` | Defines a **JavaScript** resource in a content file. |
| `{!part(css)}` | Moves defined CSS from the content file to this location in the master layout. |
| `{!part(js)}` | Moves defined JavaScript from the content file to this location in the master layout. |
| `{x.html}` | Loads a specific HTML segment file. |

<small>**Note:** For more information on using **Elements**, please refer to the [Elements]({{ site.baseurl }}{% link docs/views/elements.md %}) documentation.</small>

### Basic Usage

To display a value in your HTML:

```html
<span>{!score}</span>
```

In your controller, assign the corresponding value to the data array:

```php
public function student() {
    $data['score'] = 95;
    return $data;
}
```

The resulting HTML will be:

```html
<span>95</span>
```

### Looping through Data

To render a list of items, use the loop tags:

```html
<table>
    <!--{!students}-->
    <tr>
        <td>{!name}</td>
        <td>{!address}</td>
        <td>{!age}</td>
    </tr>
    <!--{/students}-->
</table>
```

In your controller, provide an array of data:

```php
public function list() {
    $data['students'] = [
        [
            'name'    => 'Didit Velliz',
            'address' => 'Bandung',
            'age'     => 26
        ],
        [
            'name'    => 'Puko Framework',
            'address' => 'Jakarta',
            'age'     => 18
        ]
    ];
    return $data;
}
```

### Managing Assets

You can define CSS and JavaScript requirements directly in your content HTML files:

```html
{!css(<link href="assets/css/bootstrap.min.css" rel="stylesheet" type="text/css"/>)}

{!js(<script src="assets/js/jquery.dataTables.min.js"></script>)}
{!js(<script src="assets/js/dataTables.bootstrap.min.js"></script>)}
```

To prevent an asset from being rendered, you can wrap it in an HTML comment within the tag: `{!js(<!--<script src="..."></script>-->)}`.

Ensure your **Master Layout** includes the `{CONTENT}` tag and the `{!part()}` tags to properly inject these assets:

```html
<html>
    <head>
        {!part(css)}
    </head>
    <body>
        <div class="container">
            {CONTENT}
        </div>
        {!part(js)}
    </body>
</html>
```

### Helper Functions

To retrieve the application's **base URL** within an HTML file, use the following syntax:

```html
<a href="{!url()}user/profile">Profile</a>
```
