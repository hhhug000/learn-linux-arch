# Logs and troubleshooting
[<< Previous](08-networking-fundamentals.md) | [Next >>](10-arch-installation-overview.md)

Logs tell you what the system is doing and why something failed. Learning to read logs is one of the fastest ways to fix issues on Arch.

## Contents
1. [Systemd journal basics](#systemd-journal-basics)
2. [Common log locations](#common-log-locations)
3. [Reading logs efficiently](#reading-logs-efficiently)
4. [Service troubleshooting](#service-troubleshooting)
5. [Boot and hardware issues](#boot-and-hardware-issues)
6. [Pacman and update issues](#pacman-and-update-issues)
7. [Disk and filesystem checks](#disk-and-filesystem-checks)
8. [General troubleshooting flow](#general-troubleshooting-flow)

## Systemd journal basics
`journalctl` reads the systemd journal, which collects logs from the kernel and services. It is usually the first place to check.

```bash
journalctl -b # Logs from current boot
journalctl -b -1 # Logs from previous boot
journalctl -xe # Recent errors with context
```

To see warnings only:

```bash
journalctl -p warning -b
```

Filter by service:

```bash
journalctl -u NetworkManager
journalctl -u ssh
```

## Common log locations
Not all logs are in the journal. Some services write to files under `/var/log`.

- `/var/log/pacman.log`: Package installs and updates.
- `/var/log/boot.log`: Boot messages (if enabled).
- `/var/log/Xorg.0.log`: X server logs (if using X11).
- `/var/log/journal/`: Persistent systemd journal storage (if enabled).
- `/var/log/`: General log directory.

Use `tail -f` to follow logs as they update:

```bash
tail -f /var/log/pacman.log
```

## Reading logs efficiently
Logs can be long. Use `grep` to filter and `less` to scroll.

```bash
journalctl -b | grep -i "error"
less /var/log/pacman.log
```

Useful `journalctl` options:

- `-u service`: filter by service name
- `-p err`: show only errors
- `--since "today"`: filter by time

Example:

```bash
journalctl -u NetworkManager --since "1 hour ago"
```

## Service troubleshooting
When a service fails, check its status and recent logs together.

```bash
systemctl status ssh
journalctl -u ssh -b
```

If a service will not start, look for missing files, bad config, or permission errors in the logs.

You can also check if a service is enabled at boot:

```bash
systemctl is-enabled ssh
```

## Boot and hardware issues
Kernel messages often explain hardware problems.

```bash
dmesg | less
```

To see errors only:

```bash
dmesg --level=err
```

Use `lspci` and `lsusb` to see detected hardware:

```bash
lspci
lsusb
```

## Pacman and update issues
If a package update fails, check `/var/log/pacman.log` for the exact error. Common problems include mirror issues, keyring problems, or file conflicts.

```bash
tail -n 50 /var/log/pacman.log
```

If you see a keyring error, refresh it:

```bash
sudo pacman -S archlinux-keyring
```

For file conflicts, pacman will usually print the files that need manual intervention. Do not delete system files blindly; look up the correct fix or backup first.

## Disk and filesystem checks
Disk problems often show up as read/write errors or a sudden flood of I/O warnings.

```bash
df -h # Disk usage
lsblk # Disk layout
```

If you suspect filesystem issues, use the proper tool for your filesystem (for example, `fsck` for ext4). Always run filesystem repairs from a live environment or when the filesystem is unmounted.

## General troubleshooting flow
When something breaks, use a simple loop:

1. Reproduce the issue.
2. Check logs (`journalctl`, `/var/log/*`).
3. Identify the last change (package updates, config edits).
4. Search errors and verify configs.
5. Test the fix and document what worked.

Keeping notes in a small troubleshooting log helps you solve the same issue faster next time.

Quiz: [Logs and troubleshooting](quiz/09-logs-and-troubleshooting-quiz.md)

[<< Previous](08-networking-fundamentals.md) | [Next >>](10-arch-installation-overview.md)

