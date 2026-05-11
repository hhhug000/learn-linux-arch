# System services (systemd) - Quiz
## Services

[Back](../06-system-services-systemd.md)

What is the difference between `start` and `enable`?

<details>
<summary><b>Start runs now; enable runs at boot</b></summary>
Correct
A service can be enabled without running yet, or started without enabling at boot.
</details>

<details>
<summary><b>Start removes a service; enable installs it</b></summary>
Incorrect
`start` and `enable` control service runtime and boot behavior, not installation.
</details>

<details>
<summary><b>Start runs at boot; enable runs now</b></summary>
Incorrect
This reverses the meaning of each command.
</details>

<details>
<summary><b>There is no difference; they are the same</b></summary>
Incorrect
They do different things: immediate start vs. boot-time enablement.
</details>
