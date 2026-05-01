# 🔧 SETUP: Dual Boot Linux + Windows (Ubuntu & Fedora)

> **Tags**: #setup #dualboot #ubuntu #fedora #installation #grub
> **Feynman Explain**: Dual boot is like having **two operating systems** on the SAME computer. When you turn on your PC, a menu (called GRUB) asks: "Which OS do you want today?" — Windows or Linux. You pick and go!

---

## ⚠️ IMPORTANT WARNINGS — Read Before You Begin!

```
🔴 BACKUP YOUR DATA FIRST — disk operations can cause data loss
🔴 Disable BitLocker BEFORE installing Linux
🔴 Keep Windows installation media (USB) ready as recovery
🔴 Make sure you have at least 2 hours of uninterrupted time
🔴 Keep laptop plugged in during installation
🔴 Know your Windows product key (save it somewhere safe)
```

---

## 📋 What You Need

| Item | Requirement |
|------|-------------|
| **RAM** | 4GB minimum (8GB+ recommended) |
| **Disk Space** | 50GB+ free for Linux (100GB+ ideal) |
| **USB Drive** | 8GB+ (your data will be ERASED from USB) |
| **Internet** | For downloading ISO and updates |
| **Backup** | External drive or cloud backup |

---

## 🗂️ Pre-Installation: Windows Setup

### Step 1: Check Disk Space and Shrink Windows Partition

```
1. Right-click Start → "Disk Management"
2. Right-click your C: drive → "Shrink Volume"
3. Enter amount to shrink: 51200 MB (50 GB) or more
4. Click "Shrink"
5. You'll now see "Unallocated Space" — Linux goes here!
```

### Step 2: Check Boot Mode (BIOS or UEFI)

```
Win + R → msinfo32 → Enter
Look for: BIOS Mode
→ "UEFI" = modern (most laptops post-2012)
→ "Legacy" = older system
```

> **Note**: UEFI systems need GPT partition table. Legacy BIOS uses MBR.

### Step 3: Disable BitLocker (if enabled)

```
Settings → Update & Security → Device Encryption → Turn Off
OR
Control Panel → BitLocker Drive Encryption → Turn Off
Wait for full decryption before continuing!
```

### Step 4: Disable Fast Startup

```
Control Panel → Power Options → "Choose what the power buttons do"
→ Uncheck "Turn on fast startup"
→ Save changes
```

### Step 5: Disable Secure Boot (sometimes needed)

```
Restart → Enter BIOS (F2/F10/Del/Esc during startup)
→ Security tab → Secure Boot → Disabled
→ Save and Exit (F10)
```

> **Note**: Many modern distros (Ubuntu 22.04+, Fedora 37+) support Secure Boot. Try with it enabled first.

---

## 🖥️ Part A: Dual Boot with UBUNTU

### Step 1: Download Ubuntu ISO

```
Go to: https://ubuntu.com/download/desktop
Download: Ubuntu 22.04 LTS or 24.04 LTS (.iso file)
~2-5 GB download
```

### Step 2: Create Bootable USB

**Using Balena Etcher (Easiest — Recommended):**
```
1. Download: https://www.balena.io/etcher/
2. Open Etcher
3. Click "Flash from file" → Select Ubuntu .iso
4. Click "Select target" → Choose USB drive
5. Click "Flash!" — Wait for completion
6. Your USB is now bootable!
```

**Using Rufus (Windows alternative):**
```
1. Download: https://rufus.ie/
2. Open Rufus (no install needed)
3. Device: Your USB drive
4. Boot selection: Select Ubuntu .iso
5. Partition scheme: GPT (for UEFI) or MBR (for Legacy)
6. Click START → OK to warnings
```

### Step 3: Boot from USB

```
1. Insert bootable USB
2. Restart computer
3. Press boot menu key during startup:
   Dell: F12 | HP: F9 | Lenovo: F12 | ASUS: Esc | Acer: F12
4. Select your USB drive from boot menu
5. Ubuntu loads! (takes 1-2 min)
```

### Step 4: Try or Install Ubuntu

```
On Ubuntu welcome screen:
→ Select "Try Ubuntu" first to test hardware compatibility
→ If everything works: double-click "Install Ubuntu" on desktop
→ OR directly click "Install Ubuntu" on welcome screen
```

### Step 5: Installation Steps

```
1. Language: English → Continue

2. Keyboard Layout: Select your layout → Continue

3. Updates and other software:
   ✅ Normal installation
   ✅ Download updates while installing
   ✅ Install third-party software (for graphics, WiFi)
   → Continue

4. Installation type: ← CRITICAL STEP!
   → Select "Install Ubuntu alongside Windows Boot Manager"
   → This is the SAFE dual boot option!
   
   OR for manual control:
   → "Something else" → manually create partitions (advanced)

5. If "alongside Windows" selected:
   → Drag slider to divide space between Windows and Ubuntu
   → "Install Now" → Continue

6. Time zone: Select your city (India/Kolkata for you)

7. User setup:
   Your name: Gowtham
   Computer name: gowtham-ubuntu
   Username: gowtham
   Password: ****
   → Choose "Require password to log in"
   → Continue

8. Wait 15-30 minutes for installation

9. "Installation Complete" → Restart Now

10. Remove USB when prompted → Press Enter
```

### Step 6: First Boot — GRUB Menu

```
You'll see GRUB menu:
┌─────────────────────────────┐
│ Ubuntu                       │  ← Linux option
│ Advanced options for Ubuntu  │
│ Windows Boot Manager         │  ← Windows option
└─────────────────────────────┘

Use arrow keys + Enter to select
Default will boot after 10 seconds
```

### Step 7: First Ubuntu Login

```bash
# Update immediately
sudo apt update && sudo apt upgrade -y

# Install essentials
sudo apt install -y curl wget git vim build-essential

# Check everything
uname -a
lsb_release -a
```

---

## 🎩 Part B: Dual Boot with FEDORA

### Step 1: Download Fedora ISO

```
Go to: https://getfedora.org/ or https://fedoraproject.org/workstation/
Download: Fedora Workstation XX (latest stable, e.g., Fedora 40)
~2-3 GB download
```

### Step 2: Create Bootable USB (Same as Ubuntu)

```
Use Balena Etcher or Rufus (same process)
Select Fedora .iso instead of Ubuntu .iso
```

### Step 3: Boot from USB

```
Same process as Ubuntu:
Insert USB → Restart → F12/F9/Esc → Select USB
```

### Step 4: Fedora Live Environment

```
On Fedora boot screen:
→ "Start Fedora Workstation Live"
→ Live desktop loads
→ Click "Install to Hard Drive" on desktop
```

### Step 5: Anaconda Installer (Fedora's Installer)

```
1. Language: English → Continue

2. Installation Summary screen appears with dots to configure:

3. Click "Installation Destination":
   → Select your disk
   → Choose "Automatic" for simple dual boot
     OR "Custom" for manual partitioning
   → Done

   IF Windows already installed:
   → Fedora detects it automatically
   → Click "Reclaim Space"
   → Select "Delete" on existing free space / shrink Windows
   → "Reclaim space" → Done

4. Click "Network & Hostname":
   → Turn on network toggle
   → Set hostname: gowtham-fedora
   → Done

5. Click "Begin Installation"

6. While installing, set Root Password and User:
   → Root Password: Set a strong root password
   → User Creation:
      Full name: Gowtham
      Username: gowtham
      ✅ Make this user administrator
      Password: ****

7. Wait 20-40 minutes

8. "Fedora is now installed" → Quit (or Reboot)

9. Remove USB → Reboot
```

### Step 6: Post-Install Fedora Setup

```bash
# Update everything
sudo dnf update -y

# Install essentials
sudo dnf install -y curl wget git vim nano

# Enable RPM Fusion (extra software)
sudo dnf install -y https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# Install media codecs
sudo dnf groupinstall -y "Multimedia"

# Check version
cat /etc/fedora-release
```

---

## 🗃️ Manual Partition Layout (Advanced — Recommended for Control)

When choosing "Something else" (Ubuntu) or "Custom" (Fedora):

```
Recommended partition scheme (UEFI system with GPT):

Partition 1: EFI System Partition
  - Size: 512 MB (if not exists) OR use existing Windows EFI
  - Format: FAT32
  - Mount point: /boot/efi
  - Note: Share with Windows EFI, DO NOT format!

Partition 2: Boot Partition (optional but recommended)
  - Size: 1 GB
  - Format: ext4
  - Mount point: /boot

Partition 3: Root Partition
  - Size: 30-50 GB (minimum)
  - Format: ext4
  - Mount point: /  (root)

Partition 4: Home Partition (recommended)
  - Size: remaining space
  - Format: ext4
  - Mount point: /home

Optional: Swap (modern systems with 8GB+ RAM may skip)
  - Size: = your RAM size (e.g., 8 GB)
  - Format: swap area
```

---

## 🛡️ GRUB Bootloader Configuration

GRUB is installed automatically and detects Windows. To configure:

```bash
# View GRUB config
cat /etc/default/grub

# Edit GRUB settings
sudo nano /etc/default/grub

# Key settings:
GRUB_TIMEOUT=10          # Seconds before default boots
GRUB_DEFAULT=0           # 0 = first option (Linux)

# After editing, update GRUB:
# Ubuntu:
sudo update-grub

# Fedora:
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

### Set Windows as Default Boot (if preferred):

```bash
# Ubuntu
sudo nano /etc/default/grub
# Change: GRUB_DEFAULT=0
# To: GRUB_DEFAULT="Windows Boot Manager (on /dev/sda1)"
sudo update-grub

# OR use os-prober
sudo os-prober
sudo update-grub
```

---

## 🔧 Fixing Common Issues

### "GRUB not showing" after install:

```bash
# Boot into Linux → Run:
sudo grub-install /dev/sda      # Ubuntu
sudo grub2-install /dev/sda     # Fedora
sudo update-grub                 # Ubuntu
sudo grub2-mkconfig -o /boot/grub2/grub.cfg  # Fedora
```

### "Windows missing from GRUB menu":

```bash
# Ubuntu
sudo apt install os-prober
sudo os-prober
sudo update-grub

# Fedora
sudo dnf install os-prober
sudo os-prober
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

### "No bootable device" after install:

```
→ Enter BIOS
→ Boot Order → Move Ubuntu/Fedora above Windows
→ Save and Exit
```

### Fix Windows time after installing Linux:

```bash
# Linux sets hardware clock to UTC, Windows uses local time
# Fix in Linux:
timedatectl set-local-rtc 1 --adjust-system-clock
```

---

## 🔄 Switching Between OS — Daily Use

```
Every time you start PC:
→ GRUB menu appears (10 second timer)
→ ↑↓ arrow keys to select
→ Press Enter
→ Boot into chosen OS

Tips:
• Linux partition is NOT visible from Windows by default
• Windows partition IS visible from Linux (/mnt/... or auto-mounted)
• Files saved in Linux = only accessible in Linux
• Files in Windows (C:) = accessible from Linux!
```

---

## ⚡ Accessing Windows Files from Linux (Dual Boot)

```bash
# List Windows partitions
lsblk
fdisk -l

# Mount Windows partition manually
sudo mkdir /mnt/windows
sudo mount /dev/sda3 /mnt/windows    # Replace sda3 with your Windows partition
ls /mnt/windows                       # Browse Windows files!

# Auto-mount Windows at startup (add to /etc/fstab)
sudo blkid /dev/sda3                  # Get UUID
sudo nano /etc/fstab
# Add: UUID=XXXX /mnt/windows ntfs defaults,uid=1000 0 0
```

---

## 🗑️ Removing Dual Boot (Reverting to Windows Only)

```
1. Boot into Windows
2. Run: diskpart → list disk → select disk 0 → list partition
3. Delete Linux partitions (CAREFUL! Not Windows ones)
4. Extend C: drive to reclaim space
5. Fix boot: 
   Windows Recovery → Command Prompt:
   bootrec /fixmbr
   bootrec /fixboot  
   bootrec /rebuildbcd
```

---

## ✅ Final Comparison: WSL vs Dual Boot

| Feature | WSL | Dual Boot |
|---------|-----|-----------|
| **Setup difficulty** | ✅ Easy | ⚠️ Moderate |
| **Performance** | 90% native | ✅ 100% native |
| **Risk** | ✅ Very low | ⚠️ Data loss possible |
| **Restart needed** | ❌ No | ✅ Yes to switch OS |
| **Full Linux experience** | 90% | ✅ 100% |
| **GPU support** | Limited | ✅ Full |
| **Best for** | Development, learning | Power users, DevOps |

---

## ✅ Summary

1. **Backup data** before ANYTHING — no exceptions
2. Shrink Windows partition first from Disk Management
3. Use **Balena Etcher** to create bootable USB
4. Ubuntu = simpler installer | Fedora = Anaconda installer
5. GRUB = the boot menu that appears every startup
6. Separate `/home` partition = easier OS reinstalls
7. Fix Windows time issue with `timedatectl` after installing Linux

---

**← Back to** [[00_Linux_Master_Course_INDEX]] | **Related →** [[SETUP_Ubuntu_WSL]] | [[SETUP_Fedora_WSL]]
