# 🔧 SETUP: Ubuntu on WSL (Windows Subsystem for Linux)

> **Tags**: #setup #wsl #ubuntu #windows #installation
> **Feynman Explain**: WSL is like installing a **Linux engine INSIDE Windows** — no separate computer needed! You get a full Ubuntu terminal running right inside Windows 10/11.

---

## 🎯 1-4-7 Breakdown

### 1️⃣ ONE BIG IDEA
> WSL = Run real Linux commands INSIDE Windows. No VM, no dual boot, no separate machine needed.

### 4️⃣ FOUR PHASES
1. **Enable WSL** in Windows
2. **Install Ubuntu** from Microsoft Store
3. **Configure** user + update packages
4. **Customize** terminal + tools

### 7️⃣ SEVEN THINGS YOU'LL GET
1. Full Bash terminal
2. apt-get package manager
3. File access from both Windows and Linux
4. Python, Node, Git, etc. natively
5. VS Code integration
6. SSH capability
7. Docker support (WSL 2)

---

## 📋 Prerequisites

- ✅ Windows 10 (version 2004+) or Windows 11
- ✅ 64-bit system
- ✅ Virtualization enabled in BIOS
- ✅ Admin access

### Check your Windows version:
```
Win + R → type: winver → Press Enter
```

---

## 🚀 Method 1: One-Command Install (EASIEST — Windows 11 / Win 10 Updated)

### Step 1: Open PowerShell as Administrator

```
Start Menu → Search "PowerShell" → Right-click → "Run as Administrator"
```

### Step 2: Install WSL + Ubuntu in ONE command

```powershell
wsl --install
```

> This automatically:
> - Enables WSL feature
> - Enables Virtual Machine Platform
> - Installs WSL 2 kernel
> - Installs Ubuntu (default distro)

### Step 3: Restart your computer

```
Restart now — required!
```

### Step 4: First Launch Setup

- Ubuntu opens automatically after restart
- Enter new **UNIX username** (can be different from Windows)
- Enter **password** (invisible while typing — that's normal!)
- Confirm password

### Step 5: You're in! Update Ubuntu immediately

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🔧 Method 2: Manual Install (Older Windows 10)

### Step 1: Enable WSL Feature

Open PowerShell as Administrator:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### Step 2: Enable Virtual Machine Platform

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### Step 3: Restart Windows

```
Restart your computer now
```

### Step 4: Download and Install WSL 2 Linux Kernel

```
Download from: https://aka.ms/wsl2kernel
Run the installer (WSL2 update package)
```

### Step 5: Set WSL 2 as Default

```powershell
wsl --set-default-version 2
```

### Step 6: Install Ubuntu from Microsoft Store

```
1. Open Microsoft Store
2. Search: "Ubuntu"
3. Choose version: Ubuntu 22.04 LTS (recommended) or Ubuntu 24.04 LTS
4. Click "Get" / "Install"
5. Click "Open" after installing
```

### Step 7: First-Time Setup

```
Enter username: gowtham          ← pick your username
Enter password: (invisible)      ← type your password
Confirm password:
```

### Step 8: Update System

```bash
sudo apt update
sudo apt upgrade -y
```

---

## ⚙️ Essential Post-Install Setup

### Install common tools:

```bash
# Developer essentials
sudo apt install -y curl wget git vim nano unzip

# Build tools (needed for many packages)
sudo apt install -y build-essential

# Python
sudo apt install -y python3 python3-pip

# Node.js (via nvm - recommended)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install --lts

# Check everything
python3 --version
git --version
node --version
```

---

## 🗂️ File System — IMPORTANT!

### Accessing Linux files from Windows:

```
Windows Explorer address bar: \\wsl$\Ubuntu\home\yourusername
```

### Accessing Windows files from Linux:

```bash
# Windows C: drive is at:
ls /mnt/c/

# Your Windows Desktop:
ls /mnt/c/Users/YourWindowsName/Desktop/

# Navigate to Windows folder
cd /mnt/c/Users/YourWindowsName/Documents/
```

> ⚠️ **Performance Tip**: Store Linux projects INSIDE Linux filesystem (`~/projects/`), not on `/mnt/c/`. Windows filesystem access is slower!

---

## 🖥️ Setting Up Windows Terminal (Recommended)

```
1. Open Microsoft Store
2. Search: "Windows Terminal"
3. Install it
4. Open Windows Terminal
5. Click ▼ dropdown → Select "Ubuntu"
```

**Customize Windows Terminal:**
- Set Ubuntu as default: Settings → Startup → Default Profile → Ubuntu
- Change color scheme: Settings → Profiles → Ubuntu → Appearance

---

## 🔗 VS Code Integration

```bash
# Install VS Code on Windows first, then:
code .      # Opens current folder in VS Code from Ubuntu terminal!
```

VS Code auto-installs the "WSL" extension and connects seamlessly.

---

## 🐳 Docker with WSL 2

```
1. Download Docker Desktop for Windows
2. Settings → General → "Use WSL 2 based engine" ✅
3. Settings → Resources → WSL Integration → Enable Ubuntu ✅
4. In Ubuntu terminal: docker run hello-world
```

---

## 🔄 Useful WSL Management Commands (in PowerShell)

```powershell
wsl --list --verbose        # List installed distros + WSL version
wsl --status                # Check WSL status
wsl --set-version Ubuntu 2  # Convert Ubuntu to WSL 2
wsl --shutdown              # Shut down all WSL instances
wsl                         # Launch default distro
wsl -d Ubuntu               # Launch Ubuntu specifically

# Export distro backup
wsl --export Ubuntu ubuntu-backup.tar

# Import backup
wsl --import Ubuntu C:\WSL\Ubuntu ubuntu-backup.tar
```

---

## ⚡ Quick Customization (.bashrc)

```bash
nano ~/.bashrc

# Add these at the bottom:
# Better prompt with colors
PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# Useful aliases
alias ll='ls -la'
alias update='sudo apt update && sudo apt upgrade -y'
alias projects='cd ~/projects'
alias ..='cd ..'
alias ...='cd ../..'

# Save and reload
source ~/.bashrc
```

---

## 🔧 Troubleshooting Common Issues

| Problem | Solution |
|---------|----------|
| "Virtualization not enabled" | Restart → Enter BIOS → Enable VT-x/AMD-V |
| "WslRegisterDistribution failed" | Run PowerShell as Admin, retry |
| Ubuntu stuck on install | `wsl --unregister Ubuntu` then reinstall |
| Slow file access | Move files to Linux filesystem, avoid `/mnt/c/` |
| Can't ping internet | `sudo nano /etc/resolv.conf` → add `nameserver 8.8.8.8` |
| Password forgotten | PowerShell: `ubuntu config --default-user root` → reset |

---

## ✅ Verification Checklist

```bash
# Run these to verify everything works:
whoami              # Should show your username
uname -a            # Should show Linux kernel info
lsb_release -a      # Should show Ubuntu version
df -h               # Check disk space
ping google.com     # Check internet (Ctrl+C to stop)
```

---

## 📊 WSL 1 vs WSL 2 Comparison

| Feature | WSL 1 | WSL 2 |
|---------|-------|-------|
| Architecture | Translation layer | Real Linux kernel |
| Performance | Moderate | ✅ Much faster |
| Docker support | ❌ Limited | ✅ Full support |
| Full system call | ❌ No | ✅ Yes |
| File I/O (Linux) | Faster | Slower (use /home) |
| Use case | Simple scripts | Development, Docker |

> **Recommendation**: Always use **WSL 2** for development

---

## ✅ Final Summary

1. `wsl --install` = one command to rule them all (Win 10/11 updated)
2. Ubuntu 22.04 LTS = most stable choice for beginners
3. Always update after install: `sudo apt update && sudo apt upgrade`
4. Store projects in `~/` NOT in `/mnt/c/` (performance!)
5. Use Windows Terminal + VS Code for best experience
6. WSL 2 > WSL 1 for everything development-related

---

**← Back to** [[00_Linux_Master_Course_INDEX]] | **Related →** [[SETUP_Fedora_WSL]] | [[SETUP_DualBoot]]
