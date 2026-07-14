# Files, Permissions & Navigation

Today I learned about creating files, file permissions, file systems, and basic directory navigation.

## `touch` Command

The `touch` command is used to create a new empty file.

```bash
touch file.txt
```

If the file **does not exist**, it creates the file.

If the file **already exists**, it **does not create another file**. Instead, it updates the file's **timestamp** (last access and modification time).

---

## Files in Linux

Unlike Windows, Linux does **not** rely on file extensions to identify file types.

A file like `notes`, `notes.txt`, or `notes.pdf` is simply a file—the system identifies its type by its content rather than its extension.

---

## File Metadata

Linux stores information about files using **Inodes**.

Windows uses a similar concept called the **Master File Table (MFT)** in the NTFS file system.

---

## File Permissions

Every file has:

* An **Owner**
* A **Group**
* **Others** (everyone else)

Example permissions:

```text
-rwxr-xr--
```

Meaning:

* **Owner:** `rwx` → Read, Write, Execute
* **Group:** `r-x` → Read, Execute
* **Others:** `r--` → Read only

---

## Hidden Files

Files that begin with a dot (`.`) are hidden.

Example:

```text
.secret
.bashrc
```

Use the following command to view hidden files:

```bash
ls -a
```

---

## Common File Systems

### Linux

| File System | Purpose                                       |
| ----------- | --------------------------------------------- |
| ext4        | Most common Linux filesystem                  |
| XFS         | High-performance filesystem (default in RHEL) |
| Btrfs       | Snapshots and advanced storage features       |
| FAT32       | Compatible with many devices                  |
| exFAT       | Suitable for large files on USB drives        |
| ISO9660     | Used for CDs and DVDs                         |

### Windows

| File System | Purpose                                             |
| ----------- | --------------------------------------------------- |
| NTFS        | Default Windows filesystem                          |
| FAT32       | Older but widely supported                          |
| exFAT       | Flash drives and cross-platform storage             |
| ReFS        | Used in some Windows Server and Enterprise editions |

---

## Directory Structure

* `~` → Home directory of the current user.
* `/` → Root directory (top-level directory).

Some important directories:

* `/root` → Home directory of the root user.
* `/home` → Home directories of normal users.
* `/dev` → Device files.
* `/etc` → System configuration files.

---

## Navigation Command

Go back one directory:

```bash
cd ..
```

---

## What I Learned

* `touch` creates a file if it doesn't exist; otherwise, it updates the file's timestamp.
* Linux identifies files using inodes rather than file extensions.
* Every file has an owner, a group, and permission settings.
* Hidden files start with a dot (`.`).
* `~` represents the current user's home directory, while `/` is the root directory.
* `cd ..` moves to the parent directory.
