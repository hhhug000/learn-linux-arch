# Networking fundamentals
[<< Previous](07-shell-basics-and-scripting.md) | [Next >>](09-logs-and-troubleshooting.md)

Quiz: [Networking fundamentals](quiz/08-networking-fundamentals-quiz.md)

Networking basics help you understand how your system connects to the internet and other devices. This module focuses on the most common tools and concepts you will use on Arch.

## Contents
1. [Interfaces and IP addresses](#interfaces-and-ip-addresses)
2. [Routing and the default gateway](#routing-and-the-default-gateway)
3. [DNS basics](#dns-basics)
4. [Testing connectivity](#testing-connectivity)
5. [Ports and services](#ports-and-services)
6. [Common networking tools](#common-networking-tools)
7. [Network managers](#network-managers)
8. [Wi-Fi with nmcli](#wi-fi-with-nmcli)
9. [DHCP vs static IP](#dhcp-vs-static-ip)
10. [Firewall basics](#firewall-basics)

## Interfaces and IP addresses
Your network interface is the connection point to a network, like `eth0` for wired or `wlan0`/`wlp...` for Wi-Fi. Each interface can have one or more IP addresses.

```bash
ip addr
ip link
```

Look for lines that show `inet` (IPv4) and `inet6` (IPv6).

## Routing and the default gateway
Routing decides where packets go. The default gateway is the router your system uses to reach the internet.

```bash
ip route
```

Example output might include a line like:

```text
default via 192.168.1.1 dev wlan0
```

## DNS basics
DNS turns names (like `archlinux.org`) into IP addresses. On Arch, DNS settings may come from NetworkManager, systemd-resolved, or manual configuration.

Check resolver settings:

```bash
cat /etc/resolv.conf
```

Test DNS resolution:

```bash
ping -c 3 archlinux.org
```

## Testing connectivity
Use these commands to test whether you can reach a host, and how the path looks:

```bash
ping -c 3 1.1.1.1 # Test raw IP connectivity
ping -c 3 archlinux.org # Test DNS and connectivity
```

```bash
traceroute archlinux.org
```

If `traceroute` is missing, install it first.

## Ports and services
Network services listen on ports. You can see open ports and the services using them.

```bash
ss -tulpn # Show listening TCP/UDP ports
```

## Common networking tools
These are common commands for inspection and troubleshooting:

- `ip`: Modern tool for interfaces, routes, and addresses.
- `ss`: Socket statistics (replaces older `netstat`).
- `curl`: Fetch a URL or API endpoint.
- `wget`: Download files.
- `dig` or `nslookup`: DNS lookups.

Examples:

```bash
curl -I https://archlinux.org
dig archlinux.org
```

## Network managers
On Arch, the most common network manager is NetworkManager. Some setups use `systemd-networkd` or manual configuration instead. Use one manager at a time to avoid conflicts.

Check if NetworkManager is running:

```bash
systemctl status NetworkManager
```

## Wi-Fi with nmcli
`nmcli` is the command-line tool for NetworkManager. It is useful when you do not have a GUI.

```bash
nmcli dev status
nmcli dev wifi list
nmcli dev wifi connect "SSID" password "your_password"
```

If you see a network but cannot connect, check that `NetworkManager` is running and that your wireless interface is not blocked by rfkill.

```bash
rfkill list
```

## DHCP vs static IP
Most home networks use DHCP, which automatically gives your system an IP address, gateway, and DNS servers. A static IP is manually configured and stays the same.

Use DHCP when you want simplicity. Use a static IP when you need a stable address for a server or local service.

To see how your interface got its address, inspect its IP and lease info:

```bash
ip addr show
```

## Firewall basics
A firewall controls which network traffic is allowed in and out. On Arch, a common beginner-friendly tool is `ufw` (Uncomplicated Firewall), while power users may use `nftables` directly.

Before enabling a firewall, decide what you want to allow. A typical desktop can block all inbound traffic and allow all outbound traffic.

Example with `ufw`:

```bash
sudo pacman -S ufw
sudo systemctl enable --now ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw status verbose
```

To allow a service (example: SSH):

```bash
sudo ufw allow ssh
```

If you prefer `nftables`, start with the default template and build rules carefully. Misconfigured firewall rules can lock you out of remote systems, so always test from a local session first.

[<< Previous](07-shell-basics-and-scripting.md) | [Next >>](09-logs-and-troubleshooting.md)

