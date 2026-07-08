# Partition Schemes & File Systems

Today I learned the fundamentals of **disk partitioning**, **boot firmware**, **file systems**, and **swap memory** in Linux.

---

# Partition Schemes

Before installing an operating system, a storage device (HDD/SSD) is divided into one or more partitions using a **partition scheme**.

## MBR (Master Boot Record)

- Legacy partitioning scheme.
- Supports disks up to **2 TB**.
- Allows a maximum of **4 primary partitions**.
- Commonly used with **BIOS** systems.

### Advantages
- Simple and widely supported.

### Limitations
- Maximum disk size of 2 TB.
- Limited to 4 primary partitions.

---

## GPT (GUID Partition Table)

- Modern partitioning scheme.
- Part of the **UEFI** specification.
- Supports disks larger than **2 TB**.
- Allows up to **128 partitions** by default (on most systems).
- Stores multiple copies of partition information, making it more reliable.

### Advantages
- Supports very large disks.
- More partitions.
- Better data integrity and recovery.

### Recommended
For modern computers, **GPT** is the preferred partition scheme.

---

# BIOS vs UEFI

A computer requires firmware to initialize hardware and start the operating system.

## BIOS (Basic Input/Output System)

- Traditional firmware.
- Uses the **MBR** partition scheme.
- Slower startup process.
- Limited modern hardware support.

---

## UEFI (Unified Extensible Firmware Interface)

- Modern replacement for BIOS.
- Faster boot times.
- Supports **GPT** disks.
- Includes security features such as **Secure Boot**.
- Provides better compatibility with modern hardware.

---

# EFI System Partition (ESP)

When installing Linux in **UEFI mode**, an **EFI System Partition (ESP)** is created.

It is commonly mounted at:

```bash
/boot/efi
```

This partition contains bootloader files required to start the operating system.

Typical contents include:

- GRUB bootloader
- EFI executables
- Boot configuration files

Without this partition, a UEFI system cannot boot properly.

---

# File Systems

After creating a partition, it must be **formatted with a file system** before data can be stored.

A file system determines how files and directories are organized on the disk.

## Common Linux File Systems

### ext4

- Most commonly used Linux file system.
- Stable and reliable.
- Good overall performance.
- Default file system in **Ubuntu**.

---

### XFS

- High-performance journaling file system.
- Excellent for large files and enterprise workloads.
- Default file system in **RHEL (Red Hat Enterprise Linux)**.

---

### Btrfs

- Modern Linux file system.
- Supports advanced features such as:
  - Snapshots
  - Compression
  - Checksums
  - RAID support

Ideal for systems requiring advanced storage management.

---

# Swap Partition

A **Swap** partition (or swap file) acts as **virtual memory**.

When physical RAM becomes full, Linux moves less frequently used memory pages to swap space on disk.

```
RAM  →  Swap (Disk)
```

Although swap is much slower than RAM, it helps:

- Prevent out-of-memory crashes
- Improve system stability
- Support hibernation (on some systems)

> **Note:** Swap is **not a replacement for RAM**. It only provides additional virtual memory when needed.

---

# Summary

| Component | Purpose |
|-----------|---------|
| **MBR** | Legacy partition scheme |
| **GPT** | Modern partition scheme supporting large disks |
| **BIOS** | Legacy firmware |
| **UEFI** | Modern firmware replacing BIOS |
| **EFI System Partition** | Stores UEFI bootloader files (`/boot/efi`) |
| **ext4** | Default Ubuntu file system |
| **XFS** | Default RHEL file system |
| **Btrfs** | Advanced Linux file system |
| **Swap** | Virtual memory when RAM is exhausted |

---

# Key Takeaways

- ✅ **GPT** is the modern partitioning standard.
- ✅ **UEFI** replaces the older BIOS firmware.
- ✅ The **EFI System Partition (`/boot/efi`)** stores bootloader files for UEFI systems.
- ✅ Every partition must be formatted with a **file system** before it can store data.
- ✅ **ext4**, **XFS**, and **Btrfs** are popular Linux file systems.
- ✅ **Swap** provides virtual memory when physical RAM is fully utilized.
- ✅ Modern Linux installations typically use **GPT + UEFI + ext4/XFS**.
