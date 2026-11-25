# AWS EBS (Elastic Block Store)

## 1. What is EBS?

**EBS (Elastic Block Store)** is a block-level storage service provided by AWS for EC2 instances.

- It provides persistent storage for EC2 instances.
- It behaves like a virtual hard disk attached to an EC2 instance.
- Data stored in EBS volumes persists independently of the running state of an EC2 instance.
- It is commonly used for operating systems, databases, applications and file storage.
- EBS volumes are created in a particular Availability Zone (AZ) and can be attached to EC2 instances in the same AZ.

**Example:** An EC2 instance may have one EBS volume for the operating system and another EBS volume for application data.

### Important Terms

| Term | Meaning |
|---|---|
| Block Storage | Data is stored in fixed-size blocks. |
| Volume | A virtual hard disk attached to an EC2 instance. |
| IOPS | Input/Output Operations Per Second. |
| Throughput | Amount of data transferred per second. |
| AZ | Availability Zone where the EBS volume is created. |

---

## 2. EBS Volume Types

Amazon EBS provides two main categories of storage:

1. SSD (Solid State Drive)
2. HDD (Hard Disk Drive)

### 2.1 SSD Volumes

SSD volumes are suitable for workloads requiring frequent read/write operations and high IOPS.

**A. General Purpose SSD (gp2)**

- Provides a balance between price and performance.
- IOPS performance depends on volume size.
- Supports burst performance.
- Suitable for general-purpose applications and boot volumes.
- Maximum performance: 16,000 IOPS and 250 MiB/s throughput.

**B. General Purpose SSD (gp3)**

- Latest generation of General Purpose SSD.
- Provides a baseline of 3,000 IOPS and 125 MiB/s throughput.
- IOPS and throughput can be provisioned independently of storage size.
- Maximum performance: 80,000 IOPS and 2,000 MiB/s throughput.
- Suitable for databases, applications and boot volumes.

**C. Provisioned IOPS SSD (io1)**

- Designed for I/O-intensive workloads.
- Allows us to provision the required IOPS.
- Suitable for high-performance databases and enterprise applications.
- Maximum performance: 64,000 IOPS and 1,000 MiB/s throughput.
- Supports Multi-Attach.

**D. Provisioned IOPS SSD (io2 Block Express)**

- Designed for mission-critical and high-performance workloads.
- Provides very high IOPS and low latency.
- Suitable for large databases and demanding enterprise applications.
- Maximum performance: 256,000 IOPS and 4,000 MiB/s throughput (with supported Nitro-based instances).
- Supports Multi-Attach.

### 2.2 HDD Volumes

HDD volumes are suitable for workloads that require high sequential data throughput rather than high random IOPS.

**A. Throughput Optimized HDD (st1)**

- Designed for frequently accessed, throughput-intensive workloads.
- Suitable for big data, data warehouses and log processing.
- Maximum throughput: 500 MiB/s.
- Not supported as a boot volume.

**B. Cold HDD (sc1)**

- Designed for infrequently accessed datasets.
- Provides low-cost storage for large datasets.
- Suitable for archival-style workloads where access is less frequent.
- Maximum throughput: 250 MiB/s.
- Not supported as a boot volume.

**C. Magnetic (standard)**

- Previous-generation EBS volume type.
- Uses magnetic disk technology.
- Suitable for workloads where performance is not a priority.
- Provides approximately 100 IOPS on average.
- Supports boot volumes.

---

## 3. EBS Volume Types – Quick Comparison

| Volume Type | Maximum IOPS | Maximum Throughput | Bootable |
|---|---:|---:|---|
| gp2 | 16,000 | 250 MiB/s | Yes |
| gp3 | 80,000 | 2,000 MiB/s | Yes |
| io1 | 64,000 | 1,000 MiB/s | Yes |
| io2 Block Express | 256,000 | 4,000 MiB/s | Yes |
| st1 | 500* | 500 MiB/s | No |
| sc1 | 250* | 250 MiB/s | No |
| Magnetic | 40–200 | 40–90 MiB/s | Yes |

*For HDD volumes, the IOPS figures are based on 1 MiB I/O operations. Actual performance depends on volume configuration and workload.*

**Important:**
- SSD is generally preferred for boot volumes.
- HDD is generally preferred for large sequential data workloads.
- `io1` and `io2` support Multi-Attach.
- `st1` and `sc1` cannot be used as boot volumes.

---

## 4. EBS Volume Performance Hierarchy

The following diagram summarizes the maximum performance and important characteristics.

```text
                    EBS VOLUMES
                         |
             +-----------+-----------+
             |                       |
            SSD                     HDD
             |                       |
       +-----+------+          +-----+-----+
       |            |          |           |
      gp2          gp3        st1         sc1
       |            |          |           |
   16K IOPS      80K IOPS    500 MiB/s  250 MiB/s
   250 MiB/s    2000 MiB/s
       |
       +----------------+
                        |
                  Provisioned IOPS
                        |
                   +----+----+
                   |         |
                  io1       io2
                   |         |
                64K IOPS  256K IOPS
                1000 MB/s 4000 MB/s
                   |
                   +---- Multi-Attach
                        (io1 and io2)
```

**Note:** The diagram groups volume types by their purpose; it does not represent a literal performance ranking. Multi-Attach is supported by both `io1` and `io2`, subject to supported instance and configuration requirements.

---

## 5. Device Name in Linux

When attaching an EBS volume to an EC2 instance, AWS asks for a device name.

Example:

```text
/dev/sdf
```

- `/dev/sdf` is a commonly used device name when attaching an additional EBS volume.
- It identifies the requested device mapping.
- On modern Nitro-based EC2 instances, the actual Linux device name may appear as `/dev/nvme1n1` or another NVMe device.

**Important:** Always verify the actual device name using `lsblk` before performing partitioning or formatting.

---

## 6. Steps to Start Working with an EBS Volume

After creating and attaching an EBS volume to an EC2 instance, we need to prepare it before storing data.

```text
Create EBS Volume
       |
       v
Attach Volume to EC2
       |
       v
Identify Block Device
       |
       v
Create Partition
       |
       v
Format Partition
       |
       v
Create Mount Directory
       |
       v
Mount Partition
       |
       v
Start Storing Data
```

### Step 1: Create an EBS Volume

1. Open AWS EC2 Dashboard.
2. Navigate to Elastic Block Store → Volumes.
3. Click Create Volume.
4. Select the volume type (e.g., gp3).
5. Specify the volume size.
6. Select the same Availability Zone as your EC2 instance.
7. Click Create Volume.

### Step 2: Attach the Volume

1. Select the newly created volume.
2. Click Actions → Attach Volume.
3. Select the EC2 instance.
4. Specify a device name, for example `/dev/sdf`.
5. Click Attach Volume.

### Step 3: Identify the Block Device

Connect to your Ubuntu EC2 instance and execute:

```bash
lsblk
```

This command lists all block devices attached to the system.

Example:

```text
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
nvme0n1     259:0    0    8G  0 disk
├─nvme0n1p1 259:1    0  7.9G  0 part /
└─nvme0n1p15 259:2   0  100M 0 part /boot/efi
nvme1n1     259:3    0   10G  0 disk
```

Here:
- `nvme0n1` represents the root EBS volume.
- `nvme1n1` represents the newly attached EBS volume.
- `nvme1n1` does not have a partition yet.

**Important:** Device names in your instance may differ. Never assume that the new volume is always `/dev/nvme1n1`.

### Step 4: Create a Partition

Use the `fdisk` command:

```bash
sudo fdisk /dev/nvme1n1
```

Inside the fdisk interface, use:

| Command | Purpose |
|---|---|
| `n` | Create a new partition |
| `p` | Select primary partition (if prompted) |
| `1` | Partition number |
| Enter | Accept default first sector |
| Enter | Accept default last sector (use all available space) |
| `w` | Write changes and exit |

After partitioning, check:

```bash
lsblk
```

Expected output:

```text
nvme1n1
└─nvme1n1p1
```

Here, `nvme1n1p1` is the newly created partition.

### Step 5: Format the Partition

Formatting creates a filesystem on the partition.

For Ubuntu, we commonly use the ext4 filesystem.

```bash
sudo mkfs.ext4 /dev/nvme1n1p1
```

Other filesystem examples:

```bash
sudo mkfs.xfs /dev/nvme1n1p1
sudo mkfs.ext4 /dev/nvme1n1p1
```

**Note:** Formatting an existing partition can erase its data. Make sure you have selected the correct partition.

### Step 6: Check the Filesystem

Use:

```bash
sudo blkid
```

This command displays information about block devices, including:
- UUID
- Filesystem type
- Partition details

Example:

```text
/dev/nvme1n1p1: UUID="xxxx-xxxx" TYPE="ext4"
```

### Step 7: Create a Mount Directory

A mount point is a directory through which we access the filesystem.

Create a directory:

```bash
sudo mkdir /data
```

### Step 8: Mount the Partition

Execute:

```bash
sudo mount /dev/nvme1n1p1 /data
```

Now the EBS volume is accessible through `/data`.

### Step 9: Verify the Mount

Use:

```bash
df -h
```

This command displays filesystem disk usage in a human-readable format.

Example:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme1n1p1   10G   24K   9.4G   1% /data
```

You can now create files and directories inside `/data`.

Example:

```bash
sudo touch /data/test.txt
ls -l /data
```

---

## 7. Important Linux Commands for EBS

| Command | Purpose |
|---|---|
| `lsblk` | Lists block devices and their partitions |
| `sudo fdisk <device>` | Creates or manages disk partitions |
| `sudo mkfs.ext4 <partition>` | Formats a partition using ext4 |
| `sudo mkfs.xfs <partition>` | Formats a partition using XFS |
| `sudo blkid` | Displays UUID and filesystem information |
| `df -h` | Displays mounted filesystem disk usage |
| `sudo mount <device> <directory>` | Mounts a filesystem |
| `sudo umount <directory>` | Unmounts a filesystem |
| `sudo mkdir <directory>` | Creates a mount-point directory |
| `lsblk -f` | Displays devices, filesystems and UUIDs |
| `sudo fdisk -l` | Lists disk partition information |

---

## 8. Make the EBS Volume Mount Automatically After Reboot

By default, a manually mounted EBS volume may not automatically mount after an EC2 reboot.

To make the mount persistent, configure `/etc/fstab`.

### Step 1: Find the UUID

```bash
sudo blkid /dev/nvme1n1p1
```

Example:

```text
UUID="xxxx-xxxx" TYPE="ext4"
```

### Step 2: Edit fstab

```bash
sudo nano /etc/fstab
```

Add the following entry using your actual UUID:

```text
UUID=xxxx-xxxx  /data  ext4  defaults,nofail  0  2
```

Explanation:

| Field | Meaning |
|---|---|
| UUID | Unique identifier of the partition |
| `/data` | Mount point |
| `ext4` | Filesystem type |
| `defaults,nofail` | Default mount options; do not prevent boot if the volume is unavailable |
| `0` | Dump backup option |
| `2` | Filesystem check order |

### Step 3: Test the Configuration

Before rebooting, execute:

```bash
sudo mount -a
```

If no errors occur, verify:

```bash
df -h
```

The EBS volume should now mount automatically after reboot.

**Important:** Always use the partition UUID instead of relying on a device name that may change.

---

## 9. Important Points to Remember

1. EBS is persistent block-level storage for EC2 instances.
2. EBS volumes must be created in the same Availability Zone as the EC2 instance to which they are attached.
3. An EBS volume can generally be attached to one EC2 instance at a time.
4. Multi-Attach allows supported `io1` and `io2` volumes to be attached to multiple instances simultaneously.
5. `st1` and `sc1` are not bootable.
6. `gp2` and `gp3` are suitable for general-purpose workloads.
7. `io1` and `io2` are suitable for I/O-intensive workloads.
8. EBS volumes can persist even after an EC2 instance is stopped or terminated, depending on the volume's Delete on Termination setting.
9. Always check the device name using `lsblk` before formatting.
10. Use `/etc/fstab` to configure automatic mounting after reboot.

---

**Dated:** 25 November 2026