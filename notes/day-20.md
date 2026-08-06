# Linux File Permissions (chmod)

Today I learned how Linux controls access to files using **file permissions**, the different types of users who can own or access files, and how to modify permissions using both **symbolic** and **numeric** methods.

---

# Types of Linux Users

Linux classifies users into three categories:

## System User

- Created by the operating system.
- Used for running system services and applications.
- Usually cannot log in interactively.

## Local User

- Created directly on the Linux system.
- Used by people who access that specific machine.

## Domain User

- Authenticated through a centralized directory service (such as LDAP or Active Directory).
- Can access multiple systems within the same domain using the same credentials.

---

# File Ownership

Every file in Linux has three ownership categories:

- **User (Owner)** – The person who owns the file.
- **Group** – A group of users who share access to the file.
- **Other** – All users who are **neither the file owner nor members of the file's group**.

---

# File Permissions

Linux provides three basic permissions:

| Permission | Meaning |
|------------|---------|
| `r` | Read – Allows viewing the file contents |
| `w` | Write – Allows modifying the file |
| `x` | Execute – Allows running the file as a program or script |

---

# Understanding File Permission Output

Example:

```text
-rw-r--r--
```

Breakdown:

```text
- rw- r-- r--
│ │   │   │
│ │   │   └── Other users
│ │   └────── Group
│ └────────── Owner (User)
└──────────── File type
```

### File Type

- `-` → Regular file
- `d` → Directory
- `l` → Symbolic link

### Permission Explanation

```text
rw-   → Owner can Read and Write, but cannot Execute.
r--   → Group can only Read.
r--   → Other users can only Read.
---   → No permissions.
```

---

# Change Permissions Using Symbolic Method

The **symbolic method** uses letters to represent users and permissions.

### User Symbols

| Symbol | Meaning |
|--------|---------|
| `u` | User (Owner) |
| `g` | Group |
| `o` | Other |
| `a` | All users (`u`, `g`, and `o`) |

### Permission Symbols

| Symbol | Meaning |
|--------|---------|
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission |

---

## Remove All Permissions

```bash
chmod ugo-rwx file.txt
```

Output:

```text
---------- 1 root root 32 Aug 4 12:00 file.txt
```

All permissions are removed for the owner, group, and others.

---

## Give Read Permission to Everyone

```bash
chmod u+r file.txt
chmod g+r file.txt
chmod o+r file.txt
```

Equivalent command:

```bash
chmod a+r file.txt
```

---

## Give Different Permissions to Each Category

Owner:

```bash
chmod u+rwx file.txt
```

Group:

```bash
chmod g+rx file.txt
```

Others:

```bash
chmod o-rwx file.txt
```

Result:

```text
-rwxr-x---
```

- Owner → Read, Write, Execute
- Group → Read, Execute
- Others → No permissions

---

## Give Read, Write, and Execute to Everyone

```bash
chmod +rwx file.txt
```

Possible Result:

```text
-rwxrwxrwx
```

> **Note:** The final permissions may vary depending on the system **umask**.

---

# Recursive Permission Changes

The `-R` option changes permissions **recursively**, meaning the permissions are applied to a directory and all files and subdirectories inside it.

Example:

```bash
chmod -R 755 project/
```

---

# Change Permissions Using Numeric Method

Instead of letters, Linux can represent permissions using numbers.

## Permission Values

| Permission | Value |
|------------|------:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |
| No Permission | 0 |

Permissions are calculated by adding the values together.

Examples:

| Number | Permission |
|--------:|------------|
| 7 | `rwx` (4+2+1) |
| 6 | `rw-` (4+2) |
| 5 | `r-x` (4+1) |
| 4 | `r--` |
| 0 | `---` |

---

## Examples

### Remove All Permissions

```bash
chmod 000 file.txt
```

Output:

```text
----------
```

---

### Read Permission Only for Owner

```bash
chmod 400 file.txt
```

Output:

```text
-r--------
```

Only the owner can read the file.

---

### Read and Write for Owner

```bash
chmod 600 file.txt
```

Output:

```text
-rw-------
```

Only the owner can read and modify the file.

---

### Common Permission: 755

```bash
chmod 755 file.txt
```

Output:

```text
-rwxr-xr-x
```

Meaning:

- **Owner** → Read, Write, Execute (`7`)
- **Group** → Read, Execute (`5`)
- **Others** → Read, Execute (`5`)

---

# Symbolic vs Numeric chmod

| Symbolic Method | Numeric Method |
|-----------------|----------------|
| `chmod u+rwx file.txt` | `chmod 700 file.txt` |
| `chmod g+rx file.txt` | `chmod 750 file.txt` |
| `chmod o-rwx file.txt` | `chmod 770 file.txt` |
| `chmod a+r file.txt` | `chmod 444 file.txt` |

---

# Key Commands

| Command | Description |
|---------|-------------|
| `ls -l file` | View file permissions |
| `chmod u+rwx file` | Add permissions using symbolic mode |
| `chmod g+rx file` | Give read and execute to group |
| `chmod o-rwx file` | Remove all permissions from others |
| `chmod a+r file` | Give read permission to everyone |
| `chmod 755 file` | Set permissions using numeric mode |
| `chmod 600 file` | Owner can read and write only |
| `chmod 400 file` | Owner can only read |
| `chmod 000 file` | Remove all permissions |
| `chmod -R 755 directory` | Change permissions recursively |

---

# Summary

- Linux controls file access using **User**, **Group**, and **Other** ownership categories.
- Every file has **Read (`r`)**, **Write (`w`)**, and **Execute (`x`)** permissions.
- File permissions can be modified using:
  - **Symbolic method** (`u`, `g`, `o`, `a`)
  - **Numeric method** (`755`, `644`, `600`, etc.)
- The `-R` option applies permission changes recursively to directories and their contents.
- Understanding file permissions is essential for maintaining both **security** and **controlled access** in Linux systems.