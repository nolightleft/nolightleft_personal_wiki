## Pre-step: Enable WSL2 (Run PowerShell as Administrator)

powershell

```
# 1. Turn on WSL & VM platform
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
# Restart PC after above
# 2. Set WSL2 as default backend
wsl --set-default-version 2
```
## Yuk7 ArchWSL (Most popular community build, full D-disk install)

### Step1: Prepare D folder

powershell

```
mkdir D:\WSL\ArchWSL
```

1. Download Arch.zip from: [https://github.com/yuk7/ArchWSL/releases](https://github.com/yuk7/ArchWSL/releases)
2. Extract ALL zip files **into D:\WSL\ArchWSL** (don’t extract to C!)
3. Run `D:\WSL\ArchWSL\Arch.exe` → auto download rootfs & register WSL instance (entire system on D:)

### Post-install Arch config (inside Arch terminal)

bash

运行

```
# Set root password
passwd
# Create normal user (ex: myuser)
useradd -m -G wheel myuser
passwd myuser
# Enable sudo for wheel group
echo "%wheel ALL=(ALL) ALL" >> /etc/sudoers
# Initialize pacman keyring (critical for pacman install packages)
pacman-key --init
pacman-key --populate archlinux
```

### Set default login user (PowerShell)

powershell

```
D:\WSL\ArchWSL\Arch.exe config --default-user myuser
```

## Basic WSL management commands

powershell

```
# List all installed WSL distro
wsl -l -v
# Stop Arch
wsl -t ArchD
# Shutdown all WSL
wsl --shutdown
# Uninstall completely (delete vhdx on D)
wsl --unregister ArchD
```

## Key note

- All Arch filesystem stored inside single `ext4.vhdx` on D, never writes core files to C:\Windows
- `/mnt/c /mnt/d` auto mount Windows disks inside Arch by default

Do you need to setup GUI desktop (Xfce/KDE) on Arch WSL later?