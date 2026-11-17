# Linux & DevOps — Quick Revision Notes (Days 1–19)


## Day 1 — Cloud, DevOps and SDLC

- **Cloud computing:** Delivery of services such as compute, storage, networking and security over a network.
- **DevOps:** Culture and practices that bring Development (Dev) and IT Operations (Ops) together to build, test, release and operate software reliably.
- **Application:** Commonly consists of **frontend → backend → database**.
- **SDLC (Software Development Life Cycle):** Structured process for building and maintaining software, from requirements through deployment and maintenance.
- **SLA:** Service Level Agreement — defines the expected level of service between a provider and customer.

**Remember:** DevOps aims to improve collaboration, automation, delivery speed and reliability.

## Day 2 — Operating Systems and AWS

- **Operating System (OS):** System software that acts as an interface between users/applications and computer hardware. It manages resources and provides an environment for applications.
- Examples: Windows, Linux, macOS, Android.
- **Why AWS?** Broad cloud-service offering, global infrastructure and pay-as-you-go pricing.
- **On-premises:** Infrastructure is hosted and managed in an organisation's own premises/data centre.
- **Cloud:** Infrastructure/services are provided by a cloud provider.

## Day 3 — OS Types, Linux and Architecture

**Types of OS**
- Single-user, multiuser, real-time (RTOS), mobile, server, desktop and embedded OS.

**Linux**
- Linux is technically a **kernel**; distributions combine the Linux kernel with system tools, libraries and applications.
- Examples of distributions: Ubuntu, Debian, Linux Mint, Kali, Fedora, CentOS, Arch, Alpine and SUSE/openSUSE.

**Linux architecture (simplified)**
1. **Hardware:** CPU, RAM, disks and I/O devices.
2. **Kernel:** Manages processes, memory, devices and hardware access.
3. **Shell:** Command interpreter between the user and operating-system services (e.g. Bash, sh, zsh).
4. **Utilities/applications:** Tools and programs such as `ls`, `grep`, editors and browsers.
5. **User:** Person or service interacting with the system.

**Kernel designs**
- **Monolithic:** Many OS services run in kernel space; generally efficient, but a kernel fault can affect the whole system. Linux uses a monolithic design with modular support.
- **Microkernel:** Keeps a small core in kernel space and moves more services to user space; improves isolation but can add communication overhead.

## Day 4 — General Commands

| Command | Use |
|---|---|
| `date` | Display date and time |
| `cat file` | Display file contents |
| `cat /etc/os-release` | Check Linux distribution information |
| `clear` or `Ctrl + L` | Clear terminal display |
| `history` | Show previously executed commands |
| `man command` | Open command manual |
| `command --help` | Show command help/options |
| `free -h` | Show memory usage in human-readable units |
| `free -k`, `free -m`, `free -g` | Show memory in KB, MB or GB |

**Command structure:** `command [options/flags] [arguments]`

- **Option/flag:** Changes command behaviour.
- **Argument:** The item/path on which the command operates.

## Day 5 — Basic File Commands

| Command | Use |
|---|---|
| `touch file.txt` | Create an empty file (or update its timestamp) |
| `touch a.txt b.txt` | Create multiple files |
| `touch file{1..5}` | Create a sequence of files |
| `ls` | List directory contents |
| `rm file.txt` | Remove a file |

## Day 6 — Directory Commands

| Command | Use |
|---|---|
| `mkdir dirname` | Create a directory |
| `mkdir -p parent/child` | Create parent and nested directories as needed |
| `rmdir dirname` | Remove an **empty** directory |
| `cd path` | Change current directory |
| `cd ..` | Move one level up |
| `cd ../..` | Move two levels up |
| `tree` | Display directory structure |

**Remember:** `rmdir` does not remove a non-empty directory.

## Day 7 — Path Types

- **Absolute path:** Full path starting from root `/`; works regardless of current directory. Example: `/home/user/project/app.py`.
- **Relative path:** Path interpreted from the current working directory; usually does not start with `/`. Example: `project/app.py`.
- `.` means current directory.
- `..` means parent directory.
- `~` represents the current user's home directory.

## Day 8 — Remove, Copy and Move

| Command | Use |
|---|---|
| `rm file` | Delete a file |
| `rm -r directory` | Delete a directory and its contents recursively |
| `rm -f file` | Force removal; do not prompt for confirmation |
| `rm -rf directory` | Recursive, forceful deletion |
| `cp source destination` | Copy a file |
| `cp -r source_dir destination` | Copy a directory recursively |
| `mv source destination` | Move or rename a file/directory |
| `mv source .` | Move item into the current directory |

**Safety:** `rm -rf` is destructive. Check the path carefully before running it. Avoid running broad deletion commands such as `rm -rf *` unless you have verified the current directory and every target.

## Day 9 — Linux Directories and File Editing

**Important filesystem directories**

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/bin` | Essential user command binaries (often merged into `/usr/bin`) |
| `/boot` | Bootloader and kernel-related boot files |
| `/dev` | Device files |
| `/etc` | System configuration |
| `/home` | Regular users' home directories |
| `/lib` | Essential shared libraries (often merged into `/usr/lib`) |
| `/media` | Common mount location for removable media |
| `/mnt` | Temporary/manual mount point |
| `/opt` | Optional/add-on application software |
| `/proc` | Virtual filesystem with process/kernel information |
| `/root` | Root user's home directory |
| `/run` | Runtime data |
| `/sbin` | System administration binaries (often merged into `/usr/sbin`) |
| `/srv` | Data served by system services |
| `/sys` | Kernel/device information |
| `/tmp` | Temporary files |
| `/usr` | User applications, libraries and shared data |
| `/var` | Variable data, such as logs and spool files |

**Editors:** `vi`, `vim`, `nano`, `pico`.

**Vim basics**
- Open: `vim filename`
- Press `i` to enter insert mode.
- `Esc` returns to normal mode.
- `:w` save; `:wq` save and quit; `:q!` quit without saving.

## Day 10 — Vim Modes and Shortcuts

| Mode | Purpose / entry |
|---|---|
| Normal mode | Default mode for navigation and editing commands |
| Insert mode | Press `i` from normal mode to type text |
| Command-line mode | Press `:` from normal mode to enter editor commands |
| Visual mode | Press `v` to select text |

**Normal-mode shortcuts**
- `yy` copy current line; `nyy` copy `n` lines.
- `p` paste after/below cursor.
- `dd` delete current line; `ndd` delete `n` lines.
- `yw` copy a word; `dw` delete a word.
- `u` undo; `Ctrl + r` redo.
- `gg` first line; `G` last line; `nG` go to line `n`.

**Insert-mode shortcuts**
- `i` insert before cursor; `a` insert after cursor.
- `I` insert at line start; `A` insert at line end.
- `o` open line below; `O` open line above.
- `s` replace character; `S` replace line.
- `r` replace one character; `R` replace text continuously.

**Command-line shortcuts**
- `:w` save; `:q` quit; `:wq` save and quit; `:q!` quit without saving.
- `/word` search; `:noh` clear search highlighting.
- `:set nu` show line numbers; `:set nonu` hide line numbers.
- Visual selection: `v` character-wise, `V` line-wise, `Ctrl + v` block-wise.

## Day 11 — User Management and `/etc/passwd`

**User types:** regular/local user, root user and system user.

| Command | Use |
|---|---|
| `sudo -i` | Open a root login shell |
| `id` | Show current user's UID, GID and groups |
| `useradd username` | Create a user (low-level utility) |
| `useradd -m username` | Create user with home directory |
| `adduser username` | Interactive user-creation helper on Debian/Ubuntu |
| `su - username` | Switch to user with a login environment |
| `passwd username` | Set/change user's password |
| `deluser username` | Remove user (Ubuntu/Debian); normally retains home |
| `deluser --remove-home username` | Remove user and home directory |

**`useradd` vs `adduser`:** `useradd` is a lower-level command with explicit options; `adduser` is an interactive helper on Debian/Ubuntu.

**Files affected by user/group management**
- `/etc/passwd`: account information; generally readable by users.
- `/etc/shadow`: password hashes and password-aging information; restricted access.
- `/etc/group`: group information.
- `/etc/gshadow`: secure group information; restricted access.

Useful checks: `tail -5 /etc/passwd`, `sudo tail -5 /etc/shadow`, `tail -5 /etc/group`, `sudo tail -5 /etc/gshadow`. `head` shows the beginning; `tail` shows the end.

**`/etc/passwd` format:** `username:password-link:UID:GID:comment:home:shell`

Example: `kali:x:1002:1002:,,,:/home/kali:/bin/bash`

## Day 12 — Advanced User Management and `/etc/shadow`

**Create a customised user**
```bash
sudo useradd -u 3000 -g 2000 -c "dev" -m -d /home/dev -s /bin/sh Denver
```
- `-u`: UID; `-g`: primary GID (group must already exist).
- `-c`: comment; `-m`: create home directory.
- `-d`: home directory path; `-s`: login shell.

**Modify an existing user**
```bash
sudo usermod -u 3000 kali
sudo usermod -s /sbin/nologin kali
```

**`/etc/shadow` fields**
`username:password_hash:last_change:min:max:warning:inactive:expire:reserved`

| Field | Meaning | Related `chage` option |
|---|---|---|
| Last password change | Days since 1 Jan 1970 | `-d` |
| Minimum days | Minimum wait before password can be changed | `-m` |
| Maximum days | Password validity period | `-M` |
| Warning days | Days before expiry to warn user | `-W` |
| Inactive | Days after password expiry before account is disabled | `-I` |
| Account expiry | Account expiry date (day count) | `-E` |

Example: `sudo chage -d 0 username` forces a password change at next login. Use `chage --help` or `man chage` for exact options and semantics.

## Day 13 — Groups, `gshadow` and Permissions

**`/etc/group` format:** `group_name:password:GID:user_list`

Example: `dev:x:5004:pratik`

**Group commands**
```bash
sudo groupadd developers
sudo groupmod -n new_name old_name
sudo groupmod -g 6000 developers
sudo gpasswd -a username groupname
sudo gpasswd -d username groupname
sudo usermod -aG groupname username
sudo adduser username groupname
```
- `-aG` appends a supplementary group. Do not omit `-a` when adding a supplementary group with `usermod`, or existing supplementary memberships may be replaced.
- `gpasswd -A username groupname` assigns group administrators.
- `gpasswd groupname` sets/changes a group password.

**`/etc/gshadow` format:** `group_name:password:administrators:members`

**File-type symbols in `ls -l`**
- `-` regular file; `d` directory; `l` symbolic link.
- `p` named pipe; `s` socket; `c` character device; `b` block device.

**Permission categories**
- `u`: owner/user; `g`: group; `o`: others; `a`: all.
- `r`: read; `w`: write; `x`: execute.

For a regular file: read = view contents, write = modify contents, execute = run it.
For a directory: read = list names, write = create/delete/rename entries (subject to other rules), execute = traverse/access entries.

Example permission string: `-rw-rw-r--`
- First character: file type.
- Next three: owner permissions.
- Next three: group permissions.
- Last three: others' permissions.

**Permission commands**
```bash
ls -l
chmod u+x script.sh
chmod g-w file.txt
chmod 755 script.sh
chown username file.txt
chgrp groupname file.txt
```

## Day 14 — Numeric Permissions and Links

**Numeric permissions**

| Permission | Value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |
| None (`-`) | 0 |

Add values for each category:
- `rwx` = 7; `rw-` = 6; `r-x` = 5; `r--` = 4; `---` = 0.
- `chmod 755 file` means owner `rwx`, group `r-x`, others `r-x`.
- `chmod 644 file` means owner `rw-`, group `r--`, others `r--`.

**Hard link**
```bash
ln source destination
```
- Another directory entry for the same inode/data.
- Same inode number; link count increases.
- Changes through either name affect the same underlying file.
- Removing one name does not remove data while another hard link remains.
- Normally cannot cross filesystems or link directories.

**Soft/symbolic link**
```bash
ln -s source destination
```
- Separate inode that points to the target path.
- Can point to directories and cross filesystems.
- If target is removed/moved, the symlink can become broken.
- `ls -i` displays inode numbers; `ls -l` shows symlink target.

## Day 15 — Archiving and Compression

- **Archiving:** Package multiple files/directories into one archive; it does not necessarily reduce size.
- **Compression:** Reduce file size.

**tar**
```bash
tar -cvf archive.tar source       # Create archive
tar -tvf archive.tar              # List archive contents
tar -xvf archive.tar              # Extract archive
tar -xvf archive.tar -C /path     # Extract into another directory
```
Flags: `c` create, `x` extract, `t` list, `v` verbose, `f` archive filename.

**Compression formats**
| Tool | Extension | Decompress |
|---|---|---|
| gzip | `.gz` | `gunzip file.gz` |
| bzip2 | `.bz2` | `bunzip2 file.bz2` |
| xz | `.xz` | `unxz file.xz` |

**Create compressed tar archives**
```bash
tar -czvf archive.tar.gz directory/   # gzip
tar -cjvf archive.tar.bz2 directory/  # bzip2
tar -cJvf archive.tar.xz directory/   # xz
```
Extract with `tar -xzvf`, `tar -xjvf` or `tar -xJvf` respectively.

## Day 16 — `find`, `cat`, `grep`, `sort`, `uniq`, `wc`

**find**
```bash
find . -type f                 # Regular files
find . -type d                 # Directories
find . -type l                 # Symbolic links
find /home -user username      # Owned by user
find . -name "*.txt"           # Case-sensitive name search
find . -iname "README"         # Case-insensitive name search
find . -mtime -1               # Modified within the last 24 hours
find . -mmin -30               # Modified within the last 30 minutes
find . -type f -name "*.log"   # Combine filters
```
- `-mtime`: content modification age in days; `-mmin`: minutes.
- `-ctime`/`-cmin`: inode/status change time, not creation time.
- `-atime`/`-amin`: last access time.
- `+n`: older than `n` units; `-n`: newer than `n` units; `n`: age in the corresponding whole-unit range.
- Inspect matches before using `-delete`.

**cat and redirection**
```bash
cat file.txt
cat first.txt second.txt
cat first.txt second.txt > combined.txt
cat >> notes.txt
```
- `>` creates or overwrites a file.
- `>>` appends without removing existing content.
- Press `Ctrl + D` to finish terminal input to `cat`.

**grep**
```bash
grep "error" app.log
grep -i "error" app.log       # Ignore case
grep -n "root" file.txt       # Include line numbers
grep -v "^#" config.txt       # Exclude matching lines
grep -r "TODO" ./project      # Recursive search
grep -c "error" app.log       # Count matching lines
grep -w "root" file.txt       # Whole-word match
grep -l "error" *.log         # Print matching filenames
```

**sort**
```bash
sort file.txt       # Alphabetical/lexicographic
sort -r file.txt    # Reverse order
sort -n numbers.txt # Numeric order
sort -u file.txt    # Sort and remove duplicate lines
sort -f file.txt    # Ignore case
```

**uniq**
```bash
uniq file.txt
uniq -c file.txt    # Count adjacent duplicates
uniq -d file.txt    # Only repeated adjacent lines
uniq -u file.txt    # Only non-repeated adjacent lines
sort file.txt | uniq
sort file.txt | uniq -c
```
`uniq` detects adjacent duplicates; use `sort` first to group duplicates that occur anywhere in a file.

**wc**
```bash
wc file.txt
wc -l file.txt  # Lines
wc -w file.txt  # Words
wc -c file.txt  # Bytes
wc -m file.txt  # Characters
```

**Pipe (`|`):** Sends the standard output of one command as input to another command.

## Day 17 — Processes

- **Process:** An instance of a running program.
- **Foreground:** Runs attached to the terminal; the shell waits for it.
- **Background:** Runs without occupying the terminal.
```bash
sleep 600 &
jobs
fg %1
```
`fg` uses the shell job ID (shown by `jobs`), not necessarily the PID.

**Process types:** parent, child, daemon and zombie.

**View processes**
```bash
ps
ps -e
ps -f
ps -elf
ps -elf | less
```
- `ps`: process status; `-e`: all processes; `-l`: long format; `-f`: full format.
- `| less` allows page-by-page viewing; press `q` to exit.

**Important `ps -elf` fields**
- `UID`: owner; `PID`: process ID; `PPID`: parent PID.
- `PRI`: priority; `NI`: nice value; `STIME`: start time.
- `TTY`: controlling terminal; `TIME`: CPU time; `CMD`: command.

## Day 18 — Process Management

**Monitor processes**
```bash
top
```
- Shows processes and resource usage in real time.
- Load average represents runnable/uninterruptible tasks averaged over 1, 5 and 15 minutes; interpret it relative to CPU-core count.
- CPU fields: `us` user space, `sy` system, `ni` niced tasks, `id` idle, `wa` I/O wait, `hi` hardware interrupts, `si` software interrupts, `st` stolen by hypervisor.

**Common process states**
| State | Meaning |
|---|---|
| `R` | Running or runnable |
| `S` | Interruptible sleep |
| `D` | Uninterruptible sleep, commonly I/O wait |
| `T` | Stopped |
| `Z` | Zombie (exited; parent has not collected status) |
| `I` | Idle kernel thread |

**Signals and process control**
```bash
kill PID          # Sends SIGTERM by default
kill -15 PID      # Request graceful termination
kill -9 PID       # Force termination (SIGKILL)
kill -STOP PID    # Stop process
kill -CONT PID    # Continue stopped process
pgrep processname # Find process IDs by name
pkill processname # Send signal to processes by name
```
Prefer graceful termination (`SIGTERM`) before `SIGKILL`. `kill -9` does not give a process a chance to clean up.

## Day 19 — Networking and Cron

**Network:** Two or more connected devices that communicate and share resources.

**Network types**
- `PAN`: Personal Area Network.
- `LAN`: Local Area Network.
- `MAN`: Metropolitan Area Network.
- `WAN`: Wide Area Network.

**Addresses**
- **IP address:** Logical network address; IPv4 is 32-bit and IPv6 is 128-bit.
- **MAC address:** Link-layer hardware/interface address; standard MAC address is 48-bit.

**Useful networking commands**
```bash
ping google.com                 # Test reachability
ip addr                         # Show Linux network interfaces/IPs
ip route                        # Show routes/default gateway
ipconfig                        # Windows IP configuration
nslookup google.com             # DNS lookup
dig google.com                  # Detailed DNS query
wget https://example.com/file   # Download a file
curl https://example.com        # Make an HTTP request/show response
curl -O https://example.com/file.zip # Save using remote filename
```

**Cron jobs:** Scheduled commands/scripts that run automatically.

Install and manage cron on Ubuntu/Debian:
```bash
sudo apt install cron -y
sudo systemctl start cron
sudo systemctl stop cron
sudo systemctl restart cron
sudo systemctl status cron
sudo systemctl enable cron
```

**Crontab commands**
```bash
crontab -e   # Edit current user's schedule
crontab -l   # List current user's jobs
crontab -r   # Remove ALL current user's cron jobs — use carefully
```

**Cron expression format**
```text
minute hour day-of-month month day-of-week command
```
Example:
```cron
0 9 * * 1-5 /path/to/script.sh
```
Runs at 9:00 AM, Monday to Friday. Use `crontab.guru` to validate expressions.

---

## High-Value Differences to Remember

| Compare | Difference |
|---|---|
| Absolute vs relative path | Starts from `/` vs starts from current working directory |
| `rm` vs `rmdir` | Removes files vs removes empty directories |
| `cp` vs `mv` | Copies vs moves/renames |
| `>` vs `>>` | Overwrites vs appends |
| `useradd` vs `adduser` | Low-level account utility vs interactive Debian/Ubuntu helper |
| `/etc/passwd` vs `/etc/shadow` | Account metadata vs password hashes/aging |
| `/etc/group` vs `/etc/gshadow` | Group membership metadata vs secure group data/admins |
| Hard link vs symlink | Same inode/data vs separate inode pointing to a path |
| Archive vs compression | Package files together vs reduce data size |
| `mtime` vs `ctime` | Content modification time vs inode/status change time |
| `sort` vs `uniq` | Sorts lines vs handles adjacent duplicate lines |
| Job ID vs PID | Shell job-control identifier vs system process identifier |
| `SIGTERM` vs `SIGKILL` | Graceful termination request vs immediate forced termination |

## 15-Minute Practical Revision

Try these without looking at the notes:

1. Create `practice/logs` using one `mkdir` command.
2. Create three files with `touch`.
3. Copy one file, rename another with `mv`, and remove a test file.
4. Find all `.txt` files under the practice directory.
5. Search a file for a word using `grep -in`.
6. Sort a file and count duplicates using `sort | uniq -c`.
7. Create a user and a group; add the user to the group.
8. Set permissions to `750` and explain each digit.
9. Create a hard link and a symbolic link; compare with `ls -li`.
10. Create and extract a `.tar.gz` archive.
11. Start `sleep 300` in the background, find its PID, and terminate it gracefully.
12. Check IP address, default route and DNS resolution.
13. Explain a cron expression and list your crontab.

## Safety Reminders

- Verify the current directory with `pwd` before deleting or moving files.
- Be extremely careful with `rm -rf`, `find ... -delete` and `crontab -r`.
- Use `sudo` only when administrative privileges are required. If already logged in as root, `sudo` is generally unnecessary.
- Never share password hashes, private keys, tokens or secrets from system files.
- Prefer `kill PID`/`kill -15 PID` before `kill -9 PID`.
- Check command manuals with `man command` when uncertain.
