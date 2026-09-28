# Linux Package Management, Repositories, Partitions & Swap

Today I learned how to extract RPM packages without installing them, configure a local DNF/YUM repository, create and manage disk partitions, configure persistent mounts using `/etc/fstab`, and manage swap space.

---

# Extract RPM Package Without Installing

`rpm2cpio` converts an RPM package into a `cpio` archive, which can then be extracted without installing the package.

Command:

rpm2cpio /path/to/package.rpm | cpio -idv

Options:

- `rpm2cpio` → Converts RPM to CPIO archive.
- `-i` → Extract files.
- `-d` → Create directories when required.
- `-v` → Verbose output.

Example:

rpm2cpio /run/media/root/RHEL-10-2-BaseOS-x86_64/BaseOS/Packages/zsh-5.9-15.el10.x86_64.rpm | cpio -idv

---

# DNF / YUM

**DNF (Dandified YUM)** is the package manager used in modern RHEL systems.

DNF can:

- Install packages.
- Remove packages.
- Update packages.
- Search and query packages.
- Resolve package dependencies.

YUM commands are retained for compatibility and generally map to DNF functionality in modern RHEL.

Example:

dnf install httpd

If `httpd` has dependencies, DNF automatically resolves and installs the required packages.

This is easier than installing every dependency manually using RPM.

---

# Configure a Local Repository

Packages can be stored on installation media such as an RHEL ISO and used to create a local repository.

Check mounted storage:

df -h

Example repository structure:

/yumserver/AppStream
/yumserver/BaseOS

Create a repository configuration file:

vim /etc/yum.repos.d/yum.repo

Example:

[AppStream]
name=AppStream_Repo
baseurl=file:///yumserver/AppStream
gpgcheck=0
enabled=1

[BaseOS]
name=BaseOS_Repo
baseurl=file:///yumserver/BaseOS
gpgcheck=0
enabled=1

### Important Options

- `baseurl` → Location of the repository.
- `gpgcheck=0` → Disables GPG signature checking.
- `enabled=1` → Enables the repository.

---

# Repository Commands

Clean the package manager cache:

yum clean all

List repositories:

yum repolist all

or:

dnf repolist all

List available packages:

yum list all

Check available kernel packages:

yum list kernel

Check the currently running kernel:

uname -r

---

# Package History

DNF/YUM keeps a history of package transactions.

View history:

dnf history

Undo a previous transaction:

dnf history undo 3

The transaction ID must match the transaction you want to undo.

---

# Disk and Storage

Common types of storage interfaces/devices include:

- **PATA** → Parallel ATA
- **IDE** → Integrated Drive Electronics
- **SCSI** → Small Computer System Interface
- **SATA** → Serial ATA
- **SSD** → Solid-State Drive
- **NVMe** → Non-Volatile Memory Express

---

# Partitioning

A **partition** divides a physical storage device into separate logical sections.

Two common partitioning schemes are:

## MBR

**MBR (Master Boot Record)** is an older partitioning scheme.

- Commonly associated with BIOS firmware.
- Supports up to 4 primary partitions.
- One primary partition can be replaced by an extended partition.
- Extended partitions can contain logical partitions.
- Has a practical disk-size limitation of about 2 TB with traditional 512-byte sectors.

## GPT

**GPT (GUID Partition Table)** is the modern partitioning scheme.

- Commonly used with UEFI firmware.
- Supports many partitions; RHEL commonly supports up to 128 GPT partitions.
- Supports disks larger than 2 TB.
- Does not use the primary/extended/logical partition structure of MBR.

---

# View Disk Information

To view available disks and partitions:

fdisk -l

Example:

fdisk -l

The output shows devices such as:

/dev/sda
/dev/sdb

---

# Create a Partition Using fdisk

Start `fdisk`:

fdisk /dev/sda

Useful commands inside `fdisk`:

- `p` → Print partition table.
- `n` → Create a new partition.
- `d` → Delete a partition.
- `w` → Write/save changes.
- `q` → Quit without saving.

Example:

n

Select the partition number or press Enter for the default.

For the first sector, press Enter to use the default.

For the last sector:

+1G

This creates a partition of approximately 1 GB.

Check the partition:

p

Save the changes:

w

After changing the partition table, the kernel may need to reread the partition table.

partprobe

---

# File System

A **file system** is the method used by an operating system to organize and store data on a storage device.

Common Linux file systems include:

- XFS
- ext4

Create an XFS file system:

mkfs.xfs /dev/sda5

Create an ext4 file system:

mkfs.ext4 /dev/sda5

> **Important:** Creating a file system on a partition can destroy existing data on that partition.

---

# UUID

**UUID (Universally Unique Identifier)** uniquely identifies a file system or partition.

Check UUIDs:

blkid

Example:

/dev/sda5: UUID="xxxx-xxxx" TYPE="xfs"

UUIDs are commonly used in `/etc/fstab` for persistent mounting because device names can change.

---

# Mounting a File System

Create a mount point:

mkdir -p /mnt/mydata

Mount a partition:

mount /dev/sda5 /mnt/mydata

Check the mounted file system:

df -h

Unmount it:

umount /mnt/mydata

---

# Persistent Mounting with /etc/fstab

Temporary mounts disappear after reboot.

For persistent mounting, configure:

/etc/fstab

Example:

/dev/sda4  /mnt/mydata  ext4  defaults  0 0

A safer approach is to use the UUID:

UUID=xxxx-xxxx  /mnt/mydata  ext4  defaults  0 0

After modifying `/etc/fstab`, test the configuration:

mount -a

If there are no errors, the entries can be mounted successfully.

> **Important:** Be very careful when editing `/etc/fstab`. An incorrect entry can cause boot or mounting problems.

---

# systemctl daemon-reload

After changing systemd-related configuration, you can make systemd reload its unit configuration:

systemctl daemon-reload

For `/etc/fstab` changes, `mount -a` is used to test and mount the configured file systems.

---

# Partition Changes

After creating or deleting partitions, the kernel needs to recognize the updated partition table.

Command:

partprobe

It asks the kernel to reread the partition table.

> **Note:** If a partition is currently in use, a reboot may sometimes be required before the change can be fully recognized.

---

# Swap Space

**Swap** is disk space used by Linux as additional virtual memory when needed.

Check active swap:

swapon --show

or:

free -h

A swap partition created during installation may already be enabled.

A newly created swap partition is not automatically enabled until configured.

---

# Create and Enable Swap Partition

Create a swap signature:

mkswap /dev/sda4

Enable the swap:

swapon /dev/sda4

Check:

swapon --show

To disable it:

swapoff /dev/sda4

---

# Swap Priority

Linux can use multiple swap areas.

Swap areas can have different priorities, and Linux prefers higher-priority swap areas.

Check priorities:

swapon --show

Example:

NAME      TYPE      SIZE   USED  PRIO
/dev/sda4 partition 2G     0B    -2
/dev/sdb1 partition 4G     0B    10

A higher priority value is preferred.

---

# Storage Allocation in Virtual Machines

When creating a virtual disk, virtualization software may provide options such as:

- **Allocate all disk space now** → Physical storage is allocated immediately.
- **Dynamically allocated** → The virtual disk grows as data is added, up to its configured maximum size.

These settings affect how the virtual disk consumes physical storage.

---

# Useful Commands

| Command | Description |
|---------|-------------|
| `rpm2cpio package.rpm` | Convert RPM to CPIO archive |
| `cpio -idv` | Extract CPIO archive |
| `df -h` | Show disk usage and mounted file systems |
| `dnf install package` | Install package and dependencies |
| `dnf repolist` | List repositories |
| `dnf history` | View package transaction history |
| `fdisk -l` | List disks and partitions |
| `fdisk /dev/sda` | Manage partitions on a disk |
| `partprobe` | Ask kernel to reread partition table |
| `blkid` | Display block device UUIDs and file systems |
| `mkfs.xfs` | Create XFS file system |
| `mkdir -p` | Create directory and required parents |
| `mount` | Mount a file system |
| `umount` | Unmount a file system |
| `mount -a` | Mount file systems configured in `/etc/fstab` |
| `mkswap` | Create a swap area |
| `swapon` | Enable swap |
| `swapoff` | Disable swap |

---

# Summary

- `rpm2cpio` and `cpio` can extract RPM contents without installing the package.
- **DNF** manages packages, repositories, updates, removals, and dependencies.
- **BaseOS** and **AppStream** are important RHEL repositories.
- **MBR** and **GPT** are partitioning schemes used to organize storage.
- `fdisk` can be used to create and manage partitions.
- `partprobe` asks the kernel to reread partition-table changes.
- A **file system** organizes data inside a partition.
- `mkfs.xfs` and `mkfs.ext4` create file systems.
- **UUID** provides a stable identifier for a file system and is commonly used in `/etc/fstab`.
- `/etc/fstab` is used to configure persistent mounts.
- **Swap** provides disk-based virtual memory.
- `mkswap` creates swap space, while `swapon` and `swapoff` enable and disable it.
- Higher swap priority values are preferred when multiple swap areas are available.
