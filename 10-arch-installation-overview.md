# Arch installation overview
[<< Previous](09-logs-and-troubleshooting.md) | [Next >>](README.md)

Quiz: [Arch installation overview](quiz/10-arch-installation-overview-quiz.md)

Arch can be installed manually or with the `archinstall` guided installer. This module explains the big-picture steps so you understand what the installer is doing and why each decision matters.

## Contents
1. [Before you start](#before-you-start)
2. [Manual install overview](#manual-install-overview)
3. [Archinstall overview](#archinstall-overview)
4. [Post-install essentials](#post-install-essentials)
5. [Common pitfalls](#common-pitfalls)

## Before you start
Make sure you have:

- A verified Arch ISO (check the checksum if you can).
- A backup of important data.
- A plan for disk layout (single disk, dual boot, encryption).
- A way to get online (Ethernet is easiest).

## Manual install overview
The manual install is the classic Arch experience. At a high level, you:

1. Boot into the live environment.
2. Connect to the internet.
3. Partition disks and create filesystems.
4. Mount the target filesystem under `/mnt`.
5. Install base packages with `pacstrap`.
6. Generate `fstab`.
7. `arch-chroot` into the new system.
8. Set timezone, locale, and hostname.
9. Install a bootloader.
10. Create a user and set passwords.

You do not need to memorize every command, but you should understand the purpose of each step.

## Archinstall overview
`archinstall` is a guided installer that automates much of the manual process. It lets you choose disk layout, desktop environment, and basic packages from a menu.

Typical flow:

1. Boot the ISO and start `archinstall`.
2. Choose language, keyboard, and locale.
3. Select a disk layout (wipe or manual).
4. Pick a filesystem and encryption (optional).
5. Choose a profile (desktop, server, minimal).
6. Configure users, hostname, and networking.
7. Install and reboot.

`archinstall` is great for learning because you can compare the choices it makes to the manual steps. You can also rerun it to experiment with different setups.

## Post-install essentials
After the first boot, you will typically:

- Update the system: `sudo pacman -Syu`
- Install drivers (GPU, Wi-Fi) if needed.
- Set up a desktop environment or window manager.
- Enable services like NetworkManager, Bluetooth, or SSH.

## Common pitfalls
- Forgetting to install a bootloader.
- Missing firmware packages for Wi-Fi.
- Using the wrong disk during partitioning.
- Skipping locale or timezone setup.

When in doubt, consult the Arch Wiki. It is the most reliable source of up-to-date installation guidance.

[<< Previous](09-logs-and-troubleshooting.md) | [Next >>](README.md)

