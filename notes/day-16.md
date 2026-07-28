# Linux User Accounts and User Management

Today I learned about Linux user accounts, user IDs, groups, authentication, shells, and basic user management commands.

---

# What is a User Account?

A **user account** is created by the operating system to allow users to access system resources, features, and services.

User accounts control who can use local and network resources based on assigned permissions.

---

# Types of User Accounts

Operating systems generally support different types of user accounts.

## 1. Local User Account

- Used to access **local system resources**.
- Can log in only to the computer where the account was created.

---

## 2. Domain User Account

- Used to access domain resources according to assigned permissions.
- Created on a **Domain Controller**.
- Can log in to multiple computers connected to the same domain.

---

## 3. System User Account

- Created automatically by the operating system.
- Used to run system services and background processes.

---

## 4. Group Account

A **group** is a collection of user accounts.

Groups simplify permission management by assigning permissions to multiple users at once.

---

# Linux User Types

Linux mainly has two categories of users.

## 1. Super User (root)

- Username: `root`
- UID: `0`
- Has unrestricted access to the operating system.
- Can perform all administrative tasks.

---

## 2. Non-root User

Also called:

- Regular user
- Normal user
- Standard user

A non-root user has limited permissions and requires elevated privileges for administrative tasks.

---

# User Information

Each user account contains:

- Username
- Password information
- User ID (UID)
- Group ID (GID)
- GECOS/comment field
- Home directory
- Login shell

Example:

```text
student:x:1000:1000:student:/home/student:/bin/bash
john:x:1001:1001::/home/john:/bin/bash
