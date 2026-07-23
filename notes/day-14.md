# File & Directory Management and File Systems

Today I learned how to remove files and directories, copy directories, create nested directories, and understand the basics of file systems and storage organization.

---

# Removing Files and Directories

## Delete a File

Remove a file:

```bash
rm a.txt
```

---

## Delete an Empty Directory

The `rmdir` command removes **only empty directories**.

```bash
rmdir directory_name
```

If the directory contains files or subdirectories, `rmdir` will fail.

---

## Delete a Directory Recursively

Remove a directory along with all its files and subdirectories:

```bash
rm -r directory_name
```

### `-r` (Recursive)

The recursive option deletes everything inside the directory before removing the directory itself.

---

## Force Delete a Directory

Remove a directory and all of its contents **without prompting for confirmation**:

```bash
rm -rf abc/
```

### Options Used

- `-r` → Recursively delete files and directories.
- `-f` → Force deletion without asking for confirmation.

> **⚠️ Warning:** Use `rm -rf` carefully because deleted files cannot be easily recovered.

---

# Copying Directories

Copy an empty directory:

```bash
cp abc/ xyz/
```

To copy a directory along with all of its contents, use the recursive option:

```bash
cp -r abc/ xyz/
```

---

# Creating Nested Directories

Create multiple directories in a single command:

```bash
mkdir -p abc/xyz/def
```

### `-p`

Creates parent directories automatically if they do not already exist.

---

# Verbose Mode

Many Linux commands support the `-v` (**verbose**) option.

Example:

```bash
cp -rv abc/ xyz/
```

or

```bash
rm -rv abc/
```

The command displays each action it performs on the terminal.

---

# What is a File System?

A **file system** is the method an operating system uses to organize, store, and retrieve data on storage devices such as:

- Hard Disk Drives (HDD)
- Solid State Drives (SSD)
- USB/Pen Drives
- CDs/DVDs

A storage partition **cannot store files until a file system is created on it**.

Applying a file system to a partition is called **formatting**.

---

# Common Linux File Systems

| File System | Purpose |
|-------------|---------|
| **ext4** | General-purpose Linux filesystem, widely used |
| **XFS** | High performance, especially for large files |
| **Btrfs** | Supports snapshots, checksums, and advanced storage features |
| **FAT32** | Broad compatibility across different operating systems |
| **exFAT** | Designed for large files on removable storage devices |
| **ISO9660** | Standard filesystem for CDs and DVDs |

---

# Common Windows File Systems

| File System | Purpose |
|-------------|---------|
| **NTFS** | Default Windows filesystem |
| **FAT32** | Older filesystem with broad compatibility |
| **exFAT** | Commonly used for flash drives and cross-platform storage |
| **ReFS** | Resilient filesystem used in certain Windows editions and servers |

---

# How Data is Stored

Before files are stored, a storage device is partitioned and formatted with a file system.

A simplified storage hierarchy looks like this:

```text
Storage Device
      │
 Partition
      │
 File System
      │
 Tracks
      │
 Sectors
      │
 Clusters
      │
 Files
```

---

## Track

A **track** is a circular path on a storage disk where data is recorded.

---

## Sector

A **sector** is the smallest physical storage unit on a disk.

Typical sector size:

```text
512 Bytes
```

---

## Cluster

A **cluster** is a group of one or more sectors.

The operating system stores files in clusters rather than individual sectors.

Example:

```text
Sector = 512 Bytes
2 Sectors = 1024 Bytes = 1 KB Cluster
```

---

## Example

Suppose the cluster size is **1 KB**.

```text
a.txt → 512 Bytes
```

Even though the file is only **512 bytes**, it occupies an entire **1 KB cluster**.

Similarly,

```text
b.txt → 512 Bytes
```

also occupies another **1 KB cluster**.

Unused space inside a cluster cannot be shared with another file.

---

# Typical Cluster Sizes

| File System | Typical Cluster Size |
|-------------|---------------------:|
| FAT | 64 KB |
| FAT32 | 32 KB |
| NTFS | 4 KB |

> The actual cluster size may vary depending on the partition size and formatting options.

---

# What I Learned

- `rm` removes files.
- `rmdir` removes only empty directories.
- `rm -r` deletes directories recursively.
- `rm -rf` forcefully removes directories and their contents.
- `cp -r` copies directories along with all files and subdirectories.
- `mkdir -p` creates nested directory structures in one command.
- `-v` (verbose) shows the progress of command execution.
- A file system organizes how data is stored on a storage device.
- Formatting applies a file system to a partition before it can store data.
- Data is stored in **clusters**, which are groups of one or more sectors.
- Different operating systems use different file systems such as **ext4**, **NTFS**, **XFS**, and **Btrfs**.
