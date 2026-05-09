# Filesystem basics
[<< Previous](02-common-commands.md) | [Next >>](04-users-and-permissions.md)

Linux treats almost everything as a file. That idea makes the system consistent: regular documents, devices, and even system information can be read or written using the same tools.

## Contents
1. [Everything is a file](#everything-is-a-file)
2. [The directory tree](#the-directory-tree)
3. [Paths: absolute and relative](#paths-absolute-and-relative)
4. [Key filesystem locations](#key-filesystem-locations)
5. [Mounts and devices](#mounts-and-devices)
6. [Hidden files and dotfiles](#hidden-files-and-dotfiles)

## Everything is a file
In Linux, "file" means any resource that can be accessed through the filesystem interface. A text document is a file, but so is a disk, a keyboard, or a stream of system data. This is why commands like `cat`, `ls`, and `grep` are so powerful: they work on the same interface no matter what the underlying resource is.

There are different kinds of files under the hood. Regular files hold data, directories hold lists of files, and special files act as interfaces to devices or the kernel. You will usually not need to distinguish these types day to day, but it helps explain why tools behave consistently across many parts of the system.

File extensions are mostly a hint to the user on Linux. Unlike Windows, which uses extensions to decide how to open many files, Linux relies on file content and permissions (for example, whether a file is marked executable) to determine how it behaves. This means that `file.txt` and `file` work the exact same

Some examples:

```bash
cat /etc/hostname # Read a configuration file
cat /proc/cpuinfo # Read live CPU information
ls /dev # View device files
```

If you want to see file types, use `ls -l` and look at the first character on each line:

```bash
ls -l /dev # Device files often start with b (block) or c (character)
ls -l /proc # Kernel-provided filesystems
```

## The directory tree
Linux uses a single directory tree with `/` at the top (the "root"). Every file and folder hangs off this root, even if it lives on a different physical disk. This is different from Windows, where drives like `C:` and `D:` are separate roots.

Think of the tree as a map. A path is the route from `/` down through directories to a target file. The tree always looks the same no matter where you mount extra disks, which keeps scripts and commands predictable.

## Paths: absolute and relative
An absolute path starts at the root and always begins with `/`. A relative path starts from your current working directory. `.` means "current directory" and `..` means "parent directory".

When you run commands, the shell resolves relative paths first, so knowing where you are matters. Use `pwd` often until it becomes second nature.

```bash
pwd # Show current directory
cd /etc # Absolute path
cd ../var/log # Relative path
```

## Key filesystem locations
You will see these paths often in Arch and other Linux distributions:

- `/home`: User home directories.
- `/etc`: System-wide configuration files.
- `/var`: Variable data like logs and caches.
- `/usr`: User programs and libraries.
- `/bin` and `/sbin`: Essential system binaries.
- `/tmp`: Temporary files, often cleared on reboot.

It is safe to explore these with `ls` and `cat`, but do not edit system files unless you understand what they control. Configuration lives mostly under `/etc`, user data under `/home`, and logs under `/var/log`.

## Mounts and devices
Disks and partitions are made available by mounting them into the directory tree. A mount point is just an empty directory that becomes the access point for another filesystem.

This is why a USB drive might appear under `/run/media/<user>/...` or `/mnt/...`. The same path-based tools work whether the files are on your main disk or a removable drive.

```bash
lsblk # Show block devices
mount # Show current mounts
```

To see filesystem usage, try:

```bash
df -h # Disk usage by mounted filesystem
du -sh /home/* # Disk usage by directory
```

## Hidden files and dotfiles
Files that start with a dot (`.`) are hidden by default. Many Linux programs store user settings in dotfiles inside your home directory.

Dotfiles are a big part of how Arch users customize their system. You will often edit these with a text editor when you start tweaking your shell, window manager, or tools.

```bash
ls -a # Show hidden files
ls -la ~ # Long listing, including dotfiles
```

[<< Previous](02-common-commands.md) | [Next >>](04-users-and-permissions.md)

