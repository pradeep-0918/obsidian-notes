# 🔧 SETUP: Fedora on WSL (Windows Subsystem for Linux)

> **Tags**: #setup #wsl #fedora #windows #installation
> **Feynman Explain**: Fedora isn't in the Microsoft Store, so we install it like sneaking a guest through the back door — using a community-built import! It's safe, and Fedora is a **cutting-edge distro** great for learning modern Linux.

---

## 🎯 Why Fedora on WSL?

| Reason | Detail |
|--------|--------|
| **Cutting-edge** | Newest software versions |
| **RPM packages** | Learn `dnf` (used in Red Hat, CentOS) |
| **Enterprise prep** | Red Hat is the enterprise standard |
| **SELinux** | More advanced security model |
| **Systemd** | Modern init system |

> **Note**: Fedora is NOT in Microsoft Store. We use **Fedora WSL** community project.

---

## 📋 Prerequisites

- ✅ WSL 2 already installed (see [[SETUP_Ubuntu_WSL]] first)
- ✅ Windows 10 (2004+) or Windows 11
- ✅ Internet connection
- ✅ PowerShell as Administrator

### Verify WSL 2 is ready:
```powershell
wsl --status
# Should show: Default Version: 2
```

---

## 🚀 Method 1: Using Fedora WSL (Recommended — Easiest)

### Step 1: Download Fedora WSL Release

```
Go to: https://github.com/fedora-cloud/FedoraWSL/releases
Download the latest: FedoraWSL.zip (e.g., Fedora-39.zip or Fedora-40.zip)
```

### Step 2: Extract the ZIP

```
Right-click the downloaded .zip → Extract All
Extract to: C:\WSL\Fedora\  (create this folder)
```

### Step 3: Run the Installer

```
Inside extracted folder:
Double-click → Fedora.exe
```

> This registers Fedora as a WSL distro automatically!

### Step 4: Launch Fedora

```powershell
# In PowerShell:
wsl -d Fedora

# OR search "Fedora" in Start Menu
```

### Step 5: Set Up User (First Launch)

```bash
# You'll be root by default — create a regular user:
useradd -m -G wheel gowtham      # -G wheel = sudo access
passwd gowtham                    # Set password

# Set as default user
echo '[user]' >> /etc/wsl.conf
echo 'default=gowtham' >> /etc/wsl.conf

# Exit and restart Fedora WSL
exit
```

```powershell
# In PowerShell - restart Fedora:
wsl --terminate Fedora
wsl -d Fedora
```

### Step 6: Update Fedora

```bash
sudo dnf update -y
sudo dnf upgrade -y
```

---

## 🔧 Method 2: Manual Import Using Fedora Container Image

### Step 1: Ensure WSL 2 is Set as Default

```powershell
wsl --set-default-version 2
```

### Step 2: Download Fedora Container Base Image

```
Go to: https://koji.fedoraproject.org/koji/packageinfo?packageID=26387
OR: https://mirror.cern.ch/fedora/linux/releases/
Download: Fedora-Container-Base-XX-X.x86_64.tar.xz
(replace XX with version like 39 or 40)
```

### Step 3: Create WSL Directory

```powershell
# In PowerShell:
mkdir C:\WSL\Fedora
```

### Step 4: Extract the Container Image

```powershell
# Extract the .tar.xz file (use 7-Zip if needed)
# Inside, find a file named: layer.tar
# That's the one we import!
```

### Step 5: Import Fedora into WSL

```powershell
wsl --import Fedora C:\WSL\Fedora C:\Downloads\layer.tar --version 2
```

### Step 6: Launch Fedora

```powershell
wsl -d Fedora
```

### Step 7: Install Essential Packages

```bash
# Fix minimal container (missing common tools)
dnf install -y passwd shadow-utils sudo util-linux wget curl

# Create your user
useradd -m -G wheel gowtham
passwd gowtham

# Configure sudoers (if needed)
echo '%wheel ALL=(ALL) ALL' >> /etc/sudoers
```

### Step 8: Set Default User in WSL

```bash
# Create /etc/wsl.conf
cat > /etc/wsl.conf << EOF
[user]
default=gowtham

[boot]
systemd=true
EOF
```

```powershell
# Restart WSL to apply
wsl --terminate Fedora
wsl -d Fedora
```

---

## 🔧 Method 3: Using rancher/distrod (Advanced — Full Systemd)

```powershell
# Install using distrod for full systemd support
# Download from: https://github.com/nullpo-head/wsl-distrod
# Enables: systemctl, journald, proper service management
```

---

## ⚙️ Essential Post-Install Setup (Fedora)

### Update system and install tools:

```bash
# Full system update
sudo dnf update -y

# Essential tools
sudo dnf install -y curl wget git vim nano unzip tar

# Development tools
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y python3 python3-pip

# Node.js
sudo dnf install -y nodejs npm

# Verify
python3 --version
git --version
node --version
```

---

## 📦 DNF vs APT — Key Differences

| Task | Ubuntu (apt) | Fedora (dnf) |
|------|-------------|---------------|
| Update repo | `apt update` | `dnf check-update` |
| Upgrade all | `apt upgrade` | `dnf upgrade` |
| Install | `apt install pkg` | `dnf install pkg` |
| Remove | `apt remove pkg` | `dnf remove pkg` |
| Search | `apt search pkg` | `dnf search pkg` |
| Info | `apt show pkg` | `dnf info pkg` |
| List installed | `dpkg -l` | `dnf list installed` |
| Clean cache | `apt clean` | `dnf clean all` |

> **Pro Tip**: Learning `dnf` prepares you for **Red Hat, CentOS, RHEL, Rocky Linux** — all enterprise-grade distros!

---

## 🔧 DNF Configuration Tips

```bash
# Speed up DNF (add to /etc/dnf/dnf.conf)
sudo nano /etc/dnf/dnf.conf

# Add these lines:
fastestmirror=True
max_parallel_downloads=10
defaultyes=True
keepcache=True
```

---

## 📁 File Access

Same as Ubuntu WSL:

```bash
# Access Windows C drive
ls /mnt/c/

# Your Windows Desktop
ls /mnt/c/Users/YourWindowsName/Desktop/
```

---

## 🔄 Managing Multiple WSL Distros

```powershell
# List all installed distros
wsl --list --verbose

# Example output:
#   NAME      STATE    VERSION
# * Ubuntu    Running  2
#   Fedora    Stopped  2

# Set Fedora as default
wsl --set-default Fedora

# Switch back to Ubuntu as default
wsl --set-default Ubuntu

# Run specific distro
wsl -d Ubuntu
wsl -d Fedora

# Terminate specific distro
wsl --terminate Fedora
```

---

## 💡 Fedora-Specific Power Tips

```bash
# Check Fedora version
cat /etc/fedora-release

# Enable RPM Fusion (extra packages — games, codecs)
sudo dnf install -y https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# Install COPR (community repos — like Ubuntu PPAs)
sudo dnf copr enable username/reponame
sudo dnf install package

# Enable automatic updates
sudo dnf install -y dnf-automatic
sudo systemctl enable --now dnf-automatic.timer
```

---

## 🔧 Troubleshooting Fedora WSL

| Problem | Solution |
|---------|----------|
| "dnf: command not found" | You may be in a container — `dnf install dnf` first |
| "sudo: command not found" | `dnf install sudo` |
| No internet in Fedora | `echo "nameserver 8.8.8.8" > /etc/resolv.conf` |
| systemctl not working | Enable systemd in `/etc/wsl.conf` → `[boot] systemd=true` |
| Import fails | Check file path has no spaces; use 7-Zip to extract |
| User not found | Run setup commands as root first |

---

## ✅ Verification Checklist

```bash
# Run these in Fedora WSL:
whoami                  # Your username
uname -a                # Linux kernel info
cat /etc/fedora-release # Fedora version
dnf --version           # Package manager ready
ping google.com         # Internet works (Ctrl+C to stop)
python3 --version       # Python available
```

---

## ✅ Final Summary

1. Fedora isn't on Microsoft Store — use FedoraWSL release or manual import
2. Use **WSL 2** (not WSL 1) for Fedora
3. Package manager = `dnf` (not `apt-get`)
4. Always run `sudo dnf update -y` after install
5. Add `[boot] systemd=true` to `/etc/wsl.conf` for full systemd support
6. You can run Ubuntu + Fedora SIDE BY SIDE in WSL!

---

**← Back to** [[00_Linux_Master_Course_INDEX]] | **Related →** [[SETUP_Ubuntu_WSL]] | [[SETUP_DualBoot]]
