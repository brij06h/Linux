# Linux Services, systemd & RPM Package Management

Today I learned how Linux manages **services and daemons** using `systemd`, how to control services using `systemctl`, how to troubleshoot services using dependencies and masking, and how to manage software packages using **RPM**.

---

# Processes and Daemons

After Linux starts, **systemd** manages the startup process and system services.

There are two general types of processes:

- **Program Process** → Usually started and controlled by users.
- **Daemon** → A background process that provides services to the system or other programs.

A service may use a continuously running daemon, but some services can perform a task once and then exit. These are called **oneshot services**.

---

# systemctl

`systemctl` is used to manage and inspect systemd services.

systemctl

List all service units:

systemctl list-units --type=service --all

List installed service unit files:

systemctl list-unit-files --type=service

---

# Check Service Status

systemctl status sshd.service

Important status values:

| Status | Meaning |
|--------|---------|
| `loaded` | Unit configuration file is loaded |
| `active (running)` | Service is currently running |
| `active (exited)` | Service completed successfully and exited |
| `active (waiting)` | Service is waiting for an event |
| `inactive` | Service is not running |
| `failed` | Service failed |

---

# Start, Stop & Restart Services

### Start

systemctl start sshd.service

Starts the service immediately.

### Stop

systemctl stop sshd.service

Stops the service.

### Restart

systemctl restart sshd.service

Stops and starts the service again.

> **Note:** A restart can create a new process, so the PID can change.

### Reload

systemctl reload sshd.service

Reloads the configuration without completely stopping the service.

This is useful when a service supports configuration reloads and you want to avoid interrupting existing connections.

---

# Reload or Restart

When you are unsure whether a service supports reload:

systemctl reload-or-restart sshd.service

It reloads the service if supported; otherwise, it restarts it.

---

# Enable and Disable Services

### Enable

systemctl enable sshd.service

Enables the service to start automatically during boot.

### Disable

systemctl disable sshd.service

Prevents the service from starting automatically at boot.

> **Important:** `enable` and `disable` normally affect future boots. They do not necessarily start or stop the service immediately.

To enable and start immediately:

systemctl enable --now sshd.service

---

# Check Service State

Check whether a service is active:

systemctl is-active sshd.service

Check whether it is enabled:

systemctl is-enabled sshd.service

Check whether it has failed:

systemctl is-failed sshd.service

---

# Service Dependencies

Services can depend on other units.

To check dependencies:

systemctl list-dependencies sshd.service

systemd uses these dependencies to determine which units need to be started and in what order.

---

# Masking a Service

During troubleshooting, you may want to prevent a service from being started accidentally.

systemctl mask sshd.service

Masking creates a symbolic link to `/dev/null`, preventing the service from being started normally.

Now:

systemctl start sshd.service

will fail because the service is masked.

To remove the mask:

systemctl unmask sshd.service

---

# Service Configuration Files

Every service has configuration files that control how it works.

For example, SSH server configuration is located at:

/etc/ssh/sshd_config

The SSH service uses this file to determine how the SSH server operates.

Lines beginning with `#` are generally **comments** and are ignored as configuration directives.

---

# Socket Units

A **socket unit** allows systemd to listen for communication and activate another service when a request arrives.

In simple terms:

Client
   ↓
Socket
   ↓
Service

This allows some services to start **on demand** instead of running continuously.

---

# RPM – Red Hat Package Manager

**RPM** is the package management system used to install and manage individual `.rpm` software packages on Red Hat-based systems.

Windows commonly uses:

.exe

RHEL uses:

.rpm

Example:

coreutils-8.32-31.el9.x86_64.rpm

Package name generally contains:

Name - Version - Release - Architecture - Extension

Example:

coreutils
8.32
31.el9
x86_64
.rpm

---

# RPM Architecture

Common architecture values include:

x86_64
aarch64
noarch

- `x86_64` → 64-bit x86 systems.
- `aarch64` → 64-bit ARM systems.
- `noarch` → Package is not specific to a CPU architecture.

> **Note:** `noarch` does not mean the package has no dependencies. It only means it is architecture-independent.

---

# Repository

A **repository** is a collection of software packages and metadata used by package-management tools.

RHEL commonly provides:

- **BaseOS** → Core operating system packages.
- **AppStream** → Applications, runtimes, languages, and other user-space packages.

To view mounted filesystems:

df -h

Example:

cd /run/media/root/RHEL-10-2-BaseOS-x86_64/

---

# Query Installed RPM Packages

Check whether a package is installed:

rpm -q zsh

List all installed packages:

rpm -qa

View the output page by page:

rpm -qa | less

Press:

q

to exit `less`.

---

# Install an RPM Package

rpm -ivh /path/to/package.rpm

Options:

- `-i` → Install
- `-v` → Verbose output
- `-h` → Display hash marks during installation

Example:

rpm -ivh /run/media/root/RHEL-10-2-BaseOS-x86_64/BaseOS/Packages/zsh-5.9-15.el10.x86_64.rpm

Check after installation:

rpm -q zsh

---

# Remove an RPM Package

To remove a package:

rpm -e zsh

`-e` means **erase**.

---

# Useful RPM Query Commands

| Command | Description |
|---------|-------------|
| `rpm -q package` | Check installed package version |
| `rpm -qa` | List all installed packages |
| `rpm -qi package` | Show detailed package information |
| `rpm -ql package` | List files installed by the package |
| `rpm -qc package` | List configuration files |
| `rpm -qd package` | List documentation files |
| `rpm -qf file` | Find which installed package owns a file |
| `rpm -q --scripts package` | Show package installation/removal scripts |
| `rpm -q --changelog package` | Show package changelog |
| `rpm -qlp package.rpm` | List files inside a local RPM package |

---

# RPM vs DNF

`rpm` works directly with individual RPM packages.

For repository-based package installation and dependency resolution, RHEL commonly uses:

dnf

Example:

dnf install zsh

`dnf` can find packages from configured repositories and automatically resolve dependencies.

---

# Key Commands

| Command | Description |
|---------|-------------|
| `systemctl status service` | Check service status |
| `systemctl start service` | Start service |
| `systemctl stop service` | Stop service |
| `systemctl restart service` | Restart service |
| `systemctl reload service` | Reload configuration |
| `systemctl enable service` | Enable service at boot |
| `systemctl disable service` | Disable service at boot |
| `systemctl mask service` | Prevent service from starting |
| `systemctl unmask service` | Remove service mask |
| `systemctl list-dependencies service` | Show dependencies |
| `rpm -q package` | Check installed package |
| `rpm -qa` | List installed packages |
| `rpm -ivh package.rpm` | Install RPM package |
| `rpm -e package` | Remove RPM package |
| `rpm -qi package` | Show package information |
| `rpm -ql package` | List package files |

---

# Summary

- **systemd** manages the Linux startup process and system services.
- A **daemon** is a background process that provides a service.
- `systemctl` is used to control systemd services.
- `start`, `stop`, `restart`, and `reload` control the current service state.
- `enable` and `disable` control whether a service starts automatically at boot.
- `mask` prevents a service from starting, while `unmask` removes the restriction.
- Services can have **dependencies** that determine how they are started and ordered.
- **Socket units** can activate services when communication arrives.
- Service configuration is stored in files such as `/etc/ssh/sshd_config`.
- **RPM** is used to install, remove, and query `.rpm` packages.
- `rpm -q` checks packages, `rpm -qa` lists installed packages, `rpm -i` installs, and `rpm -e` removes.
- **BaseOS** contains core operating system packages, while **AppStream** provides additional applications and user-space components.
- `dnf` is used for repository-based package installation and dependency resolution.
