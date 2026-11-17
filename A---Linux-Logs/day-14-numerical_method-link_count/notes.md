## Numerical Method for Permission Management

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

**Remember**: First digit is for user, second for group, and third for others.

- Changing owner of file:

```bash
chown username filename
```

- Changing group of file:

```bash
chgrp groupname filename
```

---

## Link Count

Types of link:
- Hardlink
- Softlink

**1. Hardlink**

To create hardlink

```bash
ln <source> <destination>
```

- Link count increases
- File type remains same
- If we do changes in linked file it gets reflected in original file
- Inode number is same(`ls -i`)
- If we delete original file, then linked file does not get deleted(acts as a backup)
- Mirror copy of original file

**2. Softlink**

To create softlink

```bash
ln -s <source> <destination>
```

- Link count does not change
- File type changes
- If we do changes in linked file it get reflected in original file
- Inode number changes(`ls -i`)
- If we delete original file, then linked file gets deleted(link breaks if original file is deleted)
- Cannot be treated as backup


Dated: 29 October 2026