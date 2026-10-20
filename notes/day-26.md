# NetworkManager, Network Configuration & SSH

Today I learned how to configure network connections using `nmtui` and `nmcli`, manage IP addresses and hostnames, check network statistics, and configure SSH authentication.

---

# Network Configuration Using nmtui

**`nmtui`** is a text-based interface for NetworkManager.

```bash
nmtui
```

To configure a connection:

1. Select **Edit a connection**.
2. Select or add an Ethernet connection.
3. Set IPv4 configuration to **Manual**.
4. Enter the IP address, prefix, gateway, and DNS if required.
5. Save the configuration.

To activate it, select **Activate a connection** and choose the required profile.

---

# Network Configuration Using nmcli

**`nmcli`** manages NetworkManager connection profiles and network devices.

### View Connections and Devices

```bash
nmcli connection show
nmcli device status
ip -br addr
```

### Create a Connection

Create an Ethernet connection using DHCP:

```bash
nmcli connection add con-name eno2 type ethernet ifname eno2
```

Create a connection with a static IP:

```bash
nmcli connection add con-name eno3 type ethernet ifname eno3 ipv4.method manual ipv4.addresses 192.168.0.5/24 ipv4.gateway 192.168.0.254
```

### Modify an Existing Connection

Set a static IP and enable automatic connection:

```bash
nmcli connection modify static-ens3 ipv4.addresses 192.0.2.2/24 ipv4.gateway 192.0.2.254 connection.autoconnect yes
```

Add an additional DNS server:

```bash
nmcli connection modify static-ens3 +ipv4.dns 2.2.2.2
```

Apply the connection settings:

```bash
nmcli connection up static-ens3
```

Deactivate or activate a connection:

```bash
nmcli connection down ens160
nmcli connection up ens160
```

Delete a connection profile:

```bash
nmcli connection delete static-ens3
```

> `static-ens3` and `ens160` are connection profile names in these examples. Use the actual names shown by `nmcli connection show`.

---

# Network Configuration Files

Persistent NetworkManager connection profiles are commonly stored in:

```text
/etc/NetworkManager/system-connections/
```

Example:

```text
ens160.nmconnection
```

After manually editing a profile file, reload the profiles:

```bash
nmcli connection reload
```

Prefer `nmcli` or `nmtui` for configuration changes. Profile files can contain sensitive information and must have appropriate ownership and permissions.

---

# Hostname and Network Statistics

Check the hostname:

```bash
hostname
```

Change it:

```bash
hostnamectl set-hostname server.example.com
```

Display interface packet statistics:

```bash
ip -s link show ens160
```

This displays received (RX) and transmitted (TX) packets, bytes, errors, and drops.

---

# SSH (Secure Shell)

SSH provides secure remote access to Linux systems.

### Important Files

| Path | Purpose |
|---|---|
| `/etc/ssh/sshd_config` | SSH server configuration |
| `/etc/ssh/` | Server configuration and host keys |
| `~/.ssh/authorized_keys` | Public keys authorized for user login |
| `~/.ssh/known_hosts` | Server host keys trusted by the SSH client |

**Host keys** identify the server. **User SSH keys** authenticate the user. The private key should remain private.

### Root Login

Root login is controlled by `PermitRootLogin`.

To disable direct root login, configure:

```text
PermitRootLogin no
```

A regular user can use `sudo` if authorized.

### Validate and Reload SSH

Validate the configuration:

```bash
sshd -t
```

Reload SSH after making configuration changes:

```bash
systemctl reload sshd
```

Check the service status:

```bash
systemctl status sshd
```

Keep an existing SSH session open while testing changes to avoid losing remote access.

---

# User Sessions and Network Sockets

Display logged-in users:

```bash
w
```

Display listening TCP and UDP sockets:

```bash
ss -tulnp
```

Display a network socket summary:

```bash
ss -s
```

---

# Quick Revision

| Command | Purpose |
|---|---|
| `nmtui` | Configure networking interactively |
| `nmcli connection show` | List connection profiles |
| `nmcli device status` | Show network devices |
| `nmcli connection up NAME` | Activate a connection |
| `nmcli connection down NAME` | Deactivate a connection |
| `ip -br addr` | Display IP addresses briefly |
| `ip -s link show ens160` | Display interface statistics |
| `hostnamectl` | Manage the hostname |
| `w` | Show logged-in users |
| `ss -tulnp` | Show listening sockets |
| `sshd -t` | Validate SSH configuration |
| `systemctl reload sshd` | Reload SSH configuration |

---

# Summary

- `nmtui` provides a text-based network configuration interface.
- `nmcli` manages NetworkManager connections and devices.
- Static IP configuration requires the correct address and prefix.
- Connection profiles are stored under `/etc/NetworkManager/system-connections/`.
- `hostnamectl` changes the system hostname.
- `ip -s link` displays network interface statistics.
- SSH host keys identify servers; user keys authenticate users.
- `sshd -t` validates SSH configuration before reloading.
- `ss` displays network socket information.
