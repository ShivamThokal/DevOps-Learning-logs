# Fields of `/etc/group`

```text
`kali:x:5004:dev`
```

- groupname --> `kali`
- link to password --> `x`
- groupId --> `5004`
- userlist --> `dev`


### Syntax

```text
group_name:password:GID:user_list
```

## Important Commands:

**1. To create a group:**
```bash
groupadd kali
```

creates a group named `kali`

**2. To modify fields:**
```bash
groupmod -n developers dev`
``` 

change groupname to `developers` from `dev`

```bash
groupmod -g 6000 developers
```

Changes the GID of `developers` to `6000`

**3. Add a user to a group**

```bash
sudo gpasswd -a username groupname
```

Example:

```bash
sudo gpasswd -a pratik dev
```

Adds `pratik` to the supplementary membership of `dev`.

#### Other ways to add user to group


using usermod




```bash
usermod -aG dev pratik
```

This will add `pratik` to `dev` group


using adduser


```bash
adduser pratik dev
```

This will add `pratik` to `dev` group


**4. Remove a user from a group**

```bash
sudo gpasswd -d username groupname
```

Example:

```bash
sudo gpasswd -d pratik dev
```

Removes `pratik` from `dev`.

**5. Set or change a group password**

```bash
sudo gpasswd groupname
```

Example:

```bash
sudo gpasswd dev
```

---

# Fields of `/etc/gshadow`

```text
test:!:kali:pratik
```

- groupname --> `test`
- encrypted password --> `!`
- admin --> `kali`
- userlist --> `pratik`


### Syntax

```text
group_name:password:administrators:members
```

### Assign a group administrator

```bash
sudo gpasswd -A username groupname
```

Example:

```bash
sudo gpasswd -A pratik test
```

---

# Permission Management in Linux

Linux permissions determine who can read, modify, or execute a file or access a directory.

### Types of files

##### User defined files
- Normal files (-)
- Directory (d)
- Linked files (l)
##### System defined files
- Pipe device file (P)
- Socket device file (s)
- Character device file (c)
- Block device file (b)


| Symbol | File type | Description |
|---|---|---|
| `-` | Regular file | Text files, scripts, documents, and other ordinary files |
| `d` | Directory | Contains directory entries |
| `l` | Symbolic link | Points to another file or directory |
| `p` | Named pipe (FIFO) | Used for inter-process communication |
| `s` | Socket | Used for communication between processes |
| `c` | Character device | Handles data as a stream of characters or bytes |
| `b` | Block device | Handles data in blocks, such as storage devices |


### Output of `ls -l`

```text
-rw-rw-r-- 1 vboxuser vboxuser 0 Oct 28 06:12 trial
```

The output contains the following fields:

| Field | Value | Meaning |
|---|---|---|
| File type | `-` | Regular file |
| Permissions | `rw-rw-r--` | Access permissions |
| Link count | `1` | Number of hard links |
| Owner | `vboxuser` | User who owns the file |
| Group | `vboxuser` | Group associated with the file |
| File size | `0` | File size in bytes |
| Timestamp | `Oct 28 06:12` | Last modification time displayed |
| Filename | `trial` | Name of the file |


### Understanding Linux Permissions

Linux permissions are divided into three categories.

| Category | Symbol | Applies to |
|---|---|---|
| User | `u` | Owner of the file |
| Group | `g` | Group associated with the file |
| Others | `o` | All other users |
| All | `a` | User, group, and others |


The nine permission characters are divided into three groups:

```text
rw-  rw-  r--
 │    │    │
 u    g    o
```

- Owner (`u`): `rw-`
- Group (`g`): `rw-`
- Others (`o`): `r--`

### Changing Permissions

To make modifications in permissions:

```bash
chmod [options] permissions filename
```

### Permission mode
| Operator | Meaning |
|---|---|
| `+` | Add permissions |
| `-` | Remove permissions |
| `=` | Set permissions exactly |

### Examples

```bash
chmod ugo=r notes.txt
```

This will give read permission to user, group and others



> **Note:** To lock and unlock user
| Operation | Command |
|---|---|
| Lock user | `usermod -L pratik |
| Unlock user | `usermod -U pratik` |
| Lock password | `passwd -l pratik` |
| Unlock password | `passwd -u pratik` |
| Check status | `passwd -S pratik` |


> Prefer `usermod -L/U to lock/unlock user`


Dated: 28 October 2026