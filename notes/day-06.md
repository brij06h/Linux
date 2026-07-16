# Linux Distributions

Today I learned about **Linux distributions**, **package management**, and **file systems**.

---

## What is a Linux Distribution?

A **Linux distribution (distro)** is a complete operating system built around the **Linux kernel**, combined with system utilities, package management, desktop environments, and applications.

### Popular Linux Distributions

- Ubuntu
- Debian
- Fedora
- Arch Linux
- Red Hat Enterprise Linux (RHEL)

---

## Red Hat Enterprise Linux (RHEL)

**Red Hat Enterprise Linux (RHEL)** is an enterprise-focused Linux distribution designed for:

- Security
- Stability
- Long-Term Support (LTS)
- Business and production environments

> **Note:** IBM acquired Red Hat in **2019**.

---

## Fedora & CentOS Stream

### Fedora

- Free and open-source
- Sponsored by **Red Hat**
- Receives the latest Linux technologies first
- Serves as the upstream testing ground for RHEL

### CentOS Stream

CentOS Stream sits **between Fedora and RHEL** in the development cycle.

```text
Fedora
   ↓
CentOS Stream
   ↓
RHEL
```

---

## Linux Package Formats

Unlike Windows, which commonly uses **`.exe`** installers, Linux software is distributed in several package formats.

| Package Format | Used By |
|---------------|---------|
| `.rpm` | RHEL, Fedora, CentOS |
| `.deb` | Ubuntu, Debian |
| Snap | Universal package format |
| Flatpak | Universal package format |
| AppImage | Portable applications |

### RPM (Red Hat Package Manager)

RPM is the package format used by **Red Hat-based distributions**, including:

- RHEL
- Fedora
- CentOS Stream

---

## Linux File Systems

Common Linux file systems include:

- **ext4** (most widely used)
- **XFS** (default on many RHEL systems)
- **Btrfs** (advanced features like snapshots)

Linux can also read and write Windows file systems such as:

- FAT32
- NTFS

---

# What I Learned

- A Linux distribution combines the **Linux kernel** with software and system tools.
- **RHEL** is designed for enterprise environments.
- **Fedora** serves as the upstream testing ground for RHEL.
- **CentOS Stream** bridges Fedora and RHEL.
- Linux uses package managers instead of traditional `.exe` installers.
- Common package formats include **RPM**, **DEB**, **Snap**, **Flatpak**, and **AppImage**.
- Linux supports multiple file systems, including **ext4**, **XFS**, **Btrfs**, **NTFS**, and **FAT32**.

---

## Quick Summary

| Topic | Key Point |
|--------|-----------|
| Linux Distribution | Operating system built around the Linux kernel |
| RHEL | Enterprise Linux distribution |
| Fedora | Upstream project for RHEL |
| CentOS Stream | Development branch between Fedora and RHEL |
| RPM | Package format for Red Hat-based systems |
| Package Formats | RPM, DEB, Snap, Flatpak, AppImage |
| File Systems | ext4, XFS, Btrfs, NTFS, FAT32 |

---
