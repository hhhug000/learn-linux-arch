# Package management (pacman)
[<< Previous](04-users-and-permissions.md) | [Next >>](06-system-services-systemd.md)

Arch uses `pacman` to install, remove, and update software. It is fast and consistent, but you should use it carefully because Arch is a rolling-release system and updates can change system behavior.

Package management is one of the biggest differences between Linux and other operating systems. Instead of downloading random installers, you use trusted repositories that keep software updated and consistent with the rest of your system.

## Contents
1. [Updating the system](#updating-the-system)
2. [Installing packages](#installing-packages)
3. [Searching and info](#searching-and-info)
4. [Removing packages](#removing-packages)
5. [Package files and caches](#package-files-and-caches)
6. [Arch repos and the AUR](#arch-repos-and-the-aur)
7. [AUR helper: yay](#aur-helper-yay)
8. [Yay vs manual AUR](#yay-vs-manual-aur)

## Updating the system
On Arch, you typically update everything at once. This keeps the system consistent.

```bash
sudo pacman -Syu # Sync repos and update all packages
```

If a large update pulls in a new kernel or major libraries, rebooting afterward is often a good idea so everything is using the new versions.

## Installing packages
Install one or more packages by name. `pacman` resolves dependencies automatically.

```bash
sudo pacman -S git
sudo pacman -S firefox neovim
```

If you are not sure about a package name, search for it first, then install the exact package you want.

## Searching and info
Use these to find packages and read descriptions before installing.

```bash
pacman -Ss editor # Search remote repos
pacman -Si neovim # Show remote package info
pacman -Qi bash # Show info for an installed package
```

You can also see which package installed a file:

```bash
pacman -Qo /usr/bin/bash # Find the owning package
```

## Removing packages
Remove a package and its unused dependencies to keep your system tidy.

```bash
sudo pacman -R firefox
sudo pacman -Rns firefox # Remove config files and orphaned deps
```

To clean up orphaned dependencies across the system:

```bash
pacman -Qdt # List orphaned packages
sudo pacman -Rns $(pacman -Qdtq) # Remove orphans
```

## Package files and caches
Pacman caches downloaded packages under `/var/cache/pacman/pkg`. You can clean old packages when disk space is tight.

```bash
sudo pacman -Sc # Remove old package versions
sudo pacman -Scc # Remove all cached packages
```

Be careful with `-Scc`, because it removes all cached packages and makes downgrades harder.

## Arch repos and the AUR
Arch packages come from official repositories. The AUR (Arch User Repository) is a community collection of build scripts that allow you to build packages from source. The AUR is powerful, but it is not curated or audited the same way official repos are.

When you use the AUR, you are trusting the PKGBUILD and any sources it pulls from. That means you should read the build script and understand what it will run on your machine. Many users install an AUR helper, but it is best to learn the manual process first so you know what the helper is automating.

Basic manual workflow:

```bash
git clone https://aur.archlinux.org/some-package.git
cd some-package
less PKGBUILD # Read the build instructions
makepkg -si # Build and install
```

If you see a suspicious download URL or a build step you do not understand, stop and investigate. AUR packages can be safe, but they are not reviewed the same way as official packages.

Helpful tips:

- Prefer official repos when possible.
- Search for well-maintained AUR packages with recent updates and active maintainers.
- Avoid running AUR build steps as root; `makepkg` builds as your user and only uses `sudo` for install.
- Keep track of what you installed from the AUR so you can update it later.

If you choose an AUR helper later, treat it as a convenience tool, not a replacement for understanding how builds work.

## AUR helper: yay
`yay` is a popular helper that can search and install packages from both official repos and the AUR with a unified command. It still builds AUR packages locally, so you are responsible for reviewing what you install.

Typical usage looks similar to `pacman`:

```bash
yay -Syu # Update system and AUR packages
yay -S google-chrome # Install an AUR package
yay -Ss terminal # Search across repos and AUR
```

Installing `yay` (manual AUR build):

```bash
sudo pacman -S --needed base-devel git
git clone https://aur.archlinux.org/yay.git
cd yay
less PKGBUILD # Review build steps
makepkg -si
```

Even with a helper, take time to read PKGBUILD and prompts when installing from the AUR. If something looks off, cancel and investigate before proceeding.

## Yay vs manual AUR
Use this as a quick mental checklist before deciding:

| Approach | Pros | Cons | Best for |
| --- | --- | --- | --- |
| Manual AUR (`makepkg`) | Maximum transparency, best for learning | Slower, more steps | First-time users, security-conscious setups |
| `yay` helper | Fast, convenient, unified search | Easy to skip review, hides steps | Experienced users who still review PKGBUILD |

[<< Previous](04-users-and-permissions.md) | [Next >>](06-system-services-systemd.md)

