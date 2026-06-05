https://www.msys2.org/

### Install rule

- Install path: **`C:\msys64`** (DO NOT use Chinese/space folder name)
- Uncheck auto-launch after install, close all MSYS2 windows first before changing source.

## 2. One-click replace all official repo → Tsinghua mirror (simplest command)

Open **MSYS2 UCRT64** from start menu, run this single sed command to replace all mirror config automatically:

bash

运行

```
sed -i "s#https\?://mirror.msys2.org/#https://mirrors.tuna.tsinghua.edu.cn/msys2/#g" /etc/pacman.d/mirrorlist*
```

> This edits 3 files automatically:
> 
> `/etc/pacman.d/mirrorlist.msys`
> 
> `/etc/pacman.d/mirrorlist.mingw32`
> 
> `/etc/pacman.d/mirrorlist.mingw64`

### Backup source (optional, for fallback)

If you want USTC as backup, manually append USTC line at top of each mirror file:

plaintext

```
Server = https://mirrors.ustc.edu.cn/msys2/$repo/$arch
```

## 3. Refresh pacman database & full system upgrade

bash

运行

```
# Force reload repo cache
pacman -Syy
# Full system update
pacman -Syu
```

> Sometimes terminal closes during first upgrade: reopen UCRT64 and rerun `pacman -Syu` again to finish updating.

## 4. Install common dev toolchain (GCC, make, git etc.)

bash

运行

```
# Base build tools + 64bit MinGW UCRT GCC
pacman -S --needed base-devel mingw-w64-ucrt-x86_64-toolchain git
```

- `base-devel`: make, patch, pkgconf etc. POSIX build tools
- `mingw-w64-ucrt-x86_64-toolchain`: gcc/g++/clang for compiling Windows native exe

## 5. Add MSYS2 bin to Windows PowerShell PATH (use gcc/git directly in PS)

Add these two paths into Windows system environment variable `Path`:

plaintext

```
C:\msys64\usr\bin
C:\msys64\ucrt64\bin
```

Restart PowerShell, verify:

powershell

```
gcc --version
git --version
```