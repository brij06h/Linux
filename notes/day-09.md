# Users, Shell & Basic Linux Commands

Today I learned about Linux users, the shell, basic commands, and how the operating system communicates with hardware.

---

# User Accounts

- Every user account should have a password.
- Logging in with a blank password is generally not allowed.
- `~` represents the **current user's home directory**.
- `#` indicates the **root (superuser)** prompt.
- `$` indicates a **normal user** prompt.

### Example

```bash
[root@server ~]#
```

```bash
[user@server ~]$
```

---

# GNOME Terminal

**GNOME Terminal** is the graphical terminal emulator commonly used in **Red Hat Enterprise Linux (RHEL)** and many other Linux distributions.

It provides a graphical interface for running Linux commands.

---

# Shell

A **Shell** is a command-line interpreter that accepts user commands and passes them to the operating system.

The most commonly used shell is **Bash (Bourne Again SHell)**.

### The shell allows users to:

- Run commands
- Execute shell scripts
- Manage files and directories
- Control processes

---

# Linux Architecture

```text
+--------+
|  User  |
+--------+
     │
     ▼
+----------------+
| Shell (Bash)   |
+----------------+
     │
     ▼
+----------------+
| Linux Kernel   |
+----------------+
     │
     ▼
+----------------+
| Device Drivers |
+----------------+
     │
     ▼
+------------------------------+
| Hardware                     |
| CPU • RAM • Disk • Keyboard  |
| Mouse • Monitor              |
+------------------------------+
```

### How it Works

1. The **user** enters a command.
2. The **shell** interprets the command.
3. The **kernel** processes the request.
4. The kernel communicates with **device drivers**.
5. Device drivers interact with the **hardware**.

> **Note:** The kernel does **not** communicate directly with hardware. It uses **device drivers** as an interface.

---

# Basic Commands

## List Files and Directories

```bash
ls
```

Displays the files and directories in the current location.

---

## Long Listing Format

```bash
ls -l
```

Displays detailed information such as:

- File type
- Permissions
- Owner
- Group
- File size
- Last modified date

---

# Understanding `ls -l`

Example:

```text
-rw-r--r--  1 user user   245 Jul  8  file.txt
drwxr-xr-x  2 user user  4096 Jul  8  Documents
```

## File Type

| Symbol | Meaning |
|---------|---------|
| `-` | Regular file |
| `d` | Directory |

The remaining characters represent **permissions** for:

- Owner
- Group
- Others

Example:

```text
-rw-r--r--
```

Breakdown:

| Section | Meaning |
|---------|---------|
| `rw-` | Owner permissions |
| `r--` | Group permissions |
| `r--` | Others permissions |

---

# Prompt Symbols

| Symbol | Meaning |
|---------|---------|
| `$` | Normal user |
| `#` | Root (superuser) |

Examples:

```bash
[user@server ~]$
```

```bash
[root@server ~]#
```

---

# Home Directory

The tilde (`~`) represents the current user's **home directory**.

Example:

```bash
cd ~
```

Moves directly to the user's home directory.

---

# Key Takeaways

- ✅ `~` represents the current user's home directory.
- ✅ `$` is the prompt for a normal user.
- ✅ `#` is the prompt for the root user.
- ✅ Bash is the default shell in many Linux distributions.
- ✅ The shell acts as a bridge between the user and the Linux kernel.
- ✅ The kernel communicates with hardware through device drivers.
- ✅ `ls` lists files and directories.
- ✅ `ls -l` displays detailed file information, including permissions.

---

# Summary

Linux follows this workflow:

```text
User
   ↓
Shell (Bash)
   ↓
Linux Kernel
   ↓
Device Drivers
   ↓
Hardware
```

Understanding this flow helps explain how user commands ultimately interact with the computer's hardware.
