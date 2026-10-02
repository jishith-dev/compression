# compression

**Author:** Jishith M P
**Version:** 2.0.0

gzip, zip, zlib and deflate compression for Zen. Pure Zen, no dependencies.

## Import

```zen
import (Compression) from "compression"

Compression c
```

## Quick start

```zen
/* compress a file */
c.gzip("notes.txt", "notes.txt.gz")

/* decompress it */
c.gunzip("notes.txt.gz", "notes_copy.txt")

/* zip some files */
List<string> files = ["a.txt", "b.txt"]
c.zip(files, "backup.zip")

/* unzip into a folder */
c.unzip("backup.zip", "output")
```

Every function returns `true` on success and `false` on failure.
On failure it also prints a message starting with `compression:`.

## Files

| Function | What it does |
|---|---|
| `gzip(source, destination, level = 6)` | Compress a file to `.gz` |
| `gunzip(source, destination)` | Decompress a `.gz` file |
| `zip(sources, destination, level = 6)` | Put a list of files into a `.zip` |
| `zipAs(sources, names, destination, level = 6)` | Same as `zip`, but you choose the names stored inside |
| `unzip(source, destination)` | Extract a `.zip` into a folder |

## Bytes (no files)

| Function | What it does |
|---|---|
| `gzipBytes(data, level)` | Returns gzip data as `List<byte>` |
| `gunzipBytes(data, output)` | Fills `output`, returns `bool` |
| `zlibBytes(data, level)` | Returns zlib data as `List<byte>` |
| `unzlibBytes(data, output)` | Fills `output`, returns `bool` |
| `deflateBytes(data, level)` | Returns raw deflate data as `List<byte>` |
| `inflateBytes(data, output)` | Fills `output`, returns `bool` |

```zen
List<byte> data = stringToBytes("hello zen hello zen hello zen")

List<byte> packed = c.gzipBytes(data, 6)

List<byte> back = []
bool ok = c.gunzipBytes(packed, back)

screen(bytesToString(back))
```

## Checksums

| Function | What it does |
|---|---|
| `crc32(data)` | Returns the CRC-32 as `long` |
| `adler32(data)` | Returns the Adler-32 as `long` |

## Levels

| Level | Meaning |
|---|---|
| `0` | Store only (no compression, fastest) |
| `1` | Fast |
| `6` | Default, good balance |
| `9` | Smallest output, slowest |

Values outside 0–9 are clamped.

## Custom names in a zip

`zip` uses the file path as the name inside the archive.
Use `zipAs` to choose your own:

```zen
List<string> files = ["/home/me/report.txt"]
List<string> names = ["report.txt"]
c.zipAs(files, names, "report.zip")
```

## Safety

- Checks CRC and size on everything it reads, so corrupt files are rejected.
- `unzip` refuses paths like `../evil.txt` and absolute paths.
- Output size is limited while decompressing, to stop decompression bombs.
- Encrypted zip entries are not supported.

## Limits

- No ZIP64: no file or archive over 4 GB, and at most 65535 files per zip.
- zlib preset dictionaries are not supported.
- The whole file is loaded into memory while it is processed.

## Compatibility

Files made by this package open with standard `gzip`, `unzip`, and zlib tools.
It can also read files made by them.
