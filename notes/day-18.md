# Password Aging Policy

Today I learned how Linux manages **password expiration** and **account validity** using the `chage` command.

---

# What is Password Aging?

Password aging is a security policy that controls:

- How long a password is valid.
- When users can change their password.
- When users receive expiration warnings.
- When an account becomes inactive or expires.

It forces users to update passwords regularly, improving system security.

---

# View Password Aging Details

```bash
chage -l john
```

Displays:

- Last password change
- Password expiry date
- Account expiry date
- Minimum & maximum password age
- Warning period
- Inactive period

---

# Set Password Aging Policy

```bash
chage -m 30 -M 90 -W 5 -I 2 john
```

| Option | Meaning |
|---------|---------|
| `-m 30` | Password cannot be changed before **30 days** |
| `-M 90` | Password expires after **90 days** |
| `-W 5` | Warn user **5 days** before expiry |
| `-I 2` | Disable login **2 days after** password expires |

### Flow

```text
Password Set
      â†“
Valid for 90 Days
      â†“
Warning Starts (Last 5 Days)
      â†“
Password Expires
      â†“
2 Days Grace Period
      â†“
Account Becomes Inactive
```

---

# Set Account Expiration Date

Expire an account on a specific date:

```bash
chage -E 2026-07-26 john
```

- `-E` sets the **account expiration date**.
- After this date, the user **cannot log in**, even with the correct password.

Verify:

```bash
chage -l john
```

---

# Set Expiration Dynamically

Expire the account **2 days from today**:

```bash
chage -E $(date -d "+2 days" +%F) john
```

Explanation:

- `date -d "+2 days"` â†’ Calculates the date 2 days from today.
- `+%F` â†’ Formats it as `YYYY-MM-DD`.
- `chage -E` â†’ Sets it as the account expiration date.

---

# Common `chage` Options

| Command | Description |
|---------|-------------|
| `chage -l user` | View password aging details |
| `chage -m` | Set minimum password age |
| `chage -M` | Set maximum password age |
| `chage -W` | Set warning days |
| `chage -I` | Set inactive period after password expiry |
| `chage -E` | Set account expiration date |

---

# Key Points

- `chage` manages **password aging** and **account expiration**.
- `-m` prevents frequent password changes.
- `-M` defines password lifetime.
- `-W` warns users before password expiry.
- `-I` disables login after the grace period.
- `-E` expires the entire user account on a specific date.
- `chage -l` is used to verify all password aging settings.