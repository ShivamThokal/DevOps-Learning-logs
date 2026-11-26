# AWS EBS – Permanent Mounting and Snapshots

## 1. Permanent Mounting of EBS Volumes

When we attach an EBS volume to an EC2 instance and mount it manually, the mount may not persist after a system reboot.

To make the mounting permanent, we configure the `/etc/fstab` file.

### What is /etc/fstab?

- `/etc/fstab` stands for Filesystem Table.
- It contains information about filesystems that should be mounted automatically.
- It is used to configure permanent mounting of storage devices.
- It is generally edited using a text editor such as `nano` or `vim`.

### Step 1: Open the fstab file

```bash
sudo nano /etc/fstab
```

### Step 2: Understand the syntax

General syntax:

```text
<device> <mount_point> <filesystem_type> <mount_options> <dump> <fsck_order>
```

Example:

```text
UUID=084ec3a9-ffc4-42ad-ae62-eb4e559459b9 /mnt ext3 defaults,nofail 0 2
```

### Explanation of Each Field

| Field | Example | Description |
|---|---|---|
| Device | UUID=084ec3a9-... | Identifies the partition |
| Mount Point | `/mnt` | Directory where the partition is mounted |
| Filesystem | `ext3` | Filesystem type |
| Mount Options | `defaults,nofail` | Defines mounting behavior |
| Dump | `0` | Disables dump backup |
| Fsck Order | `2` | Filesystem check order during boot |

**Note:** Using UUID is recommended instead of `/dev/xvdf1` or `/dev/nvme1n1p1`, because device names can change.

### Step 3: Save and exit

For Nano:

- `Ctrl + O` – Save changes.
- `Enter` – Confirm filename.
- `Ctrl + X` – Exit.

### Step 4: Test the configuration

Before rebooting, execute:

```bash
sudo mount -a
```

This command attempts to mount all filesystems configured in `/etc/fstab` that are not already mounted.

If there are no errors, verify using:

```bash
df -h
```

### Important Commands

| Command | Purpose |
|---|---|
| `sudo nano /etc/fstab` | Edit the filesystem table |
| `sudo mount -a` | Mount all applicable filesystems listed in fstab |
| `sudo umount /mnt` | Unmount a filesystem |
| `sudo systemctl daemon-reload` | Reload systemd manager configuration |
| `df -h` | Display mounted filesystem usage |
| `findmnt` | Display mounted filesystem information |

**Important:** `systemctl daemon-reload` reloads systemd configuration. It does not itself mount EBS volumes. Use `mount -a` to test fstab entries.

---

## 2. Taking Backup of an EBS Volume (Snapshot)

### What is an EBS Snapshot?

An EBS Snapshot is a point-in-time backup of an EBS volume.

- Snapshots are stored in AWS-managed storage (Amazon S3 infrastructure).
- They can be used to restore an EBS volume.
- They are incremental, meaning subsequent snapshots generally store only changed blocks.
- Snapshots can be used to create new EBS volumes.
- Snapshots are useful for backup, disaster recovery and data migration.

### Steps to Create an EBS Snapshot

1. Open the AWS EC2 Dashboard.
2. Navigate to Elastic Block Store → Volumes.
3. Select the EBS volume.
4. Click **Actions → Create Snapshot**.
5. Enter a description and optional tags.
6. Click Create Snapshot.
7. Navigate to Elastic Block Store → Snapshots.
8. Wait until the snapshot status becomes `Completed`.

### Important Snapshot Terms

| Term | Meaning |
|---|---|
| Snapshot | Point-in-time backup of an EBS volume |
| Incremental Snapshot | Stores changes since the previous snapshot |
| Snapshot ID | Unique identifier of a snapshot |
| Volume Restoration | Creating a new EBS volume from a snapshot |
| Snapshot Retention | How long snapshots are kept before deletion |

### Restoring an EBS Volume from a Snapshot

1. Open EC2 Dashboard.
2. Navigate to Elastic Block Store → Snapshots.
3. Select the required snapshot.
4. Click Actions → Create Volume from Snapshot.
5. Select the required volume type and size.
6. Select the Availability Zone.
7. Create the volume.
8. Attach the newly created volume to an EC2 instance.

**Note:** The restored volume must be created in the same Availability Zone as the EC2 instance to which you want to attach it.

---

## 3. Automating Snapshots Using Lifecycle Manager (DLM)

Amazon Data Lifecycle Manager (DLM) automates the creation, retention and deletion of EBS snapshots.

Instead of manually creating snapshots every day, we can define a policy to automate the process.

### Steps to Create a Lifecycle Policy

1. Open the AWS EC2 Dashboard.
2. Navigate to Elastic Block Store → Lifecycle Manager.
3. Click Create lifecycle policy.
4. Select the policy type for EBS snapshots.
5. Choose the appropriate target resources using tags or other supported targeting options.
6. Select the schedule.
7. Configure the snapshot interval.
8. Set the retention policy.
9. Review and create the policy.

### Important Configurations

**A. Schedule**

Defines how frequently snapshots should be created.

Examples:
- Every 12 hours.
- Daily.
- Weekly.
- Monthly.

**B. Retention Policy**

Defines how long snapshots should be retained.

Examples:
- Keep the last 7 snapshots.
- Keep snapshots for 30 days.
- Automatically delete older snapshots according to the retention policy.

**C. Target Resources**

Defines which EBS volumes should be included in the lifecycle policy.

You can use resource tags to identify the volumes.

Example:

```text
Key: Backup
Value: Daily
```

A lifecycle policy targeting this tag can automatically back up matching EBS volumes.

### Example: Daily Snapshot Policy

| Configuration | Value |
|---|---|
| Policy Type | EBS Snapshot |
| Target Tag | Backup=Daily |
| Frequency | Daily |
| Retention | 7 days |
| Action | Automatically create and retain snapshots |

**Note:** DLM policies require appropriate IAM permissions and supported resource configurations. Snapshot deletion follows the configured retention rules.

---

## 4. Difference Between Manual and Automated Snapshots

| Manual Snapshot | Automated Snapshot (DLM) |
|---|---|
| Created manually | Created automatically |
| Requires user intervention | Follows a predefined schedule |
| User manages deletion | Retention policy manages deletion |
| Suitable for occasional backups | Suitable for regular backups |
| More manual effort | Less manual effort |

---

## 5. Important Commands – Quick Revision

| Command | Purpose |
|---|---|
| `sudo umount /mnt` | Unmounts the filesystem mounted at `/mnt` |
| `sudo mount /dev/nvme1n1p1 /mnt` | Manually mounts a partition |
| `sudo mount -a` | Mounts applicable filesystems from `/etc/fstab` |
| `sudo nano /etc/fstab` | Opens fstab for editing |
| `sudo blkid` | Displays UUID and filesystem information |
| `lsblk -f` | Displays block devices and filesystem details |
| `df -h` | Shows disk usage in human-readable format |
| `findmnt` | Displays mounted filesystems |
| `sudo systemctl daemon-reload` | Reloads systemd manager configuration |

---

## 6. Important Points to Remember

1. `/etc/fstab` is used for permanent filesystem mounting.
2. UUID is preferred over device names in fstab.
3. Always test fstab entries using `sudo mount -a` before rebooting.
4. `umount` is used to detach a mounted filesystem from its mount point.
5. EBS Snapshots are point-in-time backups.
6. EBS Snapshots are incremental.
7. Amazon Data Lifecycle Manager (DLM) automates snapshot creation and retention.
8. Snapshots can be used to restore EBS volumes.
9. An EBS volume and its snapshot are separate resources.
10. Deleting an EC2 instance does not necessarily delete its EBS snapshots.

---

**Dated:** 26 November 2026
