# Archiving and Compression

>Archiving comes into picture just because links don't work with directories

## Archiving

Combining of multiple files and directories into a single file (rather Packaging and Backup)

**1. To create archive**

```bash
tar -cvf <dirName.tar> <source>
```

Eg. `tar -cvf opt.tar /opt`

This creates a .tar file named opt.tar i.e. archive of opt.

**2. To extract archive**

```bash
tar  -xvf <dirName.tar>
```

Eg. `tar -xvf otp.tar`


**3. To extract archive at different location**

```bash
tar -xvf <dirName.tar> -C <destination>
```

---

## Compression

Tool or technique to reduce the size of a file

| Command/Tool | TimeTaken | CompressionLevel | Extension | Flags | Decompression |
|---|---|---|---|---|---|
| gzip | Less | Low | .gz | -z | gunzip |
| bzip2 | Medium | Medium | .bz2 | -j | bunzip2 |
| xz | More time | High | .xz | -J | unxz |

**1. To compress file**

```bash
gzip opt.tar
```

**2. To decompress file**
```bash
guzip opt.tar.gz
```

**3. To archive and compress together**
```bash
tar -czf opt.tar.gz /opt
```

**4. To extract and decompress together at the same location**

```bash
tar -xzf opt.tar.gz
```

**5. To extract and decompress together at different location**

```bash
tar -xzf opt.tar.gz -C <destination>
```

>**Note: Compression works only on .tar file**