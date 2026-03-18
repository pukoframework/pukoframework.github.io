---
layout: default
title: jQuery DataTables
parent: Utilities
nav_order: 2
---

# jQuery DataTables
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

DataTables is the most popular jQuery plugin used for displaying data in tabular format. The Puko Framework provides libraries that simplify the integration of DataTables, particularly when implementing **Server-side Processing**.

### PHP Implementation Guide

To get started, create an instance of the `DataTables` class using the following syntax:

```php
$table = new DataTables(DataTables::POST);
```

Next, define your column specifications, specifically the column names as they appear in your database selection:

```php
$table->SetColumnSpec([
    "name",
    "age",
    "address",
    "email"
]);
```

Provide the SQL query that will be processed by the DataTables class:

```php
$table->SetQuery("SELECT * FROM students;");
```

Finally, return the processed data using the `GetDataTables` method. This method accepts an anonymous function (callback) allowing for data transformation before it is sent to the client.

```php
return $table->GetDataTables(function ($result) {
    foreach ($result as $key => &$val) {
        // You can perform data modifications or transformations here
    }
    return $result;
});
```

### JavaScript Implementation Guide

Ensure that you have included the required DataTables JavaScript files in your HTML. Using Puko's PTE syntax:

```html
{!js(<script src="assets/js/jquery.dataTables.min.js"></script>)}
{!js(<script src="assets/js/dataTables.bootstrap.min.js"></script>)}
```

<small>*Note: It is highly recommended to use DataTables version 1.10 or later.*</small>

Verify that your master template includes the necessary placeholders for dynamic assets:

```html
<html>
    <head>...</head>
    <body>
    ...
    {CONTENT}
    ...
    {!part(css)}
    {!part(js)}
    </body>
</html>
```

Initialize the DataTable in your JavaScript code as shown below:

```javascript
let table = $('#example').DataTable({
    ajax: {
        type: "POST",
        dataType: "json",
        responsive: true,
        url: $('base#api').attr('href') + "backend/api/endpoint",
        data: function (data) {
            // Additional parameters to send to the server
        },
        error: function (jqXHR, textStatus, errorThrown) {
            console.error(errorThrown);
        },
        dataSrc: function (json) {
            if (json.exception !== undefined) {
                return [];
            }
            return json.data;
        }
    },
    processing: true,
    serverSide: true,
    lengthMenu: [
        [10, 50, 100, 250, 350, 500],
        ['10', '50', '100', '250', '350', '500']
    ],
    dom: 'Bfrtip',
    buttons: [
        {
            extend: "pageLength",
            className: "btn-sm"
        },
        {
            extend: "copy",
            className: "btn-sm"
        },
        {
            extend: "csv",
            className: "btn-sm"
        },
        {
            extend: "excel",
            className: "btn-sm"
        },
        {
            extend: "pdfHtml5",
            className: "btn-sm"
        },
        {
            extend: "print",
            className: "btn-sm"
        },
        {
            text: 'Custom Button',
            className: "btn-sm",
            action: function ( e, dt, node, config ) {
                // Custom action logic
            }
        }
    ],
    initComplete: function (settings, json) {
        // Callback after table initialization
    },
    rowCallback: function (row, data) {
        // Callback for each row rendering
    },
});
```

<small>**Important:** The number and order of columns in your JavaScript initialization must match the `SetColumnSpec` definition in your PHP code.</small>
