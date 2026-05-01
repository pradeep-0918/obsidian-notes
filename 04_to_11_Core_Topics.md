# 04 ⚙️ Process Management

> **Tags**: #linux #processes #ps #top #kill
> **Feynman Explain**: Processes are like **workers in a factory**. `ps` = check who's working. `top` = see who's using most resources. `kill` = fire a worker. `nohup` = tell a worker to keep working even after you leave.

---

## 🎯 1-4-7 Breakdown

### 1️⃣ ONE BIG IDEA
> Every running program = a **process** with a unique **PID** (Process ID)

### 4️⃣ FOUR TOOLS
1. `ps` — snapshot of processes
2. `top` — live dashboard
3. `kill` — stop a process
4. `nohup` — run even after logout

---

## 🔍 ps — View Running Processes

```bash
ps              # Show processes for current terminal session
ps aux          # Show ALL processes from ALL users (most useful)
ps aux | grep nginx     # Find the nginx process specifically
ps -ef          # Alternative full listing format
```

### Reading `ps aux` output:
```
USER    PID  %CPU  %MEM    VSZ   RSS  STAT  START  TIME  COMMAND
root      1   0.0   0.1  22548  1234  Ss    10:00  0:01  /sbin/init
gowtham 1234  2.5   1.2 456789 23456  R     10:05  0:30  python app.py
```

---

## 📊 top — Real-Time System Monitor

```bash
top             # Open interactive dashboard
top -u gowtham  # Show only gowtham's processes
```

### Inside `top` — keyboard shortcuts:
| Key | Action |
|-----|--------|
| `q` | Quit |
| `k` | Kill a process (enter PID) |
| `M` | Sort by memory usage |
| `P` | Sort by CPU usage |
| `u` | Filter by user |

---

## 💀 kill — Stop a Process

```bash
kill 1234           # Politely ask process 1234 to stop (SIGTERM)
kill -9 1234        # FORCE kill — no mercy (SIGKILL)
killall firefox     # Kill ALL firefox processes by name
pkill -f "python"   # Kill all processes matching "python"
```

> ⚠️ `kill -9` = last resort. Data may not be saved.

---

## 🔄 nohup — Run After Logout

```bash
nohup python app.py &           # Run in background, survive logout
nohup ./script.sh > output.log & # Save output to log file
```

> **Feynman**: `nohup` = "No Hang Up" — when you log out (hang up the phone), don't stop the process!

---

## ✅ Revision Summary

| Command | Purpose | Key Flag |
|---------|---------|----------|
| `ps aux` | See all processes | `grep` to filter |
| `top` | Live monitor | `q` to quit |
| `kill PID` | Stop process | `-9` for force |
| `nohup` | Background task | `&` to background |

---
---

# 05 💾 Disk and File System Management

> **Tags**: #linux #disk #storage #compression
> **Feynman Explain**: `df` tells you how FULL your house is. `du` tells you which ROOMS are taking up space.

---

## 📊 df — Disk Free

```bash
df -h               # Human-readable: shows MB/GB (not bytes)
df -h /home         # Check specific partition
df -T               # Show filesystem type too
```

```
Filesystem   Size  Used  Avail  Use%  Mounted on
/dev/sda1     50G   20G    28G   42%  /
/dev/sdb1    100G   80G    18G   80%  /home     ← ⚠️ Getting full!
```

---

## 📁 du — Directory Usage

```bash
du -sh foldername       # Size of specific folder (human-readable)
du -h --max-depth=1     # Size of all folders, 1 level deep
du -h --max-depth=1 | sort -hr   # Find BIGGEST folders first
```

---

## 📦 zip / unzip

```bash
zip -r archive.zip folder/      # Compress folder into zip
unzip archive.zip               # Extract zip
unzip archive.zip -d /target/   # Extract to specific location
```

---

## 🗜️ tar — Archive Multiple Files

```bash
# CREATE archive
tar -czvf project.tar.gz folder/    # Compress folder to .tar.gz

# EXTRACT archive
tar -xzvf project.tar.gz            # Extract .tar.gz

# VIEW contents without extracting
tar -tzvf project.tar.gz
```

### tar Flags Decoded:
| Flag | Meaning |
|------|---------|
| `-c` | **C**reate archive |
| `-x` | E**x**tract archive |
| `-z` | Use g**z**ip compression |
| `-v` | **V**erbose (show files) |
| `-f` | **F**ile name follows |

> **Memory trick**: **c**reate/**e**x**t**ract + **z**ip + **v**erbose + **f**ile = `czvf` / `xzvf`

---

## ✅ Summary

| Command | Purpose |
|---------|---------|
| `df -h` | Check disk space |
| `du -sh folder` | Check folder size |
| `zip -r out.zip folder/` | Zip compress |
| `tar -czvf out.tar.gz folder/` | Tar compress |
| `tar -xzvf file.tar.gz` | Tar extract |

---
---

# 06 🔎 File Content Processing

> **Tags**: #linux #grep #awk #text-processing
> **Feynman Explain**: These tools let you READ, SEARCH, FILTER, and ANALYZE files WITHOUT opening them. Imagine having X-ray vision for text files!

---

## 🔍 grep — Search Inside Files

```bash
grep ERROR log.txt          # Find all lines with "ERROR"
grep -i error log.txt       # Case-INSENSITIVE search
grep -v ERROR log.txt       # Show lines WITHOUT "ERROR"
grep -n ERROR log.txt       # Show LINE NUMBERS of matches
grep -r 'ERROR' /var/log    # Search RECURSIVELY in all files
grep -c ERROR log.txt       # COUNT matching lines
grep "^ERROR" log.txt       # Lines STARTING with ERROR
grep "ERROR$" log.txt       # Lines ENDING with ERROR
```

### Real Example:
```bash
# Find failed SSH attempts
grep "Failed password" /var/log/auth.log

# Find all Python errors in logs
grep -r "Traceback" /var/log/myapp/
```

---

## 📐 awk — Text Column Processor

```bash
# Sample file: data.txt
# John 25 Developer
# Asha 30 Designer

awk '{print}' data.txt          # Print all lines (like cat)
awk '{print $1}' data.txt       # Print FIRST column (names)
awk '{print $1, $3}' data.txt   # Print name + job title
awk '$2 > 27 {print $1, $2}' data.txt   # Filter: age > 27
awk '{printf "Name: %s | Age: %s | Role: %s\n", $1, $2, $3}' data.txt
```

> **Feynman**: `awk` sees each line as columns separated by spaces. `$1` = column 1, `$2` = column 2, etc.

---

## 📄 cat — Show File Content

```bash
cat file.txt            # Display entire file
cat file1 file2         # Concatenate two files
cat -n file.txt         # Show with line numbers
cat > newfile.txt       # Create file (type content, Ctrl+D to save)
```

---

## ⬆️ head — Show First N Lines

```bash
head file.txt           # First 10 lines (default)
head -n 5 file.txt      # First 5 lines only
head -n 20 access.log   # Preview large log files quickly
```

---

## ⬇️ tail — Show Last N Lines

```bash
tail file.txt           # Last 10 lines (default)
tail -n 15 file.txt     # Last 15 lines
tail -f log.txt         # 🔥 LIVE VIEW — updates as file changes (great for logs!)
```

> **Feynman**: `tail -f` is like watching a TV broadcast of your log file in real time!

---

## 📊 wc — Word/Line Count

```bash
wc file.txt         # Shows: lines, words, bytes
wc -l file.txt      # Count only LINES
wc -w file.txt      # Count only WORDS
wc -c file.txt      # Count only CHARACTERS
ls | wc -l          # Count how many files are in current directory
```

---

## 🔤 sort — Sort File Content

```bash
sort names.txt          # Alphabetical (A→Z)
sort -r names.txt       # Reverse (Z→A)
sort -n marks.txt       # Numeric sort (by number)
sort -nr marks.txt      # Numeric, reverse (highest first)
sort -u names.txt       # Remove duplicates (unique)
sort -nr marks.txt | head -n 5  # Top 5 highest values
```

---

## 🔎 find — Locate Files/Directories

```bash
find . -name "*.log"            # Find all .log files recursively
find /home -type f -size +10M   # Files over 10MB in /home
find . -mtime -1                # Files modified in last 1 day
find . -empty                   # Find all empty files
find . -name "*.tmp" -delete    # Find AND delete .tmp files
find . -name "*.log" -exec rm {} \;  # Find and remove .log files
find /etc -name "*.conf" -type f     # Config files only
```

---

## ✅ Summary

| Tool | Purpose | Key Use |
|------|---------|---------|
| `grep` | Search text | `-i` case-insensitive, `-r` recursive |
| `awk` | Process columns | `$1`, `$2` for columns |
| `cat` | View files | `-n` for line numbers |
| `head` | First N lines | `-n 20` for 20 lines |
| `tail` | Last N lines | `-f` for live view |
| `wc` | Count things | `-l` lines, `-w` words |
| `sort` | Sort content | `-n` numeric, `-r` reverse |
| `find` | Locate files | `-name`, `-size`, `-mtime` |

---
---

# 07 🌐 Remote Access & File Transfer

> **Tags**: #linux #ssh #scp #remote #putty
> **Feynman Explain**: SSH is like a **secure telephone call** to another computer. SCP is like a **secure courier** that delivers files between computers.

---

## 🔑 SSH — Secure Shell

```bash
ssh username@ip_address             # Basic login
ssh gowtham@192.168.1.10            # Example
ssh -p 2222 gowtham@server.com      # Custom port
ssh -i ~/.ssh/mykey.pem user@server # Use private key file
```

### Setting up SSH Keys (more secure than password):
```bash
ssh-keygen -t rsa -b 4096           # Generate key pair
ssh-copy-id user@server             # Copy public key to server
ssh user@server                     # Now login WITHOUT password!
```

---

## 📤 SCP — Secure Copy

```bash
# Upload: local → remote
scp file.txt user@server:/home/user/

# Download: remote → local
scp user@server:/var/log/app.log ./

# Copy entire folder
scp -r myfolder/ user@server:/home/user/

# Custom port
scp -P 2222 file.txt user@server:/tmp/
```

---

## 🖥️ PuTTY (Windows GUI for SSH)

1. Download from https://www.putty.org/
2. Open PuTTY
3. Enter server IP in "Host Name"
4. Select SSH, Port 22
5. Click "Open" → Enter username + password

---

## 📁 WinSCP (Windows GUI for SCP)

1. Download from https://winscp.net/
2. Open WinSCP
3. Fill: Hostname (server IP), Username, Password
4. Click "Login"
5. Drag-drop files between local ↔ server

---

## ✅ Summary

| Tool | Purpose | Platform |
|------|---------|----------|
| `ssh` | Secure remote login | Terminal (all OS) |
| `scp` | Secure file transfer | Terminal (all OS) |
| PuTTY | SSH GUI | Windows |
| WinSCP | SCP/SFTP GUI | Windows |

---
---

# 08 📦 Package Management & Networking

> **Tags**: #linux #apt-get #wget #packages
> **Feynman Explain**: `apt-get` is Linux's **App Store** — but free, command-line, and way faster!

---

## 📦 apt-get — APT Package Manager (Debian/Ubuntu)

```bash
sudo apt-get update                     # Refresh package list (ALWAYS first!)
sudo apt-get upgrade                    # Update ALL installed packages
sudo apt-get install git                # Install a package
sudo apt-get install git curl wget      # Install multiple packages
sudo apt-get remove apache2             # Remove package (keep config)
sudo apt-get purge apache2              # Remove package + config files
sudo apt-get autoremove                 # Clean unused dependencies
sudo apt-get clean                      # Free space (delete .deb cache)
```

> ✅ **Golden Rule**: Always run `sudo apt-get update` BEFORE installing anything!

---

## 🌐 wget — Download from the Web

```bash
wget https://example.com/file.zip           # Download file
wget -c https://example.com/largefile.iso   # Continue interrupted download
wget -O myname.zip https://example.com/f.zip  # Save with custom name
wget -b https://example.com/file.zip        # Download in background
wget -r https://example.com/               # Recursive download (whole site)
wget --limit-rate=100k https://url.com     # Limit download speed
```

---

## ✅ Summary

| Command | Purpose |
|---------|---------|
| `apt-get update` | Refresh repo index |
| `apt-get install` | Install package |
| `apt-get remove` | Uninstall package |
| `apt-get upgrade` | Update all packages |
| `wget URL` | Download from internet |

---
---

# 09 🤖 Shell Scripting

> **Tags**: #linux #bash #scripting #automation
> **Feynman Explain**: A shell script is a **recipe card** for your computer. Instead of typing 10 commands one by one, write them all in a `.sh` file and run once!

---

## 📝 Creating Your First Script

```bash
# Step 1: Create file
nano hello.sh

# Step 2: Write script
#!/bin/bash
echo "Hello, World!"
echo "Today is: $(date)"

# Step 3: Make executable
chmod +x hello.sh

# Step 4: Run it
./hello.sh
```

---

## 📥 Variables

```bash
#!/bin/bash
NAME="Gowtham"
AGE=25

echo "My name is $NAME"
echo "I am $AGE years old"
```

---

## 📨 Passing Arguments

```bash
#!/bin/bash
# Save as greet.sh

echo "Script Name: $0"
echo "First Argument: $1"
echo "Second Argument: $2"
echo "Hello, $1! Your role is $2."
```

```bash
./greet.sh Gowtham DataEngineer
# Output:
# Script Name: ./greet.sh
# First Argument: Gowtham
# Second Argument: DataEngineer
# Hello, Gowtham! Your role is DataEngineer.
```

### Special Variables:
| Variable | Meaning |
|----------|---------|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$#` | Total number of arguments |
| `$@` | All arguments (separate strings) |
| `$*` | All arguments (single string) |

---

## 🔀 Conditionals

```bash
#!/bin/bash
if [ -z "$1" ]; then
    echo "Please provide a name"
    exit 1
fi

if [ "$1" == "admin" ]; then
    echo "Welcome, admin!"
else
    echo "Hello, $1!"
fi
```

---

## 🔁 Loops

```bash
#!/bin/bash
# For loop
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# Loop through files
for file in *.txt; do
    echo "Processing: $file"
done

# While loop
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

---

## 📖 Reading User Input

```bash
#!/bin/bash
echo "Enter your name:"
read NAME
echo "Hello, $NAME!"
```

---

## 🤖 Real-World Automation Script

```bash
#!/bin/bash
# Backup script
SOURCE=$1
BACKUP_DIR=$2

if [ -z "$SOURCE" ] || [ -z "$BACKUP_DIR" ]; then
    echo "Usage: ./backup.sh <source> <destination>"
    exit 1
fi

tar -czvf "$BACKUP_DIR/backup_$(date +%Y%m%d).tar.gz" "$SOURCE"
echo "Backup complete!"
```

```bash
./backup.sh /home/ubuntu/mydata /backup_folder
```

---

## ✅ Summary

| Concept | Syntax |
|---------|--------|
| Shebang | `#!/bin/bash` |
| Variable | `NAME="value"` |
| Use variable | `$NAME` |
| Arguments | `$1`, `$2`, `$#` |
| If condition | `if [ condition ]; then ... fi` |
| For loop | `for i in list; do ... done` |
| Make executable | `chmod +x script.sh` |
| Run script | `./script.sh` |

---
---

# 10 ⏰ Cron Jobs and Automation

> **Tags**: #linux #cron #automation #scheduling
> **Feynman Explain**: Cron is like setting an **alarm clock** for your computer. "At 2 AM every day, run this backup script" — and it just does it, even while you sleep!

---

## 📅 Cron Syntax

```
* * * * * command_to_run
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, 0=Sunday)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

---

## 📝 Common Cron Examples

```bash
# Every minute
* * * * * /script.sh

# Every hour at minute 0
0 * * * * /script.sh

# Every day at 2:30 AM
30 2 * * * /backup.sh

# Every Sunday at midnight
0 0 * * 0 /weekly_report.sh

# Every 5 minutes
*/5 * * * * /check_status.sh

# First day of every month at 9 AM
0 9 1 * * /monthly_invoice.sh
```

---

## 🛠️ Setting Up Cron Jobs

```bash
# Step 1: Install cron
sudo apt update && sudo apt install cron

# Step 2: Start cron service
sudo systemctl enable cron
sudo systemctl start cron

# Step 3: Edit crontab
crontab -e      # Opens editor for YOUR cron jobs
crontab -l      # List current cron jobs
crontab -r      # Remove ALL cron jobs (careful!)

# Step 4: Add a job (inside crontab -e)
30 2 * * * /home/gowtham/backup.sh >> /var/log/backup.log 2>&1
```

---

## 🪲 Debugging Cron Jobs

```bash
# Check cron is running
systemctl status cron

# View cron logs
grep CRON /var/log/syslog
cat /var/log/cron.log

# Test script manually first!
bash /home/gowtham/backup.sh

# Log cron output to file
* * * * * /script.sh >> /tmp/myscript.log 2>&1
```

---

## ✅ Summary

| Pattern | Meaning |
|---------|---------|
| `* * * * *` | Every minute |
| `0 * * * *` | Every hour |
| `0 2 * * *` | Daily at 2 AM |
| `0 0 * * 0` | Weekly (Sunday midnight) |
| `0 0 1 * *` | Monthly (1st day) |
| `*/5 * * * *` | Every 5 minutes |

---
---

# 11 ⚡ Productivity & Power Usage

> **Tags**: #linux #pipes #redirection #aliases #productivity

---

## 🔗 Pipes — Chain Commands

```bash
ls -la | grep ".txt"            # List files, filter for .txt
ps aux | grep nginx             # Find nginx process
cat log.txt | grep ERROR | wc -l  # Count ERROR lines
history | grep ssh              # Find past ssh commands
```

> **Feynman**: Pipe `|` = "take the output of this, and FEED it as input to the next command"

---

## ➡️ Redirection

```bash
# Output to file (overwrite)
echo "Hello" > file.txt
ls > filelist.txt

# Output to file (APPEND — don't overwrite)
echo "World" >> file.txt
date >> log.txt

# Input from file
sort < names.txt

# Send errors to file
command 2> errors.log

# Send both output AND errors to file
command > output.log 2>&1
```

---

## 🔗 Command Chaining

```bash
# && = run next ONLY if first succeeds
sudo apt update && sudo apt upgrade

# ; = run next regardless of success/failure
mkdir test; cd test; touch file.txt

# || = run next ONLY if first FAILS
ping server || echo "Server is down!"
```

---

## ⚡ Aliases — Create Shortcuts

```bash
# Temporary alias (only for current session)
alias ll='ls -la'
alias update='sudo apt update && sudo apt upgrade'

# Permanent alias (add to ~/.bashrc)
nano ~/.bashrc
# Add: alias ll='ls -la'
source ~/.bashrc    # Reload settings
```

---

## ✅ Summary

| Concept | Symbol | Use |
|---------|--------|-----|
| Pipe | `\|` | Chain commands |
| Overwrite | `>` | Write to file |
| Append | `>>` | Add to file |
| Input | `<` | Read from file |
| AND chain | `&&` | Run if success |
| OR chain | `\|\|` | Run if failure |
| Any chain | `;` | Run regardless |

---

**← Back to** [[00_Linux_Master_Course_INDEX]]
