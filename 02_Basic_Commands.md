# 02 🔧 Basic Linux Commands

> **Tags**: #linux #commands #basics #navigation #files
> **Feynman Explain**: Linux commands are like English sentences — verb first, then what you want to do. `mkdir projects` = "make directory named projects". Simple!

---

## 🎯 1-4-7 Breakdown

### 1️⃣ THE ONE BIG IDEA
> The terminal is your **superpower** — 1 line can do what takes 10 mouse clicks.

### 4️⃣ FOUR COMMAND CATEGORIES
1. **Navigation** — Know where you are, move around
2. **Create/Delete** — Make files and folders
3. **Copy/Move** — Manage file locations
4. **Info** — Learn about the system

### 7️⃣ SEVEN MUST-KNOW COMMANDS
1. `pwd` — where am I?
2. `ls` — what's here?
3. `cd` — move somewhere
4. `mkdir` — make a folder
5. `touch` — make a file
6. `rm` — delete it
7. `cp / mv` — copy or move

---

## 📁 mkdir — Make Directory

```bash
mkdir projects              # ✅ Creates a folder called "projects"
mkdir -p logs/2023/errors   # ✅ Creates nested folders in ONE command
mkdir dir1 dir2 dir3        # ✅ Creates multiple folders at once
```

> **Feynman**: `mkdir` = "make directory". The `-p` flag = "please make all parent folders too, don't complain if they don't exist"

---

## 📂 cd — Change Directory

```bash
cd projects     # ✅ Move INTO the 'projects' folder
cd ..           # ✅ Move UP one level (go to parent folder)
cd ~            # ✅ Go to your HOME directory (always safe!)
cd -            # ✅ Switch to the PREVIOUS directory (like Back button)
cd /            # ✅ Go to the ROOT of the filesystem
cd /home/user/projects  # ✅ Absolute path (full address)
```

> **Feynman**: `~` = your home folder. `..` = one level up. Think of file system as a tree 🌳 — `..` goes UP a branch.

---

## 📋 ls — List Files

```bash
ls              # ✅ List all non-hidden files in current directory
ls -l           # ✅ Long listing: shows permissions, owner, size, date
ls -lh          # ✅ Human-readable file sizes (KB, MB instead of bytes)
ls -a           # ✅ Shows HIDDEN files too (starting with .)
ls -lt          # ✅ Sort by modification time (newest first)
ls -lstr        # ✅ Sort by size, smallest first
ls /etc         # ✅ List files in a different directory
```

> **Feynman**: `ls -la` is your Swiss Army knife — shows EVERYTHING including hidden files with full details

---

## 📝 touch — Create Empty File

```bash
touch notes.txt             # ✅ Creates an empty file
touch file1.txt file2.txt   # ✅ Creates multiple files at once
touch -t 202301011200 old.txt  # ✅ Create file with custom timestamp
```

---

## 🗑️ rm — Remove Files or Folders

```bash
rm notes.txt        # ✅ Deletes the file 'notes.txt'
rm -r temp/         # ✅ Recursively deletes folder 'temp' and contents
rm -rf temp/        # ✅ Force delete — NO confirmation asked
rm -i file.txt      # ✅ Interactive — asks before deleting
```

> ⚠️ **WARNING**: `rm -rf` is DANGEROUS. There is NO recycle bin in Linux. Once deleted = GONE FOREVER.
> **Feynman**: `-r` = "go into every subfolder". `-f` = "don't ask me, just do it"

---

## 📋 cp — Copy Files

```bash
cp file.txt backup.txt          # ✅ Copy file to same folder
cp file.txt /home/user/docs/    # ✅ Copy file to different folder
cp -r folder/ newfolder/        # ✅ Copy entire folder recursively
cp -v file.txt backup.txt       # ✅ Verbose — shows what's happening
```

---

## 🚚 mv — Move or Rename

```bash
mv old.txt new.txt          # ✅ RENAME a file
mv file.txt /tmp/           # ✅ MOVE file to /tmp
mv folder/ /backup/         # ✅ Move entire folder
```

> **Feynman**: `mv` does BOTH move AND rename. If destination is same folder = rename. If different folder = move.

---

## 📌 pwd — Print Working Directory

```bash
pwd     # ✅ Shows: /home/gowtham/projects  (your current location)
```

---

## 🏠 Understanding Linux Directory Structure

```
/ (Root - top of tree)
├── home/           → User home directories
│   └── gowtham/    → YOUR home (~)
├── etc/            → Config files
├── var/            → Logs, variable data
├── tmp/            → Temporary files
├── usr/            → User programs
├── bin/            → Essential binaries (ls, cp, mv)
└── dev/            → Device files
```

---

## 🔗 Hidden Files in Linux

- Files starting with `.` are **hidden** by default
- Examples: `.bashrc`, `.profile`, `.gitignore`
- View with: `ls -a`

---

## ⚡ Command Combinations (Power Tips)

```bash
mkdir myproject && cd myproject     # Create folder AND enter it
ls -la | grep ".txt"                # List only .txt files
touch {file1,file2,file3}.txt       # Create 3 files at once (brace expansion)
```

---

## ✅ Revision Summary

| Command | What it Does | Key Flag |
|---------|-------------|----------|
| `pwd` | Show current path | — |
| `ls` | List files | `-la` (all + details) |
| `cd` | Navigate | `..` up, `~` home |
| `mkdir` | Create directory | `-p` for nested |
| `touch` | Create empty file | — |
| `rm` | Delete | `-rf` (force+recursive) |
| `cp` | Copy | `-r` for folders |
| `mv` | Move/Rename | — |

---

**← Back to** [[01_What_Is_Linux]] | **Next →** [[03_File_Permissions]]
