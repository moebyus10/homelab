# Linux Storage Management

## Objective

The objective of this lab is to learn how to add and manage additional storage on a Linux system.

During this lab, an additional virtual disk was added to the Ubuntu virtual machine. The disk was partitioned, formatted with the ext4 filesystem, mounted manually, and configured for persistent mounting after reboot using `/etc/fstab`.

---

## Environment

* Host OS: Windows 11
* Hypervisor: VirtualBox 7.2.12
* Guest OS: Ubuntu 26.04 LTS
* Virtual Machine: Vega
* User: moebyus

---

## Skills Practiced

* Identifying storage devices with Linux tools
* Managing disks with `lsblk` and `fdisk`
* Creating GPT partition tables
* Creating Linux partitions
* Formatting partitions with ext4
* Mounting filesystems
* Testing storage access
* Configuring persistent mounts with `/etc/fstab`
* Verifying storage after reboot

---

# Step 1 - Identify Available Disks

First, the available block devices were listed.

Command:

```bash
lsblk
```

Initial storage configuration:

```text
sda
├─sda1
└─sda2   /
```

A new virtual disk was detected:

```text
sdb      2G
```

This disk was the additional storage device used during this lab.

---

# Step 2 - Inspect the New Disk

Disk information was checked using:

```bash
sudo fdisk -l /dev/sdb
```

Result:

```text
Disk /dev/sdb: 1.95 GiB
Disk model: VBOX HARDDISK
```

The disk did not contain a filesystem yet.

Verification:

```bash
lsblk -f /dev/sdb
```

Output:

```text
NAME   FSTYPE FSVER LABEL UUID
sdb
```

---

# Step 3 - Create a GPT Partition

The disk was partitioned using `fdisk`.

Command:

```bash
sudo fdisk /dev/sdb
```

Commands used:

```text
g       Create a new GPT partition table
n       Create a new partition
Enter   Default partition number
Enter   Default first sector
Enter   Use all available space
w       Write changes
```

Partition verification:

```bash
sudo fdisk -l /dev/sdb
```

Result:

```text
Device      Start      End  Sectors  Size Type
/dev/sdb1   2048 4093951 4091904    2G  Linux filesystem
```

---

# Step 4 - Format the Partition

The new partition was formatted using the ext4 filesystem.

Command:

```bash
sudo mkfs.ext4 /dev/sdb1
```

Filesystem verification:

```bash
lsblk -f
```

Result:

```text
sdb
└─sdb1 ext4
```

UUID obtained:

```text
4a3407b0-b62e-4b44-a0ce-95c443a4d9c5
```

---

# Step 5 - Create Mount Point

A mount directory was created:

```bash
sudo mkdir /mnt/data
```

The partition was mounted:

```bash
sudo mount /dev/sdb1 /mnt/data
```

Verification:

```bash
findmnt /mnt/data
```

Output:

```text
TARGET     SOURCE      FSTYPE
/mnt/data  /dev/sdb1   ext4
```

Disk usage check:

```bash
df -h /mnt/data
```

Result:

```text
/dev/sdb1  1.9G  520K  1.8G  1% /mnt/data
```

---

# Step 6 - Storage Test

A test file was created on the new storage.

Command:

```bash
echo "Homelab Storage Test" | sudo tee /mnt/data/test.txt
```

Verification:

```bash
cat /mnt/data/test.txt
```

Output:

```text
Homelab Storage Test
```

A backup directory was created:

```bash
sudo mkdir /mnt/data/backups
```

Test file:

```bash
echo "Backup directory" | sudo tee /mnt/data/backups/info.txt
```

Directory structure:

```bash
tree /mnt/data
```

Output:

```text
/mnt/data
├── backups
│   └── info.txt
├── lost+found
└── test.txt
```

---

# Step 7 - Configure Persistent Mounting

The filesystem UUID was retrieved:

```bash
sudo blkid
```

The `/etc/fstab` file was edited:

```bash
sudo nano /etc/fstab
```

The following entry was added:

```text
UUID=4a3407b0-b62e-4b44-a0ce-95c443a4d9c5 /mnt/data ext4 defaults 0 2
```

Systemd configuration was reloaded:

```bash
sudo systemctl daemon-reload
```

The filesystem configuration was tested:

```bash
sudo mount -a
```

Verification:

```bash
findmnt /mnt/data
```

Result:

```text
TARGET     SOURCE
/mnt/data  /dev/sdb1
```

---

# Step 8 - Verify Persistence After Reboot

The virtual machine was rebooted:

```bash
sudo reboot
```

SSH reconnection from Windows:

```powershell
ssh moebyus@localhost -p 2222
```

After reconnecting, the mount was verified:

```bash
findmnt /mnt/data
```

Result:

```text
TARGET     SOURCE
/mnt/data  /dev/sdb1
```

The storage remained mounted automatically after reboot.

---

# Storage Configuration Summary

Current storage layout:

```text
NAME   SIZE  TYPE  MOUNTPOINT

sda    40G   disk
└─sda2 40G   ext4  /

sdb     2G   disk
└─sdb1  2G   ext4  /mnt/data
```

---

# What I Learned

During this lab, I learned how to:

* identify disks and partitions on Linux
* create and manage GPT partitions
* format storage devices with ext4
* mount filesystems manually
* use UUIDs for reliable device identification
* configure persistent mounts using `/etc/fstab`
* verify storage availability after reboot

---

# Conclusion

This lab demonstrated the complete process of adding additional storage to a Linux system.

The virtual disk was successfully detected, partitioned, formatted, mounted, tested, and configured for automatic mounting at system startup.

These skills are essential for Linux system administration, server management, and infrastructure maintenance.
