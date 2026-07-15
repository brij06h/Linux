# Paths, Manual Pages & User Management

Today I learned about file paths, viewing files, manual pages, and basic user management.

## Absolute Path vs Relative Path

### 1. Absolute Path

An **absolute path** always starts from the root directory (`/`).

Example:

```text
/home/john/john.txt
```

Since it starts with `/`, it always points to the same location, no matter where you are in the terminal.

---

### 2. Relative Path

A **relative path** starts from the current working directory.

Example:

```bash
cd abc
```

If the current directory is:

```text
/root/xyz
```

Then after running:

```bash
cd abc
```

your location becomes:

```text
/root/xyz/abc
```

---

## Tab Completion

Pressing the **Tab** key automatically completes file or directory names (if possible).

Example:

```bash
cd Doc<TAB>
```

may become:

```bash
cd Documents
```

This saves time and reduces typing errors.

---

## Using `cat`

Create or edit a file:

```bash
cat > a.txt
```

After typing the content, press:

```text
Ctrl + D
```

to save and exit.

View the contents of a file:

```bash
cat a.txt
```

---

## Listing Directories

To list files inside the root directory:

```bash
ls /
```

To list files inside a specific directory:

```bash
ls /root
```

---

## Manual Pages (`man`)

Linux provides built-in documentation through **manual pages**.

Example:

```bash
man ls
```

Useful sections:

* **Section 1** – User commands
* **Section 5** – File formats and configuration files

---

## User Accounts

In most Linux distributions:

* Normal user IDs (UIDs) usually start from **1000**.
* The default login shell is:

```text
/bin/bash
```

---

## Creating a User

Create a new user:

```bash
useradd john
```

By default, Linux:

* Creates a user account.
* Creates a primary group with the same name.
* Creates a home directory (depending on the distribution and configuration).

---

## What I Learned

* Absolute paths start from `/`, while relative paths start from the current directory.
* The **Tab** key helps with auto-completing file and directory names.
* `cat` can be used to create, edit, and display files.
* `man` provides built-in command documentation.
* `useradd` creates a new user and assigns a primary group.
