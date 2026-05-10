# Users and permissions
[<< Previous](03-filesystem-basics.md) | [Next >>](05-package-management-pacman.md)

Linux is a multi-user system. Every file and process has an owner, a group, and a set of permissions that decide who can read, write, or execute it. Understanding these basics prevents accidental damage and helps you troubleshoot access problems.

## Contents
1. [Users and groups](#users-and-groups)
2. [Ownership](#ownership)
3. [Permissions (rwx)](#permissions-rwx)
4. [Changing permissions](#changing-permissions)
5. [The root user and sudo](#the-root-user-and-sudo)
6. [Umask and default permissions](#umask-and-default-permissions)
7. [Common permission fixes](#common-permission-fixes)
8. [Quick practice](#quick-practice)

## Users and groups
Each user has a unique name and numeric ID (UID). Users also belong to one or more groups, which have group IDs (GIDs). Groups make it easier to grant access to multiple people at once.

On Arch, your main user typically has a personal group with the same name. System users (for services) often have low-numbered UIDs and no login shell, which helps isolate services from your personal account.

```bash
whoami # Show current user
id # Show UID, GID, and group memberships
groups # List groups for current user
```

## Ownership
Every file has an owner (user) and a group. You can view this with `ls -l`, which shows the owner and group columns.

Ownership matters because it defines who is allowed to change permissions and who can edit or run a file. Even if group or others have access, only the owner (or root) can change the permission bits.

```bash
ls -l
```

## Permissions (rwx)
Linux permissions are split into three sets: user (owner), group, and others. Each set can allow read (r), write (w), and execute (x). The execute bit means "can run this file" for programs, and "can enter this directory" for folders.

Directory permissions can be confusing at first:

- Read (`r`) lets you list the directory contents.
- Write (`w`) lets you create, delete, or rename items inside.
- Execute (`x`) lets you enter the directory and access items by name.

Example output:

```text
-rwxr-x--- 1 alice devs  4096 May 10 12:00 script.sh
```

Breakdown:

- `-` file type (a directory would show `d`).
- `rwx` owner can read, write, execute.
- `r-x` group can read, execute.
- `---` others have no access.

## Changing permissions
Use `chmod` to change permissions and `chown` to change ownership. There are two common ways to use `chmod`: symbolic and numeric.

Numeric permissions are a compact form: each set is a sum of `r=4`, `w=2`, `x=1`. For example, `755` means `rwx` for owner and `r-x` for group and others.

```bash
chmod u+x script.sh # Add execute for owner
chmod g-w notes.txt # Remove write for group
chmod 644 file.txt # rw for owner, r for group and others
chown alice:devs file.txt # Change owner and group
```

## The root user and sudo
The root user can do anything on the system. On Arch, you usually log in as a normal user and use `sudo` for admin tasks. `sudo` is safer because it limits powerful actions to the exact command you run.

If a command needs admin access, it will usually fail with "permission denied" or "not permitted". Add `sudo` only when you understand why the command needs it.

```bash
sudo pacman -Syu
```

## Umask and default permissions
When you create a new file or folder, it starts with default permissions based on your umask value. The umask hides certain permission bits by default (usually removing write access for group/others).

```bash
umask # Show current umask
```

Common defaults:

- Files often start as `644` (rw for owner, r for group/others).
- Directories often start as `755` (rwx for owner, r-x for group/others).

## Common permission fixes
If you see "permission denied", check ownership and permissions first. Sometimes you need to add yourself to a group or adjust folder permissions.

```bash
ls -l /path/to/file
sudo chown -R $USER:$USER /path/to/folder
```

Be careful with recursive changes and `sudo`. Always understand what a command will touch before running it.

## Quick practice
Try these safe checks to build confidence:

- Use `ls -l` in your home directory and interpret the permission bits.
- Create a test file with `touch`, then add and remove execute permissions.
- Make a folder and see how directory permissions affect `cd`.

[<< Previous](03-filesystem-basics.md) | [Next >>](05-package-management-pacman.md)

