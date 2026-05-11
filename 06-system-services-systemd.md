# System services (systemd)
[<< Previous](05-package-management-pacman.md) | [Next >>](07-shell-basics-and-scripting.md)

Quiz: [System services (systemd)](quiz/06-system-services-systemd-quiz.md)

Arch uses `systemd` to manage system services, startup, and logs. A service is a background process (like networking or a web server) that can be started, stopped, and enabled at boot.

## Contents
1. [Units and services](#units-and-services)
2. [Checking status](#checking-status)
3. [Start, stop, enable](#start-stop-enable)
4. [Logs with journalctl](#logs-with-journalctl)
5. [Targets and boot](#targets-and-boot)
6. [Timers and scheduled tasks](#timers-and-scheduled-tasks)

## Units and services
`systemd` manages "units". A unit can be a service, a timer, a mount, or a target. Services are the most common unit type, and their unit files usually end in `.service`.

Examples of services you might see:

- `ssh.service` (SSH server)
- `NetworkManager.service` (network management)
- `bluetooth.service` (Bluetooth stack)

## Checking status
Use `systemctl status` to see whether a service is running, its recent logs, and whether it is enabled at boot.

```bash
systemctl status ssh.service
systemctl status NetworkManager.service
```

If you omit the `.service` suffix, `systemctl` usually adds it automatically:

```bash
systemctl status ssh
```

## Start, stop, enable
"Start" runs the service now. "Enable" makes it start at boot. You usually need both for persistent services.

```bash
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl enable ssh
sudo systemctl disable ssh
```

To combine start + enable in one step:

```bash
sudo systemctl enable --now ssh
```

## Logs with journalctl
`journalctl` reads logs from the systemd journal. Use it to debug services and system boot.

```bash
journalctl -u ssh # Logs for a specific service
journalctl -b # Logs from current boot
journalctl -xe # Show recent errors with context
```

## Targets and boot
Targets are named boot states. On desktops, the default target is usually `graphical.target`. On servers, it might be `multi-user.target`.

```bash
systemctl get-default
systemctl set-default multi-user.target
```

## Timers and scheduled tasks
Timers are `systemd`'s answer to cron. A timer unit triggers a service unit on a schedule.

```bash
systemctl list-timers
```

If you see a `.timer`, look for the matching `.service` to understand what will run.

[<< Previous](05-package-management-pacman.md) | [Next >>](07-shell-basics-and-scripting.md)

