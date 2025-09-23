# Linux File and Directory Permissions Guide

## 1. What Are File and Directory Permissions?

In Linux, **file and directory permissions** control **who can read, write, or execute** files and directories. It's how the system defines **who can access a file**, and **what actions they can perform** on it.

### File Permissions are represented as **r**, **w**, and **x**:

* **r (read)**: Permission to **view** the contents of a file.
* **w (write)**: Permission to **modify** the contents of a file.
* **x (execute)**: Permission to **run** a file as a program.

### Directory Permissions are a bit different but still similar:

* **r (read)**: Permission to **list** the contents of the directory.
* **w (write)**: Permission to **create, delete, or rename files** within the directory.
* **x (execute)**: Permission to **access** the directory (you can "cd" into it).

## 2. Understanding Permission Representation

When you run the `ls -l` command to list files in a directory, you'll see something like:

```bash
-rwxr-xr--
```

This is the **permission string** for a file or directory. Let's break it down:

* The first character `-` or `d` indicates whether it's a **file** (`-`) or a **directory** (`d`).
* The next three characters (`rwx`) represent the **owner**'s permissions.
* The next three characters (`r-x`) represent the **group**'s permissions.
* The final three characters (`r--`) represent **others'** permissions.

### Example:

```bash
-rwxr-xr-- 1 user group 1234 Jan 1 12:00 example.txt
```

This means:

* The **owner** (user) has **read**, **write**, and **execute** permissions.
* The **group** has **read** and **execute** permissions.
* **Others** have only **read** permission.

## 3. User Categories for Permissions

* **Owner (user)**: The person who owns the file.
* **Group**: A set of users who share access to the file.
* **Others**: All users who are not the owner or part of the group.

## 4. Changing Permissions with `chmod`

The `chmod` (change mode) command allows you to modify file or directory permissions.

### Syntax:

```bash
chmod [options] permissions filename
```

* **Permissions**: Can be **symbolic** (like `r`, `w`, `x`) or **numeric** (like `755`).
* **Filename**: The file or directory whose permissions you want to modify.

### Examples of `chmod` (symbolic mode):

* **Add execute permission to owner**:

  ```bash
  chmod u+x file.txt
  ```

  This gives the **user (owner)** execute permission on **file.txt**.

* **Remove read permission from others**:

  ```bash
  chmod o-r file.txt
  ```

  This **removes the read permission** from **others** for **file.txt**.

* **Give write permission to the group**:

  ```bash
  chmod g+w file.txt
  ```

  This adds **write permission** for the **group** on **file.txt**.

### Examples of `chmod` (numeric mode):

Numeric values for permissions are used as follows:

* `r = 4`
* `w = 2`
* `x = 1`

To set permissions numerically, you sum the values for **user**, **group**, and **others**.

* `7` = `4+2+1` (read, write, execute)
* `6` = `4+2` (read, write)
* `5` = `4+1` (read, execute)

For example:

* **Set permissions `rwxr-xr-x` (755)**:

  ```bash
  chmod 755 file.txt
  ```

  This gives the owner full permissions (`rwx`), and the group and others read/execute permissions (`r-x`).

* **Set permissions `rw-r--r--` (644)**:

  ```bash
  chmod 644 file.txt
  ```

  This gives the owner read/write permissions, and the group and others only read permissions.

## 5. Changing Ownership with `chown`

The `chown` (change owner) command allows you to change the **owner** and **group** of a file.

### Syntax:

```bash
chown [options] owner:group filename
```

* **owner**: The new owner of the file.
* **group**: The new group associated with the file.

### Examples:

* **Change owner**:

  ```bash
  chown user file.txt
  ```

  This changes the owner of **file.txt** to **user**.

* **Change owner and group**:

  ```bash
  chown user:group file.txt
  ```

  This changes the owner of **file.txt** to **user**, and the group to **group**.

* **Change group only**:

  ```bash
  chown :group file.txt
  ```

  This changes only the **group** of **file.txt** to **group**, keeping the owner unchanged.

## 6. Changing Permissions with `chgrp`

The `chgrp` command allows you to change the **group** associated with a file.

### Syntax:

```bash
chgrp group filename
```

### Example:

```bash
chgrp developers file.txt
```

This changes the **group** of **file.txt** to **developers**.

## 7. What is `umask`?

The `umask` command defines the **default permissions** that are applied when a new file or directory is created. The `umask` specifies **which permissions should be "masked" or removed** from the default permissions.

### Default Permissions:

* Files: `666` (read and write for user, group, others)
* Directories: `777` (read, write, execute for user, group, others)

The `umask` subtracts from these default permissions.

### How It Works:

The `umask` value is the **permissions to be removed** from the default. It uses a similar numerical system:

* `0`: No permissions are removed.
* `1`: Execute permission is removed.
* `2`: Write permission is removed.
* `4`: Read permission is removed.

For example:

* A `umask` of `022` means **remove write permissions** from **group** and **others**. The resulting default permissions would be `755` (rwxr-xr-x).

### Viewing the current `umask`:

```bash
umask
```

### Setting a `umask`:

To set a new `umask`:

```bash
umask 022
```

## 8. Using `ls` to View Permissions

The `ls -l` command is used to **list** files and directories, showing their permissions.

### Example:

```bash
ls -l
```

This shows:

```bash
-rwxr-xr-- 1 user group 1234 Jan 1 12:00 file.txt
```

## 9. Common Use Cases and Commands Recap

| Use Case                     | Command | Example                     | Description                                                      |
| ---------------------------- | ------- | --------------------------- | ---------------------------------------------------------------- |
| Change file permissions      | `chmod` | `chmod 755 file.txt`        | Change permissions for a file or directory.                      |
| Change file ownership        | `chown` | `chown user:group file.txt` | Change owner and/or group of a file.                             |
| Change file group            | `chgrp` | `chgrp developers file.txt` | Change group of a file.                                          |
| View file permissions        | `ls -l` | `ls -l`                     | List files with permissions, owner, and group info.              |
| Set default file permissions | `umask` | `umask 022`                 | Set default permissions for newly created files and directories. |

## 10. Summary of Key Concepts

* **File Permissions**: Determine who can read, write, or execute a file or directory.
* **Numeric & Symbolic Notation**: Use `chmod` to manage permissions.
* **Ownership**: Use `chown` and `chgrp` to manage file ownership and group association.
* **umask**: Defines default permissions for new files and directories.

With these commands and concepts, you'll be able to **securely manage files and directories**, ensuring that only the right users have access to the right files, in the right way.
