# OPFS.js - Simplified Access to Origin Private File System (OPFS)
> Reference: [MDN: Origin Private File System](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API/Origin_private_file_system)

## Overview

**OPFS.js** is a JavaScript library that simplifies file and directory interactions with the browser's **Origin Private File System (OPFS)**. OPFS provides fast, persistent, and secure file storage directly in the browser.

This library is created to reduce boilerplate and complexity around OPFS API usage by offering an easy-to-use, promise-based interface.

### Why OPFS?

Compared to traditional browser storage options like IndexedDB, OPFS is significantly faster and more efficient for binary data and file operations:

* **IndexedDB**: General-purpose key-value database. Slower for file storage.
* **OPFS**: Purpose-built for file handling with low-level access, supporting streaming, permissions, and quota management.

---

## What Makes OPFS.js Special for Persistent Storage?

OPFS is designed for use cases where you need **truly persistent, local file storage** within the browser—ideal for applications like:

- Image editors, drawing apps, or games that store large files offline
- Web-based IDEs or note-taking tools
- Offline-first PWAs and local backups
- Client-side database exports or media

Unlike `localStorage`, `sessionStorage`, or `IndexedDB`, OPFS persists even across browser restarts and supports advanced features like true file handles, stream APIs, and large binary file support—making it ideal for storing and accessing files like images, audio, documents, or any data-heavy content.
**Web Worker compatible** — This library fully supports usage inside Web Workers (without relying on `FileSystemSyncAccessHandle`), enabling efficient background file operations for modern web applications.

OPFS data is stored in a persistent quota-managed sandbox that survives page reloads and browser restarts.
this library wraps navigator.storage.getDirectory() correctly, which gives access to this persistent storage.

OPFS.js makes this power usable with minimal code.

---

## Features

* **Async/Await API**: Clean, modern promise-based structure.
* **File Operations**: `readFile`, `writeFile`, `appendFile`, `deleteFile`, `renameFile`, `copyFile`, `fileExists`
* **Directory Operations**: `readDir`, `listFiles`, `listDirectories`, `readAllFiles`, `readAllDirectories`, `createDirectory`, `deleteDirectory`, `renameDirectory`, `dirExists`
* **Path Utilities**: `resolvePath`, `joinPath`, `splitPath`, `getParentDirectory`
* **Permissions & Metadata**: `ensurePermission`, `getFileMeta`, `isReadable`, `isWritable`
* **Streaming Support**: `streamFile`, `writeStream`, `readFileChunk`
* **Import/Export**: `importFile`, `exportFile`
* **Storage Management**: `getQuota`, `clearOPFS`
* **Debugging**: `debugTree`
* **FileData Wrapper**: `.toBlob()`, `.toText()`, `.toJSON()`, `.toArrayBuffer()`, `.toDataURL()`

---

### ✅ Comparison with `localStorage`, `sessionStorage`, and `IndexedDB`

| Feature                | `localStorage` | `IndexedDB`         | `OPFS (via OPFS.js)`     |
|------------------------|----------------|----------------------|---------------------------|
| Capacity               | ~5MB           | GBs (slower)         | GBs (fast)                |
| Binary Support         | ❌              | ✅ (but complex)     | ✅ (stream + buffer)      |
| File Hierarchy         | ❌              | ❌                   | ✅                        |
| Stream API             | ❌              | ❌                   | ✅                        |
| Random Access          | ❌              | Limited              | ✅                        |
| Persist after restart  | ✅              | ✅                   | ✅                        |
| Simplicity             | ✅              | ❌                   | ✅                        |

---

## Browser & OS Support for Origin Private File System (OPFS)

| Browser                 | Operating System     | OPFS Support     | Notes                                                                                          |
|-------------------------|----------------------|------------------|------------------------------------------------------------------------------------------------|
| **Chrome**              | Windows, macOS, Linux, Android | ✅ Supported     | Full support since version 86+.                                                                |
| **Edge (Chromium-based)**| Windows, macOS       | ✅ Supported     | Same as Chrome, fully supported.                                                               |
| **Safari**              | macOS 12.2+, iOS 15.2+| ✅ Supported     | Full support starting macOS 12.2+ and iOS 15.2+.                                               |
| **Safari**              | macOS < 12.2, iOS < 15.2 | ❌ Not Supported | OPFS is not supported on older OS versions.                                                    |
| **Safari (Private Browsing)** | macOS & iOS      | ⚠️ Not Available | OPFS is not available in Safari’s Private Browsing mode.                                       |
| **Firefox**             | Windows, macOS, Linux | ⚠️ Partial Support | Available in Firefox 111+, but support is still incomplete compared to Chromium browsers.      |
| **Internet Explorer**   | All OSes              | ❌ Not Supported | Legacy browser; does not support modern APIs like OPFS.                                        |

### Notes
- **Private Browsing**: OPFS is generally unavailable in private/incognito modes across all browsers.
- **Web Workers**: Some features like `FileSystemSyncAccessHandle` are only available in Web Workers.
- **Quota Limits**: OPFS is subject to browser-imposed storage quotas. Handle `QuotaExceededError` accordingly.
- **Reference**:
  - [MDN Docs on OPFS](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API/Origin_private_file_system)
  - [WebKit Blog](https://webkit.org/blog/12257/the-file-system-access-api-with-origin-private-file-system/)
  - [Chrome Status](https://chromestatus.com/feature/5079634203377664)
  - [RXDB on OPFS](https://rxdb.info/rx-storage-opfs.html)

---

## Quick Usage

## Installation & Import

### CDN (Browser)
```html
<script src="https://cdn.jsdelivr.net/npm/opfs@1.0.0/opfs.js"></script>
or
<script src="https://unpkg.com/opfs@1.0.0/opfs.js"></script>

<script src="https://cdn.jsdelivr.net/npm/opfs@1.0.0/opfs.min.js"></script>
or
<script src="https://unpkg.com/opfs@1.0.0/opfs.min.js"></script>
```

### NPM
```bash
npm install opfs.js
```
```js
import OPFS from 'opfs.js';
```

### Require (CommonJS)
```js
const OPFS = require('opfs.js');
```

### ES Module
```html
<script type="module">
  import OPFS from 'https://cdn.jsdelivr.net/npm/opfs.js/+esm';
</script>
```

---

## Simplified Examples

```javascript
const opfs = OPFS();

await opfs.writeFile('hello.txt', 'Hello World');
const fileData = await opfs.readFile('hello.txt');
console.log(await fileData.toText()); // "Hello World"

await opfs.appendFile('hello.txt', '\nMore');
await opfs.renameFile('hello.txt', 'greeting.txt');
await opfs.copyFile('greeting.txt', 'copy.txt');

const exists = await opfs.fileExists('copy.txt');
console.log(exists); // true

const files = await opfs.listFiles('.');
console.log(files); // ['greeting.txt', 'copy.txt']
```

---

## All Methods

| Method               | Parameters           | Returns                                | Example                                            | Description                               |
| -------------------- | -------------------- | -------------------------------------- | -------------------------------------------------- | ----------------------------------------- |
| `writeFile`          | `(path, content)`    | `Promise<void>`                        | `await opfs.writeFile('a.txt', 'text')`            | Writes string or Blob to a file           |
| `readFile`           | `(path)`             | `Promise<FileData>`                    | `let data = await opfs.readFile('a.txt')`          | Reads a file and returns FileData wrapper |
| `appendFile`         | `(path, content)`    | `Promise<void>`                        | `await opfs.appendFile('a.txt', 'text')`           | Appends content to a file                 |
| `deleteFile`         | `(path)`             | `Promise<void>`                        | `await opfs.deleteFile('a.txt')`                   | Deletes a file                            |
| `renameFile`         | `(oldPath, newPath)` | `Promise<void>`                        | `await opfs.renameFile('old.txt', 'new.txt')`      | Renames a file                            |
| `copyFile`           | `(src, dest)`        | `Promise<void>`                        | `await opfs.copyFile('a.txt', 'b.txt')`            | Copies a file                             |
| `fileExists`         | `(path)`             | `Promise<boolean>`                     | `await opfs.fileExists('a.txt')`                   | Checks if a file exists                   |
| `readDir`            | `(dirPath)`          | `Promise<Array<FileMeta>>`             | `await opfs.readDir('myDir')`                      | Lists files with metadata                 |
| `listFiles`          | `(dirPath)`          | `Promise<Array<string>>`               | `await opfs.listFiles('.')`                        | Lists filenames only                      |
| `listDirectories`    | `(dirPath)`          | `Promise<Array<string>>`               | `await opfs.listDirectories('.')`                  | Lists subdirectory names                  |
| `readAllFiles`       | `(dirPath)`          | `Promise<Array<string>>`               | `await opfs.readAllFiles('.')`                     | Recursively lists all file paths          |
| `readAllDirectories` | `(dirPath)`          | `Promise<Array<string>>`               | `await opfs.readAllDirectories('.')`               | Recursively lists all directories         |
| `createDirectory`    | `(path)`             | `Promise<void>`                        | `await opfs.createDirectory('myDir')`              | Creates a new directory                   |
| `deleteDirectory`    | `(path)`             | `Promise<void>`                        | `await opfs.deleteDirectory('myDir')`              | Deletes a directory                       |
| `renameDirectory`    | `(oldPath, newPath)` | `Promise<void>`                        | `await opfs.renameDirectory('old', 'new')`         | Renames a directory                       |
| `dirExists`          | `(path)`             | `Promise<boolean>`                     | `await opfs.dirExists('dir')`                      | Checks if directory exists                |
| `resolvePath`        | `(path)`             | `string`                               | `opfs.resolvePath('./a/../b')`                     | Returns normalized path                   |
| `joinPath`           | `(...paths)`         | `string`                               | `opfs.joinPath('a', 'b')`                          | Joins path parts                          |
| `splitPath`          | `(path)`             | `Array<string>`                        | `opfs.splitPath('a/b')`                            | Splits path into parts                    |
| `getParentDirectory` | `(path)`             | `string`                               | `opfs.getParentDirectory('a/b')`                   | Returns parent path                       |
| `getFileHandle`      | `(path)`             | `Promise<FileSystemFileHandle>`        | `await opfs.getFileHandle('file.txt')`             | Returns file handle                       |
| `getDirectoryHandle` | `(path)`             | `Promise<FileSystemDirectoryHandle>`   | `await opfs.getDirectoryHandle('dir')`             | Returns directory handle                  |
| `ensurePermission`   | `(handle, mode)`     | `Promise<boolean>`                     | `await opfs.ensurePermission(handle, 'readwrite')` | Requests permission for handle            |
| `getFileMeta`        | `(path)`             | `Promise<object>`                      | `await opfs.getFileMeta('a.txt')`                  | Returns metadata info                     |
| `isReadable`         | `(path)`             | `Promise<boolean>`                     | `await opfs.isReadable('a.txt')`                   | Checks readability                        |
| `isWritable`         | `(path)`             | `Promise<boolean>`                     | `await opfs.isWritable('a.txt')`                   | Checks writability                        |
| `streamFile`         | `(path)`             | `Promise<ReadableStream>`              | `await opfs.streamFile('file.txt')`                | Returns file read stream                  |
| `writeStream`        | `(path)`             | `Promise<WritableStreamDefaultWriter>` | `let writer = await opfs.writeStream('file.txt')`  | Returns writable stream                   |
| `readFileChunk`      | `(path, start, end)` | `Promise<ArrayBuffer>`                 | `await opfs.readFileChunk('a.txt', 0, 5)`          | Reads part of file                        |
| `importFile`         | `(File, destPath)`   | `Promise<void>`                        | `await opfs.importFile(file, 'name.txt')`          | Imports a file object                     |
| `exportFile`         | `(path)`             | `Promise<Blob>`                        | `let blob = await opfs.exportFile('a.txt')`        | Exports file as Blob                      |
| `getQuota`           | `()`                 | `Promise<{usage, quota}>`              | `await opfs.getQuota()`                            | Returns storage info                      |
| `clearOPFS`          | `()`                 | `Promise<void>`                        | `await opfs.clearOPFS()`                           | Clears OPFS files                         |
| `debugTree`          | `(dirPath)`          | `Promise<string>`                      | `await opfs.debugTree('.')`                        | Visual directory tree                     |
| `.toText()`          | `()`                 | `Promise<string>`                      | `await fileData.toText()`                          | Reads content as text                     |
| `.toBlob()`          | `()`                 | `Promise<Blob>`                        | `await fileData.toBlob()`                          | Converts to Blob                          |
| `.toJSON()`          | `()`                 | `Promise<object>`                      | `await fileData.toJSON()`                          | Parses JSON content                       |
| `.toArrayBuffer()`   | `()`                 | `Promise<ArrayBuffer>`                 | `await fileData.toArrayBuffer()`                   | Converts to ArrayBuffer                   |
| `.toDataURL()`       | `()`                 | `Promise<string>`                      | `await fileData.toDataURL()`                       | Converts to base64 URL                    |

---

## License

MIT

---

If you'd like, I can add a real `README.md` file for your project and update it automatically. Want me to do that?
