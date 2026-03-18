---
layout: default
title: Pagination
parent: Utilities
nav_order: 5
---

# Pagination
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The Puko Framework provides a simple way to paginate large datasets, which can also be combined with search queries.

### API Requirements

When requesting paginated data, the client must include the following query string parameters:

*   **page**: The current page number (starting from 1).
*   **length**: The number of records to display per page.

Example URL: `http://localhost/api/test?page=1&length=10`

### PHP Implementation Guide

To implement pagination in your controller, use the `Paginations` utility class:

```php
$length = 10;
$sql = "SELECT * FROM students WHERE status = 'active'";

$paginate = new Paginations();
$paginate->SetLength($length);
$paginate->SetQuery($sql);

return $paginate->GetDataPaginations(function ($result) {
    // Optional: Perform data transformations for each record
    foreach ($result as $key => &$val) {
        // Your logic here
    }
    return $result;
});
```

### JSON Response Structure

The `GetDataPaginations` method returns a standardized JSON object that is easy to consume on the frontend:

```json
{
    "page": 1,
    "totalpage": 2,
    "length": 10,
    "displayed": 10,
    "anchor": [
        {
            "page": 1
        },
        {
            "page": 2
        }
    ],
    "totaldata": 15,
    "data": [
        {
            "id": 1,
            "name": "Student Name"
        }
        // ... additional data
    ]
}
```

#### Field Descriptions:

*   **page**: The current page number.
*   **totalpage**: The total number of available pages.
*   **length**: The requested number of records per page.
*   **displayed**: The actual number of records returned for the current page.
*   **anchor**: A list of objects representing available page numbers (useful for building pagination UI).
*   **totaldata**: The total number of records matching the query in the database.
*   **data**: An array of objects containing the requested record data.
