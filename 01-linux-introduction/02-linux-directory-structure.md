
# Linux Directories and Subdirectories

## 1. Introduction

Linux uses a hierarchical file system to organize files and directories. The file system starts from the root directory, represented by `/`.

Directories contain files and other directories called subdirectories. This structure helps users organize system files, applications, configuration files, logs, and personal data.

## 2. Linux Directory Structure

The main Linux directories and their subdirectories are organized as follows:

```text
/
├── bin/
├── boot/
├── dev/
├── etc/
│   ├── ssh/
│   ├── systemd/
│   └── network/
├── home/
│   └── username/
│       ├── Documents/
│       ├── Downloads/
│       └── Pictures/
├── lib/
├── media/
├── mnt/
├── opt/
├── proc/
├── root/
├── run/
├── sbin/
├── srv/
├── sys/
├── tmp/
├── usr/
│   ├── bin/
│   ├── lib/
│   ├── local/
│   └── share/
└── var/
    ├── log/
    ├── cache/
    ├── lib/
    └── tmp/
```

**Note:** The exact directories and subdirectories vary depending on the Linux distribution and system configuration.

## 3. Root Directory (/)

The root directory is the top-level directory in Linux. All other files and directories exist under it.

Examples:

- `/home`
- `/etc`
- `/usr`
- `/var`

## 4. Important Linux Directories

### 4.1 /bin

Contains essential user commands required for basic system operations.

**Examples:**

- `ls`
- `cp`
- `mv`
- `cat`

On modern Linux systems, `/bin` may be linked to `/usr/bin`.

### 4.2 /boot

Contains files required to start the operating system.

**Examples:**

- Linux kernel images
- Initial RAM filesystem images
- Bootloader-related files

### 4.3 /dev

Contains device files that represent hardware and virtual devices.

**Examples:**

- `/dev/sda` – A disk device, if present
- `/dev/null` – Discards written data
- `/dev/zero` – Provides zero-valued bytes

### 4.4 /etc

Contains system-wide configuration files.

**Important subdirectories and files:**

- `/etc/ssh/` – SSH server and client configuration
- `/etc/systemd/` – Systemd configuration
- `/etc/hosts` – Local hostname mappings
- `/etc/passwd` – User account information
- `/etc/fstab` – File system mount configuration

### 4.5 /home

Contains personal directories for regular users.

**Examples:**

- `/home/sakshi/`
- `/home/sakshi/Documents/`
- `/home/sakshi/Downloads/`

Users generally store their personal files inside their home directories.

### 4.6 /lib

Contains essential shared libraries and kernel modules needed by system programs.

On many modern distributions, `/lib` is linked to a directory under `/usr`.

### 4.7 /media

Commonly used as a mount point for removable storage devices.

**Examples:**

- USB drives
- Removable disks
- Memory cards

### 4.8 /mnt

Commonly used as a temporary mount point for manually mounted file systems.

**Example:**

- `/mnt/backup/`

### 4.9 /opt

Used for optional application software and some third-party packages.

**Example:**

- `/opt/application/`

### 4.10 /proc

A virtual file system that provides information about running processes and kernel state.

**Examples:**

- `/proc/cpuinfo` – CPU information
- `/proc/meminfo` – Memory information
- `/proc/uptime` – System uptime

Most entries are generated dynamically by the kernel.

### 4.11 /root

The home directory of the root user, which is the system administrator account.

It is different from the root directory `/`.

### 4.12 /run

Stores temporary runtime information about the current system session.

Examples include process IDs and service-related runtime data.

### 4.13 /sbin

Contains system administration commands.

**Examples may include:**

- `fsck`
- `reboot`
- `shutdown`

The exact command locations vary across Linux distributions.

### 4.14 /srv

Stores data served by system services.

**Examples:**

- Website data
- File transfer service data

Its exact use depends on system configuration.

### 4.15 /sys

A virtual file system that exposes information about devices, drivers, and kernel objects.

It is mainly used by the operating system and system management tools.

### 4.16 /tmp

Stores temporary files created by applications and users.

Temporary files may be removed automatically during a reboot or by system cleanup policies.

### 4.17 /usr

Contains many user-space programs, libraries, documentation, and shared resources.

**Important subdirectories:**

- `/usr/bin/` – User commands and applications
- `/usr/sbin/` – System administration programs
- `/usr/lib/` – Libraries and supporting files
- `/usr/local/` – Locally installed software
- `/usr/share/` – Architecture-independent shared data and documentation

### 4.18 /var

Contains variable data that changes during system operation.

**Important subdirectories:**

- `/var/log/` – System and application logs
- `/var/cache/` – Cached application data
- `/var/lib/` – Persistent application state
- `/var/tmp/` – Temporary files intended to persist longer than typical `/tmp` files

## 5. Absolute and Relative Paths

### 5.1 Absolute Path

An absolute path starts from the root directory `/`.

**Example:**

`/home/sakshi/Documents/notes.txt`

### 5.2 Relative Path

A relative path starts from the current working directory.

**Example:**

`Documents/notes.txt`

A relative path does not begin with `/` and is interpreted from the current directory.
