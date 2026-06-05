# How to Install Packages on Arch Linux WSL2 \(Complete Guide\)

This guide covers **official repo packages \(pacman\)** and **AUR packages** for Arch WSL2, including WSL\-specific fixes, common commands, and error solutions\.

## 1\. First\-Time WSL Arch Initialization \(Must Do\)

Fresh Arch WSL install has uninitialized keyring, which causes package installation failures\. Run these commands first:

```bash
# Initialize package keyring
sudo pacman-key --init
sudo pacman-key --populate archlinux

# Update keyring & full system upgrade (core for WSL)
sudo pacman -Sy archlinux-keyring
sudo pacman -Syu
```

## 2\. Official Repository Install \(pacman Default\)

Use `pacman` for all official Arch repo packages\.

### Basic Install Commands

```bash
# Install single package
sudo pacman -S <package-name>

# Install multiple packages at once
sudo pacman -S pkg1 pkg2 pkg3

# Reinstall broken package
sudo pacman -S --overwrite "*" <package-name>

# Search package before installing
pacman -Ss <keyword>

# Check if package is installed
pacman -Qs <package-name>
```

### Common WSL Daily Packages

```bash
# Basic dev tools
sudo pacman -S git curl wget nano neovim

# Compilation tools (required for AUR)
sudo pacman -S base-devel gcc make cmake
```


## 3\. AUR Package Installation \(Most Software\)


Arch official repo lacks many apps \(e\.g\., nodejs latest, telegram, vscode\)\. Use **yay** \(best AUR helper for WSL\)\.

```
sudo sed -i 's/SandboxUsers = alpm/#SandboxUsers = alpm/' /etc/pacman.conf
```

### Step 1: Install Yay AUR Helper

```bash
# Install git & base-devel first
sudo pacman -S --needed git base-devel

# Clone yay source
git clone https://aur.archlinux.org/yay.git
cd yay

# Build & install
makepkg -si

# Return to home folder
cd ~ && rm -rf yay
```

### Step 2: Use Yay to Install AUR Packages

```bash
# Install AUR package (NO sudo needed!)
yay -S <aur-package-name>

# Update ALL packages (official + AUR)
yay -Syu

# Search AUR packages
yay -Ss <keyword>
```

## 4\. Full Package Management Cheat Sheet

```bash
# Upgrade entire system
sudo pacman -Syu    # Official only
yay -Syu             # Official + AUR

# Uninstall package (keep dependencies)
sudo pacman -R <package>

# Uninstall package + unused dependencies
sudo pacman -Rns <package>

# Clean cache packages
sudo pacman -Sc

# Fix broken database (WSL common error)
sudo pacman -Scc
```

## 5\. WSL2 Exclusive Errors \& Fixes

### Error 1: Keyring timeout / no keys

**Fix**: Re\-init keyring

```bash
sudo pacman-key --init
sudo pacman-key --populate archlinux
sudo pacman -Syu
```

### Error 2: File exists / overwrite conflict

**Fix**: Force overwrite \(safe for WSL\)

```bash
sudo pacman -S --overwrite "*" <package>
```

### Error 3: Yay build failed

**Fix**: Ensure base\-devel is installed

```bash
sudo pacman -S base-devel
```

## 6\. Important WSL Arch Notes

- **Do not use sudo with yay** — AUR compilation prohibits root user

- Always run `pacman -Syu` first before installing new packages

- WSL2 has no hardware kernel drivers, all software packages work normally

- All installed packages reside in your D\-disk WSL vhdx file \(no C disk waste\)

# Arch WSL 换国内源 + 关闭 Landlock + 安装 base-devel 全套命令

## 1、先禁用 pacman Sandbox（解决 Landlock 报错）

bash

运行

```
sudo sed -i 's/SandboxUsers = alpm/#SandboxUsers = alpm/' /etc/pacman.conf
```

## 2、一键替换国内镜像（清华 + 中科大 + 阿里，置顶）

bash

运行

```
#备份原镜像
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
#写入国内源到文件头部
sudo tee /etc/pacman.d/mirrorlist <<'EOF'
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.aliyun.com/archlinux/$repo/os/$arch
EOF
```

## 3、添加 archlinuxcn 国内社区源（可选，装更多软件）

bash

运行

```
sudo tee -a /etc/pacman.conf <<'EOF'
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
SigLevel = Optional TrustAll
EOF
```

## 4、刷新数据库

bash

运行

```
sudo pacman -Syy
```
## 5、安装 git + base-devel（带 --nosandbox 规避 Landlock）

bash

运行

```
sudo pacman -S --needed git base-devel
```

## 6、后续装 yay


```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si --nosandbox
cd ~ && rm -rf yay
```

### 快捷永久别名（以后不用每次加 --nosandbox）

bash

运行

```
echo "alias pacman='pacman --nosandbox'" >> ~/.bashrc
source ~/.bashrc
```

## Step1: Refresh Arch keyring first (core fix for unknown signature error)

bash

运行

```
sudo pacman-key --init
sudo pacman-key --populate archlinux
sudo pacman -Syu archlinux-keyring --noconfirm
```

## Step2: Modify `/etc/pacman.conf` to trust ALL imported signatures permanently

Open config:

bash

运行

```
sudo nano /etc/pacman.conf
```

Find `[options]`, change **SigLevel**:

ini

```
[options]
SigLevel = Required DatabaseOptional TrustAll
```

> `TrustAll`: Any key downloaded into local gnupg keyring is automatically trusted, eliminate `unknown trust` signature error entirely.
> 
> Save: `Ctrl+O → Enter → Ctrl+X`