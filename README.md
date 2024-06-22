
---

# WordPress FTP Filesystem Modifications

## Overview

This README documents the modifications made to the `class-wp-filesystem-ftpext.php` file to handle errors related to FTP connections. Specifically, we have added checks to ensure that FTP functions are only called when a valid FTP connection is available.

## Modifications

### 1. FTP Connection Checks

We added checks before calling any FTP functions to ensure that the FTP connection (`$this->link`) is valid. These checks prevent functions from being called with a null or invalid FTP connection, which was causing fatal errors.

### 2. Error Logging

Whenever an FTP function is attempted with an invalid connection, an error is logged to help with debugging and monitoring.

### Modified Functions

- `cwd()`
- `chdir($dir)`
- `chmod($file, $mode = false, $recursive = false)`
- `get_contents($file)`
- `put_contents($file, $contents, $mode = false)`
- `exists($path)`
- `is_file($file)`
- `is_dir($path)`
- `mtime($file)`
- `size($file)`
- `delete($file, $recursive = false, $type = false)`
- `copy($source, $destination, $overwrite = false, $mode = false)`
- `move($source, $destination, $overwrite = false)`
- `dirlist($path = '.', $include_hidden = true, $recursive = false)`

### Example of Added Check

Here is an example of the added check in the `cwd()` function:

```php
public function cwd() {
    if ($this->link && get_resource_type($this->link) === 'ftp') {
        $cwd = ftp_pwd($this->link);
        if ($cwd) {
            $cwd = trailingslashit($cwd);
        }
        return $cwd;
    } else {
        error_log('FTP connection is null or invalid.');
        return false;
    }
}
```

## Errors Encountered

### 1. `ftp_nlist()`

**Error Message:**
```
PHP Fatal error:  Uncaught TypeError: ftp_nlist(): Argument #1 ($ftp) must be of type FTP\Connection, null given
```
**Cause:** This error occurs when `ftp_nlist()` is called with a null FTP connection.

**Solution:** Added a check before calling `ftp_nlist()` to ensure the connection is valid.

### 2. `ftp_pwd()`

**Error Message:**
```
PHP Fatal error:  Uncaught TypeError: ftp_pwd(): Argument #1 ($ftp) must be of type FTP\Connection, null given
```
**Cause:** This error occurs when `ftp_pwd()` is called with a null FTP connection.

**Solution:** Added a check before calling `ftp_pwd()` to ensure the connection is valid.

### 3. `ftp_chdir()`

**Error Message:**
```
PHP Fatal error:  Uncaught TypeError: ftp_chdir(): Argument #1 ($ftp) must be of type FTP\Connection, null given
```
**Cause:** This error occurs when `ftp_chdir()` is called with a null FTP connection.

**Solution:** Added a check before calling `ftp_chdir()` to ensure the connection is valid.

---
