
# Linux Storage Management

## Objective

Learn how to manage storage devices under Linux by:

- detecting new disks
- creating partitions
- formatting a filesystem
- mounting a filesystem
- configuring persistent mounts with `/etc/fstab`
- verifying storage after reboot

---

# Environment

- Host: Windows 11
- Hypervisor: VirtualBox 7.2
- Guest OS: Ubuntu 26.04 LTS
- User: moebyus

---

# Initial State

System disk:

```
/dev/sda
```

A new virtual disk of **2 GB** was added from VirtualBox.

---

# Detect the New Disk

```bash
lsblk
```

Output:

```
sdb      2G
```

More information:

```bash
sudo fdisk -l /dev/sdb
```

Filesystem information:

```bash
lsblk -f
```

---

# Create a GPT Partition Table

Launch fdisk:

```bash
sudo fdisk /dev/sdb
```

Commands used:

```
g
n
w
```

Verification:

```bash
lsblk
```

Result:

```
sdb
└── sdb1
```

---

# Create an ext4 Filesystem

```bash
sudo mkfs.ext4 /dev/sdb1
```

Verify:

```bash
lsblk -f
```

Filesystem:

```
ext4
```

---

# Create a Mount Point

```bash
sudo mkdir -p /mnt/data
```

---

# Mount the Filesystem

```bash
sudo mount /dev/sdb1 /mnt/data
```

Verification:

```bash
findmnt /mnt/data
```

```
/mnt/data
```

Disk usage:

```bash
df -h /mnt/data
```

---

# Test Read and Write

Create a file:

```bash
echo "Homelab Storage Test" | sudo tee /mnt/data/test.txt
```

Create a directory:

```bash
sudo mkdir /mnt/data/backups
```

Create another file:

```bash
echo "Backup directory" | sudo tee /mnt/data/backups/info.txt
```

Display the directory tree:

```bash
tree /mnt/data
```

Example:

```
/mnt/data
├── backups
│   └── info.txt
├── lost+found
└── test.txt
```

---

# Configure Automatic Mount

Retrieve the UUID:

```bash
sudo blkid
```

Example:

```
UUID=4a3407b0-b62e-4b44-a0ce-95c443a4d9c5
```

Edit:

```bash
sudo nano /etc/fstab
```

Add:

```text
UUID=4a3407b0-b62e-4b44-a0ce-95c443a4d9c5 /mnt/data ext4 defaults 0 2
```

Reload configuration:

```bash
sudo systemctl daemon-reload
```

Test configuration:

```bash
sudo umount /mnt/data
sudo mount -a
```

Verify:

```bash
findmnt /mnt/data
```

---

# Verify After Reboot

Restart the system:

```bash
sudo reboot
```

Reconnect through SSH.

Verify:

```bash
findmnt /mnt/data
```

Result:

```
TARGET    SOURCE
/mnt/data /dev/sdb1
```

The filesystem is automatically mounted at boot.

---

# Commands Learned

```bash
lsblk
lsblk -f
fdisk
mkfs.ext4
mount
umount
findmnt
df -h
blkid
tree
nano
systemctl daemon-reload
mount -a
```

---

# Skills Acquired

- Detecting storage devices
- GPT partitioning
- Creating Linux filesystems
- Mounting filesystems
- Managing mount points
- Using UUIDs
- Configuring `/etc/fstab`
- Persistent storage management
- Validating storage after reboot

---

# Result

✔ Added a new virtual disk

✔ Partitioned the disk

✔ Formatted it with ext4

✔ Mounted it manually

✔ Tested file operations

✔ Configured persistent mounting

✔ Verified automatic mounting after reboot

This lab demonstrates the complete lifecycle of provisioning and mounting additional storage on a Linux system.
