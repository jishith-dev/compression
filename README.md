# Compression

A simple file compression and archive library for ZEN.

**Version:** 1.0.0

## Features

- GZIP compression
- GZIP decompression
- ZIP archive creation
- ZIP archive extraction
- Single-file and multi-file ZIP archives
- Native file-based API
- Simple public interface

---

## Installation

Install the package using the ZEN package manager:

```bash
zen install compression
```

---

## Import

Import the `Compression` API:

```zen
import (Compression) from "compression"
```

Create a `Compression` instance:

```zen
Compression c
```

---

# API

## `gzip()`

Compresses a single file using GZIP.

```zen
c.gzip(source, destination)
```

### Parameters

| Parameter | Type | Description |
|---|---|---|
| `source` | `string` | Path to the input file |
| `destination` | `string` | Path where the compressed `.gz` file will be written |

### Returns

`bool`

Returns `true` when the operation succeeds.

### Example

```zen
import (Compression) from "compression"

Compression c

bool result = c.gzip("data.txt", "data.txt.gz")

screen(result)
```

This produces:

```text
data.txt
    ↓
data.txt.gz
```

---

## `gunzip()`

Decompresses a GZIP file back into a regular file.

```zen
c.gunzip(source, destination)
```

### Parameters

| Parameter | Type | Description |
|---|---|---|
| `source` | `string` | Path to the `.gz` file |
| `destination` | `string` | Path where the decompressed file will be written |

### Returns

`bool`

Returns `true` when the operation succeeds.

### Example

```zen
import (Compression) from "compression"

Compression c

bool result = c.gunzip("data.txt.gz", "data_restored.txt")

screen(result)
```

This reverses the GZIP operation:

```text
data.txt.gz
    ↓
data_restored.txt
```

The restored file contains the original data.

---

# GZIP Example

A complete compression and decompression example:

```zen
import (Compression) from "compression"

Compression c

fs.writeFile(
    "hello.txt",
    "Hello from Zen compression!"
)

bool compressed = c.gzip(
    "hello.txt",
    "hello.txt.gz"
)

screen(compressed)

bool restored = c.gunzip(
    "hello.txt.gz",
    "hello_restored.txt"
)

screen(restored)
```

Resulting files:

```text
hello.txt
hello.txt.gz
hello_restored.txt
```

The original and restored files can be compared to verify that the data was preserved.

---

# `zip()`

Creates a ZIP archive from one or more files.

```zen
c.zip(files, destination)
```

### Parameters

| Parameter | Type | Description |
|---|---|---|
| `files` | `List<string>` | List of files to add to the archive |
| `destination` | `string` | Path where the `.zip` archive will be written |

### Returns

`bool`

Returns `true` when the operation succeeds.

### Single File Example

```zen
import (Compression) from "compression"

Compression c

List<string> files = [
    "hello.txt"
]

bool result = c.zip(
    files,
    "archive.zip"
)

screen(result)
```

The resulting archive contains:

```text
archive.zip
└── hello.txt
```

---

## Multiple Files

`zip()` can package multiple files into a single archive.

```zen
import (Compression) from "compression"

Compression c

List<string> files = [
    "index.html",
    "style.css",
    "app.js",
    "README.md"
]

bool result = c.zip(
    files,
    "project.zip"
)

screen(result)
```

The resulting archive contains:

```text
project.zip
├── index.html
├── style.css
├── app.js
└── README.md
```

ZIP is useful when several files need to be stored or transferred together.

---

# `unzip()`

Extracts the files contained in a ZIP archive.

```zen
c.unzip(source, destination)
```

### Parameters

| Parameter | Type | Description |
|---|---|---|
| `source` | `string` | Path to the `.zip` archive |
| `destination` | `string` | Directory where the files will be extracted |

### Returns

`bool`

Returns `true` when the operation succeeds.

### Example

```zen
import (Compression) from "compression"

Compression c

bool result = c.unzip(
    "project.zip",
    "project"
)

screen(result)
```

If the archive contains:

```text
index.html
style.css
app.js
README.md
```

the destination becomes:

```text
project/
├── index.html
├── style.css
├── app.js
└── README.md
```

---

# ZIP Example

Create an archive and extract it again:

```zen
import (Compression) from "compression"

Compression c

List<string> files = [
    "hello.txt",
    "data.txt",
    "README.md"
]

bool compressed = c.zip(
    files,
    "archive.zip"
)

screen(compressed)

bool restored = c.unzip(
    "archive.zip",
    "output"
)

screen(restored)
```

The flow is:

```text
hello.txt ─┐
data.txt  ─┼──→ zip() ──→ archive.zip
README.md ─┘

archive.zip
     │
     ↓
  unzip()
     │
     ↓
output/
├── hello.txt
├── data.txt
└── README.md
```

---

# GZIP vs ZIP

The two formats serve slightly different purposes.

## GZIP

GZIP is intended for compressing a single file:

```text
file.txt
   ↓
gzip()
   ↓
file.txt.gz
```

Use GZIP when you mainly need to reduce the size of one file.

Common uses include:

- Log files
- Text files
- Generated data
- Large single files
- Network data compression
- Storage reduction

To restore the file:

```text
file.txt.gz
   ↓
gunzip()
   ↓
file.txt
```

## ZIP

ZIP is intended for creating an archive containing one or more files:

```text
file1.txt ─┐
file2.txt ─┼──→ zip() ──→ archive.zip
file3.txt ─┘
```

Use ZIP when you want to bundle multiple files together.

Common uses include:

- Project archives
- Backups
- File bundles
- Distributing multiple files
- Packaging resources
- Transferring collections of files

To extract the archive:

```text
archive.zip
     ↓
  unzip()
     ↓
multiple files
```

---

# Complete API Example

```zen
import (Compression) from "compression"

Compression c

screen("=== Compression Example ===")

/* GZIP */

fs.writeFile(
    "data.txt",
    "Hello from Zen!"
)

bool gz = c.gzip(
    "data.txt",
    "data.txt.gz"
)

screen("gzip():")
screen(gz)

bool ungz = c.gunzip(
    "data.txt.gz",
    "data_restored.txt"
)

screen("gunzip():")
screen(ungz)

/* ZIP */

List<string> files = [
    "data.txt",
    "data_restored.txt"
]

bool z = c.zip(
    files,
    "archive.zip"
)

screen("zip():")
screen(z)

/* UNZIP */

bool uz = c.unzip(
    "archive.zip",
    "unzipped"
)

screen("unzip():")
screen(uz)

screen("=== Done ===")
```

---

# Verification

Compression should preserve the original data after decompression.

For GZIP:

```text
original file
     ↓
   gzip()
     ↓
  .gz file
     ↓
  gunzip()
     ↓
restored file
```

The original and restored files should contain identical data.

For ZIP:

```text
original files
      ↓
    zip()
      ↓
  .zip archive
      ↓
   unzip()
      ↓
restored files
```

The extracted files should contain the same data as the original files.

For example, from a shell:

```bash
cmp data.txt data_restored.txt
```

A successful comparison means the files are identical.

---

# API Summary

| API | Purpose | Input | Output |
|---|---|---|---|
| `gzip()` | Compress one file | Source file | `.gz` file |
| `gunzip()` | Decompress GZIP | `.gz` file | Original file |
| `zip()` | Create ZIP archive | File list | `.zip` archive |
| `unzip()` | Extract ZIP archive | `.zip` archive | Extracted files |

---

# Design

Compression v1 provides a small public API around two standard compression/archive formats:

```text
GZIP
├── gzip()
└── gunzip()

ZIP
├── zip()
└── unzip()
```

The API is intentionally simple so that common file-compression operations require only a source and destination path.

---

# Version

**Compression v1.0.0**

Public APIs:

```text
gzip()
gunzip()
zip()
unzip()
```

---

# License

MIT License
