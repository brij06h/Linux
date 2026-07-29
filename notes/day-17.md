# Manage User Password

Today I learned how Linux stores user passwords, how password hashing works, the purpose of salts, and why passwords cannot be directly recovered from their hashes.

---

# Where Linux Stores Passwords

Encrypted user passwords are stored in:

```bash
/etc/shadow
```

View the password file:

```bash
tail /etc/shadow
```

The `/etc/shadow` file stores **encrypted password hashes**, not the actual plain text passwords.

---

# What is a Hash?

A **hash** is the encrypted representation of a password created using a hashing algorithm.

### Characteristics of a Hash

- Works in **one direction only**.
- Plain text password → Hash
- Hash **cannot be directly converted back** into the original password.
- Even a small change in the password creates a completely different hash value.

---

# Password Hashing Algorithms

Linux uses cryptographic hashing algorithms to protect passwords.

| Prefix | Algorithm |
|---------|-----------|
| `$1$` | MD5 |
| `$5$` | SHA-256 |
| `$6$` | SHA-512 (Default in many Red Hat systems) |

Example:

```text
$1$buefb2sLd997veaNsskakhA
```

Here:

- `$1$` → MD5 hashing algorithm

Another example:

```text
$6$abc123xyz...
```

- `$6$` → SHA-512 hashing algorithm

---

# SHA-512

**SHA-512 (Secure Hash Algorithm 512)** is commonly used in Red Hat-based Linux distributions.

- Produces a **512-bit hash value**.
- More secure than older algorithms like MD5.
- Used to securely store user passwords.

---

# What is Salt?

A **salt** is a random value added to a password before hashing.

### Purpose of Salt

- Makes each password hash unique.
- Prevents users with the same password from having identical hashes.
- Protects against rainbow table attacks.

### Example

**User 1**

```text
Password : linux123
Salt     : 4b%s*nd
Hash      → ABC123...
```

**User 2**

```text
Password : linux123
Salt     : fkA#dm&
Hash      → XYZ789...
```

Although both users use the **same password**, their final hashes are completely different because the salts are different.

---

# Password Hash Generation Process

Linux creates the password hash using the following process:

```text
Plain Text Password
        +
      Random Salt
        ↓
Hashing Algorithm (SHA-512 / SHA-256 / MD5)
        ↓
Final Password Hash
        ↓
Stored in /etc/shadow
```

---

# Password Verification During Login

When a user logs in:

```text
User Login
      ↓
Enter Password
      ↓
System reads /etc/shadow
      ↓
Identifies hashing algorithm
(SHA-512 / SHA-256 / MD5)
      ↓
Retrieves stored salt
      ↓
Combines entered password + salt
      ↓
Generates a new hash
      ↓
Compares with stored hash
      ↓
If hashes match → Login Successful
Else → Login Failed
```

---

# Can a Hash Be Recovered?

A hash **cannot be directly reversed** into the original password because hashing is a **one-way process**.

However, password recovery or cracking tools may guess passwords using techniques such as:

- Dictionary attacks
- Brute-force attacks
- Rainbow table attacks (less effective when salts are used)

Using strong passwords and unique salts makes password cracking significantly more difficult.

---

# Key Points

- Passwords are stored in **`/etc/shadow`**.
- Linux stores **hashed passwords**, not plain text passwords.
- Hashing is a **one-way process**.
- Common hash algorithms:
  - `$1$` → MD5
  - `$5$` → SHA-256
  - `$6$` → SHA-512
- **SHA-512** is commonly used in Red Hat Linux.
- A **salt** is added before hashing to generate unique hashes.
- Even if two users have the same password, different salts produce different hashes.
- During login, Linux hashes the entered password with the stored salt and compares it with the stored hash.
- Password hashes cannot be directly recovered, but weak passwords may be cracked using specialized tools.

---

# Vim Save and Exit Commands

| Command | Description |
|---------|-------------|
| `:q` | Quit (only if there are no unsaved changes) |
| `:q!` | Quit without saving changes |
| `:w` | Save the file |
| `:wq` | Save and quit |
| `:x` | Save (only if modified) and quit |
| `ZZ` | Save and quit (Press **Shift + Z** twice, no `:` required) |

---

# Examples

## Save and Exit

```bash
Esc
:wq
```

Press **Enter**.

---

## Exit Without Saving

```bash
Esc
:q!
```

Press **Enter**.

---

## Save Without Exiting

```bash
Esc
:w
```

Press **Enter**.

---

# Common Error

If you see:

```text
E37: No write since last change (add ! to override)
```

It means there are unsaved changes.

To **save and exit**:

```bash
:wq
```

To **discard changes and exit**:

```bash
:q!
```
