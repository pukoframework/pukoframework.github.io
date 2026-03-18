---
layout: default
title: File Uploads
parent: Utilities
nav_order: 3
---

# File Uploads
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The Puko Framework provides built-in libraries to simplify the file upload process.

### HTML Implementation

To upload files, ensure your HTML form uses the `POST` method and the `enctype="multipart/form-data"` attribute:

```html
<form action="/upload" method="POST" enctype="multipart/form-data">
    <input type="file" name="filedata" />
    <button type="submit">Upload</button>
</form>
```

---

### Backend Processing

In your controller, you can handle file uploads using the `Request::Files` utility.

#### Basic Retrieval

```php
$file = Request::Files('filedata', null, true);
```

*   **First Parameter:** The expected key name from the HTTP `$_FILES` array (e.g., `filedata`).
*   **Second Parameter:** A default fallback value if the file is not found (e.g., `null`).
*   **Third Parameter:** A boolean value. If `true`, the utility transforms the file data into a PHP object; if `false`, it returns the default array structure.

#### Validation

You can easily add validations using the `File` object's methods:

```php
if ($file === null) {
    throw new Exception("File is required.");
}

// Check if the file size is within limits (e.g., 15MB)
if (!$file->isSizeSmallerThan(15 * Files::MB)) {
    throw new Exception("The file is too large. Maximum size is 15MB.");
}

// Check for native PHP upload errors
if ($file->isError()) {
    throw new Exception("An error occurred during the file upload process.");
}
```

### Available Methods

The following methods are available for managing uploaded files:

*   `getName()`: Retrieves the original file name.
*   `getType()`: Retrieves the MIME type of the file.
*   `getTmpName()`: Retrieves the temporary server path of the uploaded file.
*   `isError()`: Returns `true` if a native PHP upload error occurred.
*   `getSize()`: Retrieves the file size in kilobytes (kB).
*   `isSizeSmallerThan(float $limit)`: Validates that the file is smaller than the specified limit (default: 10MB).
*   `getFile()`: Retrieves the binary content or the temporary path for further processing.

### Saving the File

#### To a Database

```php
$model = new \plugins\model\data();

// In MySQL, ensure the column (e.g., 'filedata') is set to 'LONGBLOB'
$model->filedata = $file->getFile();
$model->save();
```

#### To the Server Directory

```php
$targetDir = Framework::$factory->getRoot() . '/uploads/listing';

if (!is_dir($targetDir)) {
    mkdir($targetDir, 0755, true);
}

$uploadPath = $targetDir . '/' . $file->getName();

if (move_uploaded_file($file->getTmpName(), $uploadPath)) {
    // File moved successfully
}
```
