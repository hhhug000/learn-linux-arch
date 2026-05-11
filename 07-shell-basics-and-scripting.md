# Shell basics and scripting
[<< Previous](06-system-services-systemd.md) | [Next >>](08-networking-fundamentals.md)

The shell is the program that reads your commands and runs them. On Arch, the default is usually `bash`, so when you type in a terminal, you are talking to Bash (unless you have changed it). Learning the shell well makes every other Linux task easier.

## Contents
1. [How the shell works](#how-the-shell-works)
2. [Command structure](#command-structure)
3. [Pipes and redirection](#pipes-and-redirection)
4. [Globbing and quoting](#globbing-and-quoting)
5. [Variables and environment](#variables-and-environment)
6. [Exit codes](#exit-codes)
7. [Scripting basics](#scripting-basics)
8. [Bash scripting tips](#bash-scripting-tips)
9. [Functions and arguments](#functions-and-arguments)
10. [Input and prompts](#input-and-prompts)
11. [Arrays and loops](#arrays-and-loops)
12. [String and number tests](#string-and-number-tests)
13. [File tests](#file-tests)
14. [Debugging scripts](#debugging-scripts)
15. [Common patterns](#common-patterns)

## How the shell works
The shell reads a line, splits it into a command and arguments, then runs the command. It also expands things like wildcards, variables, and command substitutions before executing.

You can think of the shell as a small programming language focused on running programs and connecting them together.

## Command structure
A typical command looks like:

```bash
command -option --long-option argument1 argument2
```

Examples:

```bash
ls -la /etc
grep -i "error" /var/log/syslog
```

Commands usually accept options (flags) and arguments. Read a command's manual with `man command` or `command --help`.

## Pipes and redirection
Pipes send the output of one command into another. Redirection sends output to a file instead of the screen.

```bash
ls -la | grep "^d" # Show only directories
cat file.txt | wc -l # Count lines
```

Redirection:

```bash
echo "hello" > file.txt # Overwrite file
echo "more" >> file.txt # Append to file
grep "error" app.log > errors.txt
```

## Globbing and quoting
Globbing uses wildcards to match file names:

- `*` matches any characters
- `?` matches a single character
- `[...]` matches a set or range

```bash
ls *.txt
rm file?.log
```

Quoting controls how the shell interprets characters:

- Double quotes preserve spaces but still expand variables: `"$HOME"`
- Single quotes prevent expansion: `'${HOME}'`

```bash
echo "Home is $HOME"
echo 'Home is $HOME'
```

## Variables and environment
Shell variables store values for the current shell. Environment variables are inherited by child processes.

```bash
name="arch"
echo $name

export EDITOR="nvim" # Environment variable
```

View environment:

```bash
env
```

## Exit codes
Commands return exit codes: `0` means success, non-zero means failure. You can check the last exit code with `$?`.

```bash
ls /does/not/exist
echo $?
```

## Scripting basics
A shell script is a text file with a list of commands. It usually starts with a shebang that tells the system which interpreter to use.

```bash
#!/usr/bin/env bash
echo "Hello from a script"
```

Make it executable and run it:

```bash
chmod +x script.sh
./script.sh
```

## Bash scripting tips
These habits make scripts safer and easier to debug:

- Use `set -euo pipefail` to exit on errors, undefined variables, and pipeline failures.
- Quote variables to avoid word splitting: `"$var"`.
- Prefer `$(...)` over backticks for command substitution.
- Use `printf` instead of `echo` for predictable output.

Example:

```bash
#!/usr/bin/env bash
set -euo pipefail

name="arch"
printf "Hello, %s\n" "$name"
```

## Functions and arguments
Functions let you reuse code. Scripts also receive positional arguments like `$1`, `$2`, and `$@`.

```bash
#!/usr/bin/env bash
set -euo pipefail

greet() {
	local who="$1"
	printf "Hello, %s\n" "$who"
}

greet "world"
greet "${1:-user}"
```

Common argument variables:

- `$0` script name
- `$1` first argument
- `$@` all arguments (preserves words)
- `$#` number of arguments

## Input and prompts
Scripts can read input from the user or from stdin. Use `read` for interactive prompts and pipelines for non-interactive data.

```bash
read -r name
printf "Hello, %s\n" "$name"
```

Prompt with a message:

```bash
read -r -p "Enter project name: " project
printf "Creating %s\n" "$project"
```

To read from a pipeline:

```bash
printf "a\nb\n" | while read -r line; do
	printf "Line: %s\n" "$line"
done
```

## Arrays and loops
Bash supports arrays, which are useful for lists of files or arguments.

```bash
files=("a.txt" "b.txt" "c.txt")
printf "%s\n" "${files[@]}"
```

Loop over an array:

```bash
for f in "${files[@]}"; do
	printf "File: %s\n" "$f"
done
```

## String and number tests
Use `[` and `]` (or `[[` and `]]` in Bash) to compare strings and numbers. Quote your variables to avoid issues with empty values.

```bash
name="arch"
if [ -z "$name" ]; then
	echo "name is empty"
fi
```

```bash
count=5
if [ "$count" -ge 3 ]; then
	echo "count is at least 3"
fi
```

Common tests:

- `-z` empty string, `-n` non-empty string
- `=` and `!=` for string comparison
- `-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge` for numbers

## File tests
File tests let you check if something exists or has certain permissions before acting.

```bash
if [ -f /etc/hostname ]; then
	echo "hostname file exists"
fi
```

```bash
if [ -d "$HOME" ]; then
	echo "home directory exists"
fi
```

Common file tests:

- `-e` exists
- `-f` regular file
- `-d` directory
- `-x` executable
- `-r` readable, `-w` writable

## Debugging scripts
When a script misbehaves, add tracing or run checks before executing.

```bash
bash -n script.sh # Syntax check only
bash -x script.sh # Trace commands as they run
```

You can also enable tracing inside a script:

```bash
set -x # Turn on tracing
# ...commands...
set +x # Turn off tracing
```

## Common patterns
Conditionals and loops let you automate tasks. Here are a few small examples:

```bash
if [ -f /etc/hostname ]; then
	echo "hostname exists"
fi
```

```bash
for file in *.log; do
	echo "Processing $file"
done
```

```bash
while read -r line; do
	echo "Line: $line"
done < file.txt
```

The shell is powerful, but small mistakes can be dangerous. Start with simple scripts and add complexity only when you are comfortable.

Quiz: [Shell basics and scripting](quiz/07-shell-basics-and-scripting-quiz.md)

[<< Previous](06-system-services-systemd.md) | [Next >>](08-networking-fundamentals.md)

