# Day 12 – Linux Group Management and Permission Management

## 1. Group Management in Linux

In Linux, groups are used to organize users and manage access permissions to files, directories, and other system resources.

Linux stores group-related information in the following files:

- `/etc/group` – stores group information and group members.
- `/etc/gshadow` – stores protected group password information, group administrators, and group members.

---

## 2. Fields of `/etc/group`

The `/etc/group` file contains four fields separated by colons (`:`).

### Example

```text
dev:x:5004:
```

| Field | Value | Description |
|---|---|---|
| Group name | `dev` | Name of the group |
| Password field | `x` | Indicates that protected group password information is stored in `/etc/gshadow` |
| Group ID (GID) | `5004` | Numeric identifier of the group |
| User list | Empty | No supplementary members are listed in this field |

### Syntax

```text
group_name:password:GID:user_list
```

### Important commands

**1. Create a group**

```bash
sudo groupadd dev
```

Creates a group named `dev`.

**2. Modify a group**

```bash
sudo groupmod -n developers dev
```

Renames the existing `dev` group to `developers`.

Other useful options:

- `-n` – change the group name.
- `-g` – change the group ID (GID).

Example:

```bash
sudo groupmod -g 6000 developers
```

Changes the GID of `developers` to `6000`, provided the new GID is available.

**3. Add a user to a group**

```bash
sudo gpasswd -a username groupname
```

Example:

```bash
sudo gpasswd -a pratik dev
```

Adds `pratik` to the supplementary membership of `dev`.

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

Prompts you to set or change the password for the group.

> Note: Group passwords are rarely used on modern Linux systems. Group membership and file permissions are the usual ways to manage access.

---

## 3. Fields of `/etc/gshadow`

The `/etc/gshadow` file contains four fields separated by colons (`:`).

### Example

```text
test:!::pratik
```

| Field | Value | Description |
|---|---|---|
| Group name | `test` | Name of the group |
| Encrypted password | `!` | Indicates that a usable group password is locked or unavailable |
| Administrators | Empty | No group administrators are listed |
| User list | `pratik` | `pratik` is listed as a member of the group |

### Syntax

```text
group_name:password:administrators:members
```

**Remember the difference:**

- **Administrators:** Users permitted to administer the group.
- **Members:** Users who belong to the group.

### Assign a group administrator

```bash
sudo gpasswd -A username groupname
```

Example:

```bash
sudo gpasswd -A pratik test
```

Assigns `pratik` as the administrator of the `test` group, replacing the existing administrator list.

### Verify group information

```bash
cat /etc/group
cat /etc/gshadow
getent group dev
groups username
id username
```

- `cat` displays file contents.
- `getent group` displays information about a group.
- `groups` shows a user's group memberships.
- `id` displays a user's UID, primary GID, and supplementary groups.

**Security note:** `/etc/gshadow` contains protected information. Use `sudo` when necessary, and avoid sharing password hashes or sensitive system-file contents.

---

## 4. Permission Management in Linux

Linux permissions determine who can read, modify, or execute a file or access a directory.

### Types of files in Linux

Linux identifies file types using the first character of the long listing displayed by `ls -l`.

| Symbol | File type | Description |
|---|---|---|
| `-` | Regular file | Text files, scripts, documents, and other ordinary files |
| `d` | Directory | Contains directory entries |
| `l` | Symbolic link | Points to another file or directory |
| `p` | Named pipe (FIFO) | Used for inter-process communication |
| `s` | Socket | Used for communication between processes |
| `c` | Character device | Handles data as a stream of characters or bytes |
| `b` | Block device | Handles data in blocks, such as storage devices |

**Note:** Regular files, directories, and symbolic links are common filesystem objects. Pipes, sockets, and device files are also represented in the filesystem.

---

## 5. Understanding the Output of `ls -l`

### Example

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

### Command to display file permissions

```bash
ls -l
```

To display hidden files as well:

```bash
ls -la
```

---

## 6. Understanding Linux Permissions

Linux permissions are divided into three categories.

| Category | Symbol | Applies to |
|---|---|---|
| User | `u` | Owner of the file |
| Group | `g` | Group associated with the file |
| Others | `o` | All other users |
| All | `a` | User, group, and others |

### Permission symbols

| Permission | Symbol | Meaning for files |
|---|---|---|
| Read | `r` | Read file contents |
| Write | `w` | Modify file contents |
| Execute | `x` | Execute the file |
| No permission | `-` | Permission is not granted |

For directories, read, write, and execute have different practical effects:

- `r` – list directory entries.
- `w` – create, remove, or rename entries, subject to other restrictions.
- `x` – traverse the directory and access entries when other permissions allow it.

### Example: `rw-rw-r--`

```text
-rw-rw-r--
│││││││││└── Others: read
││││││└┴──── Group: read and write
│││└┴─────── Owner: read and write
└─────────── File type: regular file
```

The nine permission characters are divided into three groups:

```text
rw-  rw-  r--
 │    │    │
 u    g    o
```

- Owner (`u`): `rw-`
- Group (`g`): `rw-`
- Others (`o`): `r--`

Therefore, the owner and group can read and write the file, while others can only read it.

---

## 7. Changing Permissions with `chmod`

The `chmod` command changes file and directory permissions.

### Syntax

```bash
chmod [options] permissions filename
```

### Symbolic permission mode

| Operator | Meaning |
|---|---|
| `+` | Add permissions |
| `-` | Remove permissions |
| `=` | Set permissions exactly |

### Examples

**1. Give read permission to everyone**

```bash
chmod a=r trial
```

Sets read-only permissions for the owner, group, and others.

Equivalent:

```bash
chmod ugo=r trial
```

Result:

```text
-r--r--r--
```

**2. Add execute permission to the owner**

```bash
chmod u+x trial
```

**3. Remove write permission from the group**

```bash
chmod g-w trial
```

**4. Give read, write, and execute permissions to everyone**

```bash
chmod a=rwx trial
```

**5. Give read and write permissions to the owner, and read-only permissions to the group and others**

```bash
chmod u=rw,g=r,o=r trial
```

Result:

```text
-rw-r--r--
```

**6. Remove all permissions from others**

```bash
chmod o= trial
```

---

## 8. Numeric Permissions (Octal Mode)

Linux also allows permissions to be set using numbers.

| Permission | Numeric value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |
| No permission (`-`) | 0 |

Add the values to calculate each category's permission number.

| Permission | Calculation | Value |
|---|---|---:|
| `rwx` | 4 + 2 + 1 | 7 |
| `rw-` | 4 + 2 + 0 | 6 |
| `r-x` | 4 + 0 + 1 | 5 |
| `r--` | 4 + 0 + 0 | 4 |
| `---` | 0 + 0 + 0 | 0 |

### Examples

```bash
chmod 755 trial
```

Result:

```text
-rwxr-xr-x
```

```bash
chmod 644 trial
```

Result:

```text
-rw-r--r--
```

```bash
chmod 700 trial
```

Result:

```text
-rwx------
```

**Remember:** The first digit represents the owner, the second represents the group, and the third represents others.

---

## 9. Quick Revision

| Task | Command |
|---|---|
| Create a group | `groupadd groupname` |
| Rename a group | `groupmod -n newname oldname` |
| Change GID | `groupmod -g GID groupname` |
| Add a user to a group | `gpasswd -a username groupname` |
| Remove a user from a group | `gpasswd -d username groupname` |
| Set a group password | `gpasswd groupname` |
| Assign group administrators | `gpasswd -A username groupname` |
| Display file permissions | `ls -l` |
| Change permissions symbolically | `chmod u+x filename` |
| Set read-only permissions for everyone | `chmod a=r filename` |
| Set permissions numerically | `chmod 644 filename` |
| View a user's groups | `groups username` |
| View user and group IDs | `id username` |

### Key points to remember

1. `/etc/group` has four fields: group name, password field, GID, and members.
2. `/etc/gshadow` has four fields: group name, password, administrators, and members.
3. `u`, `g`, and `o` mean owner, group, and others.
4. `r`, `w`, and `x` mean read, write, and execute.
5. `chmod` modifies file and directory permissions.
6. Numeric permission values are `r = 4`, `w = 2`, and `x = 1`.
7. Use `ls -l` to inspect permissions and ownership.
