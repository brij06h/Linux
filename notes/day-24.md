# Linux Disk Partitioning, File Systems & Persistent Mounts

Today I learned how Linux divides storage using partitions, creates file systems on those partitions, and mounts them for use. I also learned the difference between MBR and GPT partitioning and how to configure persistent mounts using `/etc/fstab`.

---

# Disk Partitioning

A **partition** is a logical section of a physical disk.

Partitioning can be useful to:

- Separate OS and user data.
- Allocate storage for specific applications.
- Create swap space.
- Control available storage for different purposes.

Example:

/dev/sda
├── /dev/sda1
├── /dev/sda2
└── /dev/sda3

---

# MBR Partition Scheme

**MBR (Master Boot Record)** is an older partitioning scheme commonly associated with BIOS systems.

- Supports up to **4 primary partitions**.
- One primary partition can be an **extended partition**.
- Extended partitions can contain logical partitions.
- Traditional MBR has a disk/partition size limit of about **2 TiB**.

Example:

MBR
├── Primary
├── Primary
├── Primary
└── Extended
     ├── Logical
     ├── Logical
     └── Logical

---

# GPT Partition Scheme

**GPT (GUID Partition Table)** is the modern partitioning scheme commonly used with UEFI systems.

- Supports many partitions; RHEL commonly supports up to **128 partitions**.
- Supports disks larger than 2 TiB.
- Uses GUIDs to identify disks and partitions.
- Stores a backup copy of the partition table.
- Uses checksums to help detect partition-table corruption.

GPT is generally used on modern Linux systems.

---

# Partition Management Tools

Linux provides several tools for partition management:

- `fdisk` → Common command-line partitioning tool.
- `parted` → Supports both MBR and GPT.
- `gdisk` → Designed primarily for GPT.

Check disks and partitions:

fdisk -l

Check block devices:

lsblk

---

# Using parted

Start parted:

parted /dev/sdb

Display the partition table:

print

Create a GPT partition table:

parted /dev/sdb mklabel gpt

Create a partition:

parted /dev/sdb mkpart data xfs 1MiB 2GiB

> `mkpart` creates the partition but does **not** create the actual file system.

Check the partition:

parted /dev/sdb print

> **Warning:** `mklabel` can destroy the existing partition table and make existing data inaccessible.

---

# File System

A **file system** organizes and manages data stored on a partition or other block device.

Common Linux file systems:

- **XFS**
- **ext4**

Create an XFS file system:

mkfs.xfs /dev/sdb1

Create an ext4 file system:

mkfs.ext4 /dev/sdb1

Check the file system:

lsblk --fs

or:

blkid

> Creating a file system on a device can destroy existing data.

---

# Mounting a File System

A file system must be mounted to a directory before users and applications can normally access it.

Create a mount point:

mkdir -p /mnt/mydata

Mount the partition:

mount /dev/sdb1 /mnt/mydata

Check:

df -h

or:

mount | grep sdb1

Unmount:

umount /mnt/mydata

---

# Persistent Mount

A manually mounted file system will normally not remain mounted after a reboot.

To automatically mount it at boot, configure:

/etc/fstab

Example:

UUID=xxxx-xxxx  /mnt/mydata  xfs  defaults  0 0

The `/etc/fstab` entry has **six fields**:

Device/UUID
Mount point
File system type
Mount options
Dump
FSCK order

Example:

UUID=xxxx-xxxx  /mnt/mydata  xfs  defaults  0 0

### Important Fields

- **UUID** → Identifies the file system.
- **Mount point** → Directory where the file system is mounted.
- **Type** → `xfs`, `ext4`, etc.
- **Options** → `defaults` provides commonly used mount options.
- **Dump** → Usually `0`.
- **FSCK order** → Determines file-system checking order.

For XFS, the last field is normally `0`.

---

# UUID

**UUID (Universally Unique Identifier)** provides a stable identifier for a file system.

Check UUID:

blkid

or:

lsblk --fs

Using UUID in `/etc/fstab` is recommended because device names such as `/dev/sdb1` can change depending on how disks are detected.

---

# Verify /etc/fstab

After editing `/etc/fstab`, reload systemd configuration:

systemctl daemon-reload

Test the entries:

mount -a

A useful command to verify the file:

findmnt --verify

> **Important:** An incorrect `/etc/fstab` entry can cause mounting or boot problems. Always test the configuration before rebooting.

---

# Partition vs File System vs Mount

These are three different steps:

Disk
 ↓
Partition
 ↓
File System
 ↓
Mount Point
 ↓
Usable Storage

Example:

/dev/sdb
   ↓
/dev/sdb1
   ↓
XFS
   ↓
/mnt/mydata

---

# Useful Commands

| Command | Description |
|---------|-------------|
| `fdisk -l` | List disks and partitions |
| `lsblk` | Display block devices |
| `lsblk --fs` | Display file systems and UUIDs |
| `parted /dev/sdb` | Manage partitions |
| `mkfs.xfs /dev/sdb1` | Create XFS file system |
| `mkfs.ext4 /dev/sdb1` | Create ext4 file system |
| `blkid` | Display UUID and file-system information |
| `mount /dev/sdb1 /mnt/mydata` | Mount a file system |
| `umount /mnt/mydata` | Unmount a file system |
| `df -h` | Display mounted file-system usage |
| `mount -a` | Mount entries from `/etc/fstab` |
| `findmnt --verify` | Verify `/etc/fstab` entries |
| `systemctl daemon-reload` | Reload systemd configuration |

---

# Summary

- **Partitioning** divides a physical disk into logical sections.
- **MBR** is an older partitioning scheme with a traditional 2 TiB limit.
- **GPT** is the modern partitioning scheme and supports larger disks and many partitions.
- `fdisk` and `parted` are commonly used to manage partitions.
- A partition needs a **file system** before it can be used for normal file storage.
- `mkfs.xfs` and `mkfs.ext4` create file systems.
- `mount` attaches a file system to a directory.
- `/etc/fstab` configures **persistent mounts** across reboots.
- UUIDs are recommended in `/etc/fstab` because they provide stable device identification.
- Always verify `/etc/fstab` before rebooting.
