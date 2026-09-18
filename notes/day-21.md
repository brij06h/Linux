# Linux Special Permissions & Umask

Today I learned about **special permissions in Linux**, including **SUID and SGID**, how to change file ownership, how SGID can be used on directories for **group inheritance**, and how **umask** controls default permissions for newly created files and directories.

---

# SUID – Set User ID

**SUID** allows an executable file to run with the **effective privileges of the file owner**.

The symbolic form is:

chmod u+s script

Check the permission:

ls -l script

Output:

-rwsr-xr-x. 1 root root ..... script

Notice the `s` in the owner's execute position:

-rwsr-xr-x
   ^
  SUID

### Numeric Value

SUID = `4`

chmod 4755 script

`4` represents SUID and `755` represents normal permissions.

> **Note:** SUID does not mean only the owner can execute the file. It means the executable runs with the owner's effective privileges.

---

# SGID – Set Group ID

SGID can be applied to executable files and directories.

For an executable file, SGID allows the process to use the **effective group privileges of the file's group**.

chmod g+s script

Check:

ls -l script

Output:

-rwxr-sr-x. 1 John root ..... script

The `s` appears in the group execute position.

### Numeric Value

SGID = `2`

chmod 2755 script

---

# Change File Ownership

The `chown` command is used to change the owner or group of a file.

### Change Owner

chown John script

Output:

-rwxr-xr-x. 1 John root ..... script

### Change Group

chown :HR script

Output:

-rwxr-xr-x. 1 John HR ..... script

### Change Both

chown John:HR script

---

# `ls -l` vs `ls -ld`

### Check File

ls -l file

### Check Directory Itself

ls -ld directory

Example:

ls -ld /home/xyz

Output:

drwxr-xr-x. 2 root root ..... /home/xyz

`d` at the beginning means it is a **directory**.

---

# SGID on Directories

SGID has an important use when applied to directories.

When SGID is set on a directory, **new files and directories created inside it inherit the directory's group ownership**.

Example:

mkdir /home/xyz
chown :HR /home/xyz
chmod g+s /home/xyz

Check:

ls -ld /home/xyz

Output:

drwxrwsr-x. 4 root HR ..... /home/xyz

Notice:

drwxrwsr-x
     ^
    SGID

Now if user `John` creates:

touch /home/xyz/John3.txt
mkdir /home/xyz/Johndir3

The new objects inherit:

Group → HR

This is useful for **shared directories used by teams or departments**.

---

# Umask

**Umask (user file-creation mode mask)** controls which permissions are removed when a new file or directory is created.

Check the current umask:

umask

Example:

0022

### Default Permissions

Regular files start with:

666

Directories start with:

777

The umask removes permission bits from these base permissions.

---

# Umask Example: 0022

### File

666 → Base file permission
022 → Umask
----------------
644 → Final permission

Result:

-rw-r--r--

### Directory

777 → Base directory permission
022 → Umask
----------------
755 → Final permission

Result:

drwxr-xr-x

> **Important:** Umask works as a permission mask, not simply as normal decimal subtraction.

---

# Change Umask Temporarily

umask 077

This changes the umask for the **current shell session**.

For example:

umask

Output:

0077

A `077` umask prevents group and other users from receiving permissions on newly created objects.

---

# Permanent Umask Configuration

System/login defaults can be configured through files such as:

/etc/login.defs
/etc/bashrc

For example:

vim /etc/login.defs

Search for:

/umask

and configure:

UMASK 022

> Changing the umask does **not change existing files**. It affects newly created files and directories after the new setting takes effect.

---

# Special Permission Values

| Permission | Symbol | Numeric Value |
|------------|--------|--------------:|
| SUID | `u+s` | 4 |
| SGID | `g+s` | 2 |
| Sticky Bit | `+t` | 1 |

Examples:

chmod 4755 file       # SUID
chmod 2755 file       # SGID
chmod 1777 directory  # Sticky Bit

---

# Key Commands

| Command | Description |
|---------|-------------|
| `ls -l file` | View file permissions and ownership |
| `ls -ld directory` | View directory permissions and ownership |
| `chmod u+s file` | Set SUID |
| `chmod g+s file` | Set SGID |
| `chmod 4755 file` | Set SUID numerically |
| `chmod 2755 file` | Set SGID numerically |
| `chown John file` | Change owner |
| `chown :HR file` | Change group |
| `chown John:HR file` | Change owner and group |
| `umask` | View current umask |
| `umask 077` | Temporarily change umask |

---

# Summary

- **SUID** (`u+s`) allows an executable to run with the effective privileges of its owner.
- **SGID** (`g+s`) allows an executable to use the effective group privileges of its group.
- **SGID on a directory** makes new files and directories inherit the directory's group.
- `chown` is used to change **file ownership and group ownership**.
- `ls -l` checks file information, while `ls -ld` checks the directory itself.
- **Umask** controls default permissions for newly created files and directories.
- Default base permissions are:
  - File → `666`
  - Directory → `777`
- With `umask 022`:
  - File → `644`
  - Directory → `755`
- Special permission values:
  - `4` → SUID
  - `2` → SGID
  - `1` → Sticky Bit
- Umask changes affect **newly created objects**, not existing ones.
