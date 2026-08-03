# Gain Superuser Access (sudo)

Today I learned how Linux grants **superuser (root)** privileges using **sudo**, how to configure the `/etc/sudoers` file, and how to assign administrative rights to users or groups.

---

# Root User vs Normal User

## Root User

- Has **UID 0**.
- Can perform **all administrative tasks**.
- Has unrestricted access to the entire system.
- Only the root user can perform critical operations like:
  - Create/Delete users
  - Disk partitioning
  - Modify system configuration
  - Install system-wide software

## Normal User

- Has **limited privileges**.
- Cannot perform administrative tasks directly.
- Must use **sudo** to temporarily gain root privileges.

> **Principle of Least Privilege:** Users should only have the permissions necessary to perform their tasks.

---

# Why Root Access is Limited

In production environments:

- Not everyone is allowed to log in as `root`.
- Administrative tasks are performed using **sudo**.
- This improves security and accountability.

---

# Give a User Sudo Access

Open the sudo configuration file:

```bash
vim /etc/sudoers
```

Example:

```text
root ALL=(ALL) ALL
john ALL=(ALL) ALL
```

Meaning:

- `root` → Can execute all commands.
- `john` → Can also execute all commands using `sudo`.

---

# Why Permission is Still Denied?

Example:

```bash
john@localhost:~$ useradd Mark
```

Output:

```text
Permission denied
```

Although John is listed in `sudoers`, he is still logged in as a **normal user**.

To execute administrative commands:

```bash
sudo useradd Mark
```

or switch to root:

```bash
sudo su
```

After gaining root privileges, John can execute administrative commands.

---

# Verify the New User

List all home directories:

```bash
ls /home/
```

View user details:

```bash
tail /etc/passwd | grep Mark
```

---

# Give Sudo Access to a Group

Instead of assigning permissions user-by-user, Linux allows sudo access through groups.

View groups:

```bash
tail /etc/group
```

or

```bash
cat /etc/group | grep wheel
```

Edit the sudoers file:

```bash
vim /etc/sudoers
```

Example:

```text
%wheel ALL=(ALL) ALL
```

or create your own group:

```text
%HR ALL=(ALL) ALL
```

> `%` indicates that the entry refers to a **group**, not an individual user.

---

# Add a User to the Group

Set a password for Mark:

```bash
passwd Mark
```

Switch to Mark:

```bash
su - Mark
```

Try creating a user:

```bash
sudo useradd john
```

Output:

```text
Mark is not in the sudoers file.
```

Return to root and add Mark to the HR group:

```bash
usermod -aG HR Mark
```

- `-a` → Append
- `-G` → Secondary group

Now log in again:

```bash
su - Mark
```

Create a user:

```bash
sudo useradd john
```

Verify:

```bash
cat /etc/passwd | grep john
```

```bash
ls /home/
```

---

# Sudo Workflow

```text
Normal User
      ↓
sudo command
      ↓
Password Verification
      ↓
Temporary Root Privileges
      ↓
Execute Administrative Command
```

---

# Best Practices

- Respect the privacy of others.
- Think before executing commands.
- Root commands can permanently modify the system.
- Use sudo only when required.

> **"With great power comes great responsibility."**

---

# Key Commands

| Command | Description |
|---------|-------------|
| `sudo command` | Run a command with root privileges |
| `sudo su` | Switch to the root shell |
| `vim /etc/sudoers` | Edit sudo configuration |
| `ls /home/` | List user home directories |
| `tail /etc/passwd \| grep user` | View user information |
| `tail /etc/group` | View groups |
| `cat /etc/group \| grep wheel` | Check wheel group |
| `passwd user` | Set/change user password |
| `su - user` | Switch to another user |
| `usermod -aG group user` | Add user to a group |

---

# Lab Tasks

### Task 1

Modify the **sudoers** file so a user can run **only**:

- `useradd`
- `usermod`
- `userdel`

instead of granting full administrative privileges.

---

### Task 2

Create a dedicated group (e.g., `HR`) that is allowed to execute only:

- `useradd`
- `usermod`
- `userdel`

Then add users to that group so they inherit those limited administrative privileges.