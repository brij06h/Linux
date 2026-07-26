# Managing Links Between Files

Today I learned how Linux uses **hard links** and **symbolic (soft) links** to create multiple ways of accessing files.

---

# What are Links?

Linux allows you to create multiple file names that point to the same file or to another file.

These file names are called **links**.

There are two types of links:

- **Hard Links**
- **Symbolic (Soft) Links**

---

# Hard Links

A **hard link** is another file name that points to the **same data** on the file system.

```text
Filename A ─┐
            ├── Same File Data
Filename B ─┘
```

Every file in Linux starts with **one hard link**, which is its original filename.

---

## Create a File

```bash
cat > hdl.txt
```

Example content:

```text
This is Hard Link file and its name is hdl.txt
```

View its details:

```bash
ls -l hdl.txt
```

---

## Create a Hard Link

```bash
ln hdl.txt hardlink.txt
```

Check the files:

```bash
ls -l hdl.txt hardlink.txt
```

Example output:

```text
-rw-r--r--. 2 root root 64 Jul 13 08:42 hdl.txt
-rw-r--r--. 2 root root 64 Jul 13 08:42 hardlink.txt
```

The number **2** indicates that the file data has **two hard links (two filenames pointing to the same data)**.

---

## Editing a Hard Link

Append data:

```bash
cat >> hdl.txt
```

Any changes made through **either filename** are reflected immediately because both names point to the same file data.

---

# Viewing the Inode Number

Use the `-i` option to display the inode number.

```bash
ls -il hdl.txt hardlink.txt
```

Example output:

```text
17129266 -rw-r--r--. 2 root root 64 Jul 13 08:42 hardlink.txt
17129266 -rw-r--r--. 2 root root 64 Jul 13 08:42 hdl.txt
```

Notice that both files have the **same inode number** (`17129266`).

This confirms that both filenames reference the **same file**.

---

# Hard Link Limitations

Hard links have a few important limitations:

- They can only be created **within the same file system** (for example, the same `ext4` partition).
- They can only be created for **regular files**.
- Hard links cannot normally be created for directories.
- Since multiple filenames reference the same data, the filename itself is **not stored inside the file**.

---

# Symbolic (Soft) Links

A **symbolic link** (or **soft link**) is similar to a **shortcut**.

Instead of pointing directly to the file data, it points to the **path of another file or directory**.

```text
symbolic.txt
      ▲
      │
sym_link ─────────► symbolic.txt
```

---

## Create a Symbolic Link

```bash
ln -s /root/symbolic.txt symbolic_link.txt
```

Here:

- `-s` creates a symbolic (soft) link.
- `symbolic_link.txt` points to `/root/symbolic.txt`.

---

## View Symbolic Links

```bash
ls -il /root/symbolic.txt symbolic_link.txt
```

Example output:

```text
17129280 -rw-r--r--. 1 root root 45 Jul 13 09:10 /root/symbolic.txt
17129310 lrwxrwxrwx. 1 root root 18 Jul 13 09:12 symbolic_link.txt -> /root/symbolic.txt
```

Notice:

```text
lrwxrwxrwx
```

The first character:

```text
l
```

indicates that the file is a **symbolic link**.

Also, the symbolic link has a **different inode number** because it is a separate file that stores the path to the original file.

---

# Advantages of Symbolic Links

Unlike hard links, symbolic links can:

- Link files across **different file systems**.
- Point to **directories**.
- Point to **special device files** as well as regular files.
- Act as convenient shortcuts to frequently used files or directories.

---

# Hard Link vs Symbolic Link

| Feature | Hard Link | Symbolic (Soft) Link |
|---------|-----------|----------------------|
| Points to | File data (inode) | File or directory path |
| Shares the same inode | ✅ Yes | ❌ No |
| Can cross file systems | ❌ No | ✅ Yes |
| Can link directories | ❌ No (normally) | ✅ Yes |
| If original file is deleted | ✅ Data remains accessible through other hard links | ❌ Link becomes broken (dangling link) |
| Acts like a shortcut | ❌ No | ✅ Yes |

---

# What I Learned

- Linux supports **hard links** and **symbolic (soft) links**.
- A hard link creates another filename that points to the **same file data**.
- Hard-linked files share the **same inode number**.
- Editing one hard link updates the same underlying data, so all hard links reflect the changes.
- Hard links work only within the **same file system** and cannot normally be created for directories.
- Symbolic links act like **shortcuts** and point to the file's path.
- Symbolic links can point to files or directories across different file systems.
- In `ls -l` output, a file beginning with **`l`** indicates a symbolic link.
