# Common commands
[<< Previous](01-what-is-linux.md) | [Next >>](03-filesystem-basics.md)

This module introduces everyday commands you will use to move around the filesystem, inspect files, and manage processes. Focus on understanding what each command does before memorizing flags.

## Contents
1. [Navigation and listing](#navigation-and-listing)
2. [Working with files and folders](#working-with-files-and-folders)
3. [Viewing file contents](#viewing-file-contents)
4. [Search and find basics](#search-and-find-basics)
5. [Processes and system info](#processes-and-system-info)

## Navigation and listing
Use these to move around and see what is in a directory (folder):

```bash
pwd # Prints working directory
ls # Lists directory contents/structure
ls -la # ls, but shows all files (including hidden) in advanced list format
cd /path/to/folder # Change the working directory
```

## Working with files and folders
Create, copy, move, and remove items:

```bash
mkdir projects # Makes a new directory
touch notes.txt # Makes a new file
cp notes.txt notes.bak # Copies a file
mv notes.txt notes-old.txt # Moves a file
rm notes-old.txt # Removes a file
```

## Viewing file contents
Read or inspect files without editing them:

```bash
cat file.txt # Print (concatonate) a file to the terminal
less file.txt # Advanced text viewer
head -n 20 file.txt # Prints first X lines to terminal
tail -n 20 file.txt # Prints last X lines to terminal
```

## Search and find basics
Locate text in files and files on disk:

```bash
grep "pattern" file.txt # Find occurences of pattern in file.txt
grep -R "pattern" . # Search recursively from the current directory
find . -name "*.log" # Find files by name pattern
```

## Processes and system info
Check what is running and basic system status:

```bash
ps aux # Show a snapshot of running processes
top # Live view of processes and resource use
whoami # Print the current user
uname -a # Show kernel and system information
```

These commands are safe to run and read-only, so try them often to build muscle memory.

[<< Previous](01-what-is-linux.md) | [Next >>](03-filesystem-basics.md)

