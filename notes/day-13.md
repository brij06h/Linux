# Viewing Files, Command History & Linux Directory Structure

Today I learned how to view specific parts of files, check command history, use help options, and understand the purpose of important Linux directories.

---

## Viewing File Contents

### `head`

By default, `head` displays the **first 10 lines** of a file.

```bash
head file.txt
```

Display only the first **3 lines**:

```bash
head -n 3 file.txt
```

---

### `tail`

By default, `tail` displays the **last 10 lines** of a file.

```bash
tail file.txt
```

Display only the last **3 lines**:

```bash
tail -n 3 file.txt
```

---

## Counting File Statistics

The `wc` (**word count**) command displays file statistics such as lines, words, and characters.

Display all statistics:

```bash
wc file.txt
```

Count only the number of lines:

```bash
wc -l file.txt
```

Count only the number of characters:

```bash
wc -c file.txt
```

---

## Getting Help

Many Linux commands provide built-in help using the `--help` option.

Example:

```bash
useradd --help
```

This displays the available options and usage information for the command.

---

## Command History

Linux keeps a history of commands executed in the terminal.

View previously used commands:

```bash
history
```

Run a command directly from the history using its number:

```bash
!141
```

This executes command number **141** from the history list.

---

# Linux Directory Structure

Linux stores different types of files in standard directories, each with a specific purpose.

| Directory | Purpose |
|-----------|---------|
| `/bin` | Contains essential binary (executable) commands required by the system. |
| `/boot` | Contains files required to boot the operating system. |
| `/dev` | Contains special device files used to access hardware devices. |
| `/etc` | Stores system-wide configuration files. |
| `/home` | Home directories for regular users where personal files and configuration are stored. |
| `/root` | Home directory of the **root** (administrator) user. |
| `/run` | Stores runtime data such as process IDs (PID files) and lock files. Its contents are recreated after every reboot. |
| `/tmp` | Temporary storage for files. Files not accessed or modified for around **10 days** are typically removed automatically. |
| `/usr` | Contains installed software, shared libraries, documentation, and read-only program data. |
| `/var` | Stores variable data such as log files, caches, databases, print queues, and other frequently changing files. |

---

## Important Subdirectories of `/usr`

### `/usr/bin`

Contains user commands and executable programs.

### `/usr/sbin`

Contains system administration commands, typically used by the administrator.

### `/usr/local`

Stores locally installed or customized software that is separate from the system-managed packages.

---

## What I Learned

- `head` displays the beginning of a file, while `tail` displays the end.
- `head -n` and `tail -n` allow viewing a specific number of lines.
- `wc` provides file statistics, including lines and character counts.
- `--help` displays built-in usage information for many Linux commands.
- `history` shows previously executed commands.
- `!<number>` reruns a command directly from the history list.
- Linux organizes files into standard directories, each serving a specific purpose.
- `/usr` contains software and programs, while `/var` stores data that changes during system operation.
