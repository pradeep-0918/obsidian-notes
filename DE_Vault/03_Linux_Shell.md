# 03 — Linux + Shell Scripting
#linux #bash #shell #cron #ssh #text-processing

**Roadmap Day:** 5 | [[DE_Vault/00_Master_Index]] | Prev → [[02_Python_ETL]] | Next → [[04_Apache_Spark]]

---

## 🗺️ Topic Map

```
Linux + Shell
├── File Operations (find, xargs, rsync, ln)
├── Text Processing (awk, sed, grep, cut, sort)
├── Process Management (nohup, tmux, ps, kill)
├── Disk & Memory (df, du, iotop, free)
├── Shell Scripting (variables, loops, functions)
├── Error Handling (set -euo pipefail, trap)
├── Cron Jobs
├── Networking (ssh, scp, curl, netstat)
└── Log Monitoring
```

---

## 1. File Operations

```bash
# find — search files
find /data -name "*.parquet" -mtime -7          # modified in last 7 days
find /logs -type f -size +100M                   # files larger than 100MB
find /tmp -name "*.tmp" -delete                  # find and delete
find /data -name "*.csv" | xargs wc -l           # count lines in all CSVs

# xargs — build command from stdin
find . -name "*.py" | xargs grep -l "TODO"       # find files containing TODO
ls *.csv | xargs -I {} cp {} /backup/{}          # copy each file to backup
cat files.txt | xargs -P 4 -I {} gzip {}         # parallel gzip with 4 workers

# rsync — smart file sync (only copies changes)
rsync -avz --progress source/ dest/              # local sync with verbose
rsync -avz -e ssh user@host:/remote/ /local/     # sync from remote
rsync -avz --exclude='*.log' --delete src/ dst/  # delete files not in source

# symlinks
ln -s /data/raw/2024-01-01/ /data/latest         # create symlink
ls -la /data/latest                              # see where it points
```

> 💡 **`xargs -P n`:** Run n processes in parallel. Huge speedup for embarrassingly parallel tasks like compression.

> 💡 **`rsync --dry-run`:** Always test rsync with `--dry-run` first to see what would be changed.

---

## 2. Text Processing

### awk — structured text processing
```bash
# Print specific columns (tab-delimited)
awk '{print $1, $3}' data.tsv

# Filter rows + print
awk -F',' '$3 > 1000 {print $1, $3}' sales.csv  # -F sets delimiter

# Sum a column
awk -F',' '{sum += $2} END {print "Total:", sum}' data.csv

# Count unique values in col 1
awk '{print $1}' data.txt | sort | uniq -c | sort -rn

# Print lines between patterns
awk '/START/,/END/' file.log
```

### sed — stream editor
```bash
sed 's/old/new/g' file.txt                    # replace all
sed -i 's/localhost/prod-db/g' config.yml     # in-place edit
sed -n '10,20p' huge_file.txt                 # print lines 10-20
sed '/^#/d' config.txt                        # delete comment lines
sed 's/[[:space:]]//g' file.txt               # remove all whitespace
```

### grep — pattern matching
```bash
grep -E "ERROR|WARN" app.log                  # extended regex, multiple patterns
grep -r "TODO" src/ --include="*.py"          # recursive, filter by extension
grep -c "ERROR" app.log                       # count matching lines
grep -v "DEBUG" app.log                       # invert match (exclude DEBUG)
grep -A 3 -B 2 "Exception" app.log            # 3 lines after, 2 before match
grep -o 'ip=[0-9.]*' access.log               # only print matched part
```

### cut, sort, uniq pipeline
```bash
# Top 10 most common IPs in access log
cut -d' ' -f1 access.log | sort | uniq -c | sort -rn | head -10

# Extract specific CSV column
cut -d',' -f3 data.csv | sort | uniq

# Sort by field 2 numerically, descending
sort -t',' -k2 -rn data.csv
```

---

## 3. Process Management

```bash
# View processes
ps aux | grep python                          # find python processes
ps aux --sort=-%cpu | head -10               # top CPU consumers
top -u dataeng                               # processes for user
htop                                         # interactive process viewer

# Kill processes
kill -9 PID                                  # SIGKILL — force kill
kill -15 PID                                 # SIGTERM — graceful shutdown
pkill -f "python etl.py"                     # kill by process name/pattern

# Run in background
nohup python etl.py > etl.log 2>&1 &         # runs after logout
echo $!                                      # PID of last background process

# tmux — persistent terminal sessions
tmux new -s etl_session                      # create named session
tmux attach -t etl_session                   # reconnect to session
# Inside tmux:
# Ctrl+B D — detach   | Ctrl+B % — split vertical | Ctrl+B " — split horizontal
# Ctrl+B [ — scroll mode | q to exit scroll
```

> 💡 **`nohup` vs tmux:** nohup just keeps process alive; tmux gives you an interactive terminal you can reconnect to. For long ETL runs, tmux is better.

> 💡 **`2>&1`:** Redirects stderr (2) to stdout (1). Without this, error messages won't appear in your log file.

---

## 4. Disk & Memory

```bash
df -h                                         # disk usage of all filesystems
df -h /data                                   # specific path
du -sh *                                      # size of each item in current dir
du -sh /data/raw/* | sort -rh | head -10      # largest directories
du -sh --exclude=".git" .                     # exclude dirs

free -m                                       # memory in MB
free -h                                       # human readable
cat /proc/meminfo                             # detailed memory info

iotop                                         # I/O usage by process (needs sudo)
iostat -x 1                                   # disk I/O stats every 1 second
vmstat 1 5                                    # VM stats: 5 snapshots, 1s apart
```

---

## 5. Shell Scripting

### Script Template
```bash
#!/usr/bin/env bash
set -euo pipefail    # e: exit on error, u: error on undefined var, o: pipe fail
IFS=$'\n\t'          # safer field splitting

# ── Variables ──────────────────────────────────────────────────
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly LOG_FILE="${SCRIPT_DIR}/logs/etl_$(date +%Y%m%d).log"
readonly ENV="${1:-dev}"    # first arg, default "dev"

# ── Functions ───────────────────────────────────────────────────
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

die() {
    log "ERROR: $*"
    exit 1
}

check_dependency() {
    command -v "$1" &>/dev/null || die "Required tool not found: $1"
}

# ── Error trap ──────────────────────────────────────────────────
trap 'die "Script failed at line $LINENO"' ERR
trap 'log "Script interrupted"; exit 1' INT TERM

# ── Main ────────────────────────────────────────────────────────
main() {
    log "Starting ETL pipeline — ENV: $ENV"
    check_dependency python3
    check_dependency psql

    python3 "${SCRIPT_DIR}/etl.py" --env "$ENV" 2>&1 | tee -a "$LOG_FILE"
    log "ETL complete"
}

main "$@"
```

### Loops & Conditionals
```bash
# For loop over files
for file in /data/raw/*.parquet; do
    log "Processing: $file"
    python3 process.py --input "$file"
done

# While loop
while IFS=, read -r date source; do
    python3 etl.py --date "$date" --source "$source"
done < jobs.csv

# If/elif/else
if [[ -f "$FILE" ]]; then
    log "File exists"
elif [[ -d "$DIR" ]]; then
    log "Directory exists"
else
    die "Neither file nor dir found"
fi

# String comparisons
[[ "$ENV" == "prod" ]]     # string equality
[[ "$ENV" != "dev" ]]      # not equal
[[ -z "$VAR" ]]            # empty string
[[ -n "$VAR" ]]            # non-empty string

# Arithmetic
[[ $count -gt 0 ]]
(( count++ ))
result=$(( 100 * processed / total ))
```

### Arrays
```bash
sources=("api" "db" "file")
for source in "${sources[@]}"; do
    echo "$source"
done
echo "Count: ${#sources[@]}"
```

---

## 6. Error Handling

```bash
# set options
set -e          # exit on any error (but beware: not in functions by default)
set -u          # treat unset variables as error
set -o pipefail # fail on pipe errors (grep ... | wc -l → -1 if grep fails)
set -x          # debug mode: print each command (use in dev only)

# trap — run code on signals
trap 'cleanup' EXIT                    # always runs on exit
trap 'die "Error at line $LINENO"' ERR # runs on any error
trap 'log "SIGTERM received"' TERM     # runs on kill signal

cleanup() {
    log "Cleaning up temp files..."
    rm -f /tmp/etl_temp_*
}

# Check exit codes
python3 etl.py || { log "ETL failed"; exit 1; }
python3 etl.py && log "ETL succeeded"
```

> ⚠️ **`set -e` gotcha:** Doesn't trigger on failing commands in `if` conditions or `||` chains. `if grep pattern file; then ...` won't exit even if grep fails.

---

## 7. Cron Jobs

```bash
# Edit crontab
crontab -e    # edit for current user
crontab -l    # list current crontab

# Format: minute hour day-of-month month day-of-week command
# ┌──────── minute (0-59)
# │ ┌────── hour (0-23)
# │ │ ┌──── day of month (1-31)
# │ │ │ ┌── month (1-12)
# │ │ │ │ ┌ day of week (0-7, 0=Sun)
# │ │ │ │ │
  0 6 * * * /home/dataeng/etl.sh >> /var/log/etl_cron.log 2>&1
  */15 * * * * /home/dataeng/healthcheck.sh    # every 15 minutes
  0 0 1 * * /home/dataeng/monthly_report.sh   # 1st of each month midnight
  
# Best practice: use full paths in cron (cron has minimal PATH)
# Redirect output: >> log 2>&1
# Lock to prevent overlapping runs:
flock -n /tmp/etl.lock python3 etl.py || echo "Already running"
```

> 💡 **Cron PATH issue:** Cron runs with minimal environment. Always use full paths (`/usr/bin/python3`) or set PATH at top of script.

---

## 8. Networking

```bash
# SSH
ssh -i ~/.ssh/key.pem user@host              # with key
ssh -L 5432:localhost:5432 user@host         # port forwarding (tunnel DB)
ssh-keygen -t ed25519 -C "your_email"       # generate key pair

# SCP (secure copy)
scp file.csv user@host:/remote/path/
scp -r user@host:/remote/dir/ /local/

# curl
curl -s "https://api.example.com/data" | jq '.results'
curl -X POST -H "Content-Type: application/json" \
     -d '{"key":"value"}' https://api.example.com
curl -o output.parquet https://example.com/data.parquet

# wget
wget -q --show-progress https://example.com/dataset.zip
wget -r --no-parent https://example.com/data/  # recursive download

# Network diagnostics
netstat -tulpn | grep 5432               # what's listening on port 5432
ss -tulpn                                 # modern replacement for netstat
lsof -i :8080                             # process using port 8080
curl -I https://api.example.com           # check headers only
```

---

## 9. Log Monitor Script

```bash
#!/usr/bin/env bash
# monitor_logs.sh — watch log file and alert on errors

set -euo pipefail

LOG_FILE="${1:-/var/log/app.log}"
PATTERN="${2:-ERROR}"
ALERT_EMAIL="${ALERT_EMAIL:-ops@company.com}"

tail -F "$LOG_FILE" | while read -r line; do
    if echo "$line" | grep -qE "$PATTERN"; then
        echo "ALERT: Pattern '$PATTERN' found in $LOG_FILE" >&2
        echo "$line"
        # Send email/Slack notification
        curl -s -X POST "$SLACK_WEBHOOK" \
            -H "Content-Type: application/json" \
            -d "{\"text\": \"🚨 Log alert: $line\"}"
    fi
done
```

---

## 🃏 Interview Cheatsheet
- **`set -euo pipefail`:** `-e` exit on error, `-u` exit on undefined var, `-o pipefail` fail if any pipe command fails.
- **`2>&1`:** Redirect stderr to stdout.
- **`nohup` vs tmux:** nohup keeps process alive after logout; tmux provides interactive reconnectable session.
- **`xargs -P 4`:** Parallel execution with 4 workers.
- **`awk` vs `sed`:** awk for structured column processing; sed for line-level substitution.
- **`flock`:** Prevent cron job overlap using file locking.
