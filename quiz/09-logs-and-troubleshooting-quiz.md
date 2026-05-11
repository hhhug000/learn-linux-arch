# Logs and troubleshooting - Quiz
## Journal

[Back](../09-logs-and-troubleshooting.md)

Which command shows logs from the current boot?

<details>
<summary><b>`journalctl -b`</b></summary>
Correct
`-b` filters the systemd journal to the current boot session.
</details>

<details>
<summary><b>`journalctl -u`</b></summary>
Incorrect
`-u` filters by service name, not by boot session.
</details>

<details>
<summary><b>`dmesg -b`</b></summary>
Incorrect
`dmesg` reads kernel messages and does not use `-b` for boot sessions.
</details>

<details>
<summary><b>`tail -f /var/log`</b></summary>
Incorrect
`/var/log` is a directory; `tail` needs a file, and it is not boot-specific.
</details>
