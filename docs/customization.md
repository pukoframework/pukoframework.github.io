---
layout: default
title: Customization
nav_order: 10
---

# Customization
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The Puko Framework documentation is built using **Just the Docs**, a flexible Jekyll theme. You can customize the look and feel of your documentation site to match your project's branding.

## Color Schemes

Just the Docs supports light and dark color schemes by default. You can switch between them in your `_config.yml` file:

```yaml
# Supported values: "light" (default), "dark", or a custom scheme
color_scheme: dark
```

## Custom Color Schemes

To define a custom color scheme, create a new SCSS file in `_sass/color_schemes/` (e.g., `_sass/color_schemes/my-scheme.scss`) and define the required color variables.

### Example Scheme Definition:

```scss
$link-color: #039BE5;
$btn-primary-color: #039BE5;
$feedback-color: #039BE5;
// ... add more overrides here
```

Once defined, reference your scheme in `_config.yml`:

```yaml
color_scheme: my-scheme
```

## Overriding Styles

If you need to make specific CSS changes without defining a whole new scheme, you can use the custom SCSS file located at `_sass/custom/custom.scss`.

Any styles added to this file will automatically override the theme's default styling.

### Example Overrides:

```scss
// Customizing the sidebar width
.side-bar {
  width: 300px;
}

// Customizing code block backgrounds
code {
  background-color: #f5f5f5;
  border-radius: 4px;
}
```

## Custom Layouts and Includes

For deeper structural customization, you can override the theme's default layouts or includes by creating files with the same names in your project's `_layouts/` or `_includes/` directories.

Common files to override:
*   **_includes/head_custom.html**: Add custom meta tags, fonts (e.g., Google Fonts), or external scripts.
*   **_includes/footer_custom.html**: Add custom footer links or copyright information.
