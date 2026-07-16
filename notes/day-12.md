# Input, Output & Redirection

Today I learned how Linux handles input, output, errors, and how to redirect them into files.

## Switching Users

Switch to another user:

```bash
su john
```

This logs in as **john** and takes you to John's environment (after entering the password).

To return to the previous user:

```bash
exit
```

---

## Standard Input, Output & Error

Every Linux program communicates through three standard channels:

| Channel                  | Number | Default Device |
| ------------------------ | :----: | -------------- |
| Standard Input (stdin)   |    0   | Keyboard       |
| Standard Output (stdout) |    1   | Terminal       |
| Standard Error (stderr)  |    2   | Terminal       |

* **stdin (0)** → Receives input from the keyboard.
* **stdout (1)** → Displays normal output on the terminal.
* **stderr (2)** → Displays error messages on the terminal.

---

## Finding Files

Search for a file named `passwd`:

```bash
find /etc -name passwd
```

If the user doesn't have permission to access certain directories, **Permission denied** messages are sent to **stderr**, while valid results are sent to **stdout**.

---

## Redirecting Output

Store standard output in a file:

```bash
find /etc -name passwd > stdout.txt
```

View the file:

```bash
cat stdout.txt
```

### `>` (Overwrite)

Creates a new file or replaces the existing content.

```bash
date > file.txt
```

### `>>` (Append)

Adds new output without removing existing content.

```bash
date >> file.txt
```

---

## Redirecting Errors

Store only error messages:

```bash
find /etc -name passwd 2> stderr.txt
```

Append errors:

```bash
find /etc -name passwd 2>> stderr.txt
```

---

## Redirecting Both Output and Errors

Store both stdout and stderr in the same file:

```bash
find /etc -name passwd &> output.txt
```

Append both:

```bash
find /etc -name passwd &>> output.txt
```

Another equivalent syntax:

```bash
find /etc -name passwd > output.txt 2>&1
```

---

## Ignoring Error Messages

Linux provides a special file called **`/dev/null`**, often called the **black hole** because anything sent to it is discarded.

Ignore all error messages:

```bash
find /etc -name passwd 2> /dev/null
```

---

## Pipelines (`|`)

A **pipeline** connects the **standard output** of one command to the **standard input** of another command.

Example:

```bash
ls -l | less
```

or

```bash
cat file.txt | grep Linux
```

Pipelines allow multiple commands to work together efficiently.

---

## What I Learned

* Linux uses three standard channels: **stdin (0)**, **stdout (1)**, and **stderr (2)**.
* `>` overwrites a file, while `>>` appends to it.
* `2>` redirects only error messages.
* `&>` redirects both output and errors to the same file.
* `/dev/null` is used to discard unwanted output or errors.
* Pipelines (`|`) connect the output of one command to the input of another.
