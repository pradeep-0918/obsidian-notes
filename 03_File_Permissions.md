# 03 🔐 Linux File Permissions

> **Tags**: #linux #permissions #chmod #chown #security
> **Feynman Explain**: Permissions are like **locks on doors**. Each file has 3 doors: one for the Owner, one for the Group, one for Everyone else. You decide who can read, write, or enter (execute).

---

## 🎯 1-4-7 Breakdown

### 1️⃣ THE ONE BIG IDEA
> Every file/folder has 3 permission types × 3 user categories = **9 permission bits**

### 4️⃣ FOUR THINGS TO KNOW
1. **Permission Types** — Read, Write, Execute
2. **User Categories** — Owner, Group, Others
3. **Numeric system** — rwx = 4+2+1 = 7
4. **chmod/chown** — change permissions/ownership

### 7️⃣ SEVEN OCTAL VALUES TO MEMORIZE
0 = --- | 1 = --x | 2 = -w- | 3 = -wx | 4 = r-- | 5 = r-x | 6 = rw- | **7 = rwx (FULL)**

---

## 🔑 Permission Types

| Permission | Symbol | Value | Meaning |
|------------|--------|-------|---------|
| **Read** | `r` | 4 | View file contents |
| **Write** | `w` | 2 | Modify/edit contents |
| **Execute** | `x` | 1 | Run as program / enter directory |

> **Feynman**: Think of permissions like a permission slip. `r` = can read it, `w` = can edit it, `x` = can run it (or walk into a folder)

---

## 👥 User Categories

| Category | Symbol | Who |
|----------|--------|-----|
| **User (Owner)** | `u` | The person who created/owns the file |
| **Group** | `g` | Users in the file's assigned group |
| **Others** | `o` | Everyone else (except root) |

---

## 🔢 Numeric (Octal) System

```
Permission = r + w + x
           = 4 + 2 + 1

rwx = 7   (4+2+1) → Full access
rw- = 6   (4+2+0) → Read + Write
r-x = 5   (4+0+1) → Read + Execute
r-- = 4   (4+0+0) → Read only
-wx = 3   (0+2+1) → Write + Execute
-w- = 2   (0+2+0) → Write only
--x = 1   (0+0+1) → Execute only
--- = 0   (0+0+0) → No access
```

### Reading a 3-digit permission number:

```
chmod 755 script.sh
      ↑↑↑
      |||→ Others  : 5 = r-x (read + execute)
      ||→  Group   : 5 = r-x (read + execute)
      |→   Owner   : 7 = rwx (full access)
```

---

## 📖 Reading `ls -l` Output

```
drwxr-xr-x  2  gowtham  staff  4096  Apr 18  mydir
-rw-r--r--  1  gowtham  staff   256  Apr 18  file.txt
```

```
Position breakdown:
[d]  [rwx]  [r-x]  [r-x]
 ↑     ↑      ↑      ↑
 |     |      |      |
 |     |      |      └── Others permissions
 |     |      └───────── Group permissions
 |     └──────────────── Owner permissions
 └────────────────────── File type: d=directory, -=file, l=symlink
```

### File Type Prefix Characters

| Symbol | Meaning |
|--------|---------|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |

---

## ⚙️ chmod — Change Permissions

```bash
# Numeric method (most common)
chmod 777 file.txt      # Full access for EVERYONE (dangerous!)
chmod 755 script.sh     # Owner: all | Group+Others: read+execute
chmod 644 file.txt      # Owner: rw | Others: read only (most common for files)
chmod 444 report.txt    # Read-only for ALL (nobody can change)
chmod 600 secret.txt    # Owner read+write only, others: NOTHING

# Symbolic method
chmod +x file.sh        # Add execute permission for ALL
chmod u+w file.txt      # Add write for owner (user) only
chmod g-w file.txt      # Remove write from group
chmod o-rwx file.txt    # Remove all permissions from others
chmod a+r file.txt      # Add read for ALL (a = all)

# Recursive (for folders)
chmod -R 755 myfolder/  # Apply to folder and ALL contents inside
```

### Common Permission Patterns

| Pattern | Octal | Use Case |
|---------|-------|----------|
| `rwxr-xr-x` | `755` | Scripts, executables |
| `rw-r--r--` | `644` | Regular text files |
| `rw-rw-r--` | `664` | Shared group files |
| `rwx------` | `700` | Private scripts |
| `rw-------` | `600` | Private files (SSH keys!) |
| `rwxrwxrwx` | `777` | ⚠️ Avoid — full access all |

---

## 👤 chown — Change Ownership

```bash
chown gowtham file.txt              # Change owner to 'gowtham'
chown gowtham:developers file.txt   # Change owner AND group
chown -R gowtham myproject/         # Recursive ownership change
chown :staff file.txt               # Change ONLY the group
```

---

## 👑 Special Notes on root

- `root` bypasses ALL permission checks
- Always use `sudo` for elevated commands
- Never run as root permanently — too risky
- `sudo command` = run as root temporarily

---

## 🔗 Hard Links vs Soft Links

```bash
ln original.txt link.txt        # Hard link
ln -s original.txt shortcut.txt # Soft link (symlink)
```

| Feature | Hard Link | Soft Link (Symlink) |
|---------|-----------|---------------------|
| Points to | Same **inode** (data) | File **path** |
| Disk space | No extra space | Tiny pointer |
| If original deleted | ✅ File survives | ❌ Link breaks |
| Cross filesystem | ❌ No | ✅ Yes |
| Link to folders | ❌ No | ✅ Yes |

---

## 🧪 Quick Practice Examples

```bash
# Check current permissions
ls -l myfile.txt

# Make a script executable
chmod +x deploy.sh
./deploy.sh                 # Now you can run it!

# Secure your SSH private key (REQUIRED)
chmod 600 ~/.ssh/id_rsa

# Set web server file permissions
chmod 755 /var/www/html/
chmod 644 /var/www/html/index.html
```

---

## ✅ Revision Summary

1. **3 permissions**: Read(4), Write(2), Execute(1)
2. **3 categories**: Owner(u), Group(g), Others(o)
3. **Octal**: r=4, w=2, x=1 → Add them up per category
4. **755** = most common for executables (owner full, rest read+exec)
5. **644** = most common for text files (owner rw, rest read only)
6. **600** = SSH keys, private files (owner only)
7. **chmod** changes permissions; **chown** changes ownership

---

**← Back to** [[02_Basic_Commands]] | **Next →** [[04_Process_Management]]
