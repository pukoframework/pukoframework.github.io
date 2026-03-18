---
layout: default
title: Elements
parent: Views
nav_order: 1
---

# Elements
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

**Elements** is a feature set originally from **PTE** (the template engine used by the Puko Framework). Elements allow you to create standalone, modular, and reusable view segments. Each element is a self-contained package consisting of HTML, JavaScript, CSS, and a PHP logic file.

### Managing Elements via CLI

#### Downloading Elements

You can download existing elements from the [official elements repository](https://github.com/Velliz/elements) using the following command:
 
```bash
php puko element download <element_name>
```

Example:

```bash
php puko element download adminlte_description
```

#### Creating New Elements

To create a new, custom element, use the `add` command:

```bash
php puko element add <element_name>
```

<small>**Important:** Element names must not contain spaces or start with numbers. It is recommended to use alphanumeric characters and underscores `[a-z_A-Z]` only.</small>

### Element Structure

Once an element is created or downloaded, it is stored in the `plugins/elements` directory with the following structure:

```text
- plugins/
  - elements/
    - <element_name>/
      - <element_name>.php   (PHP Logic)
      - <element_name>.html  (HTML Template)
      - <element_name>.js    (JavaScript Assets)
      - <element_name>.css   (CSS Styles)
```

### Implementation Example

After downloading or creating an element, you can instantiate it within your controller.

**Controller Implementation:**

```php
public function profile() 
{
    // Instantiate the element class
    $desc = new AdminLTE_Description('desc', []);
    
    // Set element-specific configurations
    $desc->SetStyle(AdminLTE_Description::HORIZONTAL);
    $desc->SetDescription([
        [
            'Title' => 'Didit Velliz',
            'Text'  => 'Programmer',
        ],
        [
            'Title' => 'Lois',
            'Text'  => 'Gamer',
        ],
        [
            'Title' => 'Christian',
            'Text'  => 'Lead Developer',
        ]
    ]);

    // Pass the element object to the view data
    $data['desc'] = $desc;
    
    return $data;
}
```

**HTML Template Implementation:**

To render the element in your HTML file, use the corresponding PTE tag:

```html
<div class="profile-section">
    {!desc}
</div>
```
