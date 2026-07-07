# Disks, Partitions & Mount Points

Today I learned how Linux identifies storage devices and how partitions are accessed.

## Users

* **Root User (Superuser):** Has full administrative privileges.
* **Normal User:** Has limited permissions and cannot perform administrative tasks without proper authorization.

---

## Storage Devices in Linux

In Linux, storage devices are represented inside the `/dev` directory.

For example:

* `/dev/sda` → First SATA storage device
* `/dev/sdb` → Second SATA storage device

Here:

* `sd` stands for **SCSI/SATA disk**
* `a`, `b`, `c`... represent the order in which drives are detected.

---

## Partitions

A disk can be divided into multiple partitions.

Example:

* `/dev/sda1` → First partition
* `/dev/sda2` → Second partition
* `/dev/sda3` → Third partition

If a new hard disk is added:

* `/dev/sdb`

  * `/dev/sdb1`
  * `/dev/sdb2`
  * `/dev/sdb3`

---

## Mount Points

Creating a partition alone doesn't make it usable.

A partition must be **mounted** before Linux can access it.

A **mount point** is simply a directory where a partition is attached.

Example:

```
/data
```

If `/dev/sdb1` is mounted on `/data`, then all files inside that partition become accessible through the `/data` directory.

The root directory `/` is the top-level directory of the Linux file system.

---

## What I Learned

* Root users have full privileges.
* Storage devices are represented inside `/dev`.
* Partitions are named like `/dev/sda1`, `/dev/sda2`, etc.
* A partition becomes usable only after it is mounted to a directory.
