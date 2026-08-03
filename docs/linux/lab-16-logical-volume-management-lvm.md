# 🧪 LVM (Logical Volume Management)

## 🎯 Objective

The goal of this lab is to learn how to manage Linux storage using **LVM (Logical Volume Management)**.

LVM provides a flexible storage management layer allowing administrators to:

- Create flexible storage pools
- Separate physical disks from logical volumes
- Resize storage without data loss
- Extend filesystems while they are mounted

---

# 💻 Environment

- OS: Ubuntu 26.04
- Virtualization: VirtualBox
- Additional disk: 10 GB VDI
- Storage technology: LVM2
- Filesystem: ext4

---

# 🔎 Disk Identification

A new virtual disk was added to the virtual machine.

Initial disk verification:

```bash
lsblk
```

Detected disks:

```
sda   40G   System disk
sdb    2G   Existing data disk
sdc   10G   New LVM test disk
```

The new disk used for this lab:

```
/dev/sdc
```

---

# 1️⃣ Creating the LVM Partition

A new partition was created using `fdisk`.

Command:

```bash
sudo fdisk /dev/sdc
```

Actions performed:

- Created a primary partition
- Used the entire disk capacity
- Changed partition type to Linux LVM

Partition created:

```
/dev/sdc1
```

Verification:

```bash
lsblk
```

Result:

```
sdc
└── sdc1   10G
```

---

# 2️⃣ Installing LVM Tools

The LVM utilities were not installed by default.

Installation:

```bash
sudo apt update
sudo apt install lvm2
```

Verification:

```bash
which pvcreate
```

Output:

```
/usr/sbin/pvcreate
```

---

# 3️⃣ Creating the Physical Volume (PV)

The partition was initialized as an LVM Physical Volume.

Command:

```bash
sudo pvcreate /dev/sdc1
```

Verification:

```bash
sudo pvs
```

Result:

```
PV         VG Fmt  Attr PSize
/dev/sdc1     lvm2      <10G
```

The disk is now available for LVM management.

---

# 4️⃣ Creating the Volume Group (VG)

A Volume Group was created to pool the storage capacity.

Command:

```bash
sudo vgcreate vg_lab16 /dev/sdc1
```

Verification:

```bash
sudo vgs
```

Result:

```
VG        #PV  VSize
vg_lab16    1  <10G
```

Created Volume Group:

```
vg_lab16
```

---

# 5️⃣ Creating the Logical Volume (LV)

A Logical Volume of 5 GB was created.

Command:

```bash
sudo lvcreate -L 5G -n lv_data vg_lab16
```

Verification:

```bash
sudo lvs
```

Result:

```
LV       VG        LSize
lv_data  vg_lab16  5.00G
```

---

# 6️⃣ Creating the Filesystem

The Logical Volume was formatted with ext4.

Command:

```bash
sudo mkfs.ext4 /dev/vg_lab16/lv_data
```

Verification:

```bash
lsblk -f
```

Result:

```
vg_lab16-lv_data ext4
```

---

# 7️⃣ Mounting the Logical Volume

A mount point was created:

```bash
sudo mkdir /data
```

The filesystem was mounted:

```bash
sudo mount /dev/vg_lab16/lv_data /data
```

Verification:

```bash
df -h /data
```

Result:

```
/dev/mapper/vg_lab16-lv_data   4.9G   /data
```

The logical volume is now accessible through:

```
/data
```

---

# 8️⃣ Persistent Mount Configuration

The filesystem UUID was retrieved:

```bash
sudo blkid /dev/vg_lab16/lv_data
```

UUID:

```
4ee169a4-cee4-4967-9163-15f8faf11f32
```

Added to `/etc/fstab`:

```
UUID=4ee169a4-cee4-4967-9163-15f8faf11f32 /data ext4 defaults 0 2
```

Configuration test:

```bash
sudo mount -a
```

No errors were returned.

---

# 9️⃣ Extending the Logical Volume

The Volume Group still had free space available.

Verification:

```bash
sudo vgs
```

Before extension:

```
VG        VSize   VFree
vg_lab16  <10G    <5G
```

The Logical Volume was extended:

```bash
sudo lvextend -L 8G /dev/vg_lab16/lv_data
```

Result:

```
Logical volume successfully resized.
```

---

# 🔟 Resizing the Filesystem

The ext4 filesystem was extended to use the new space.

Command:

```bash
sudo resize2fs /dev/vg_lab16/lv_data
```

The resize was performed online while mounted.

Verification:

```bash
df -h /data
```

Result:

```
/dev/mapper/vg_lab16-lv_data   7.8G   /data
```

---

# ✅ Final LVM Architecture

```
Virtual Disk
/dev/sdc (10G)
        |
        |
Partition
/dev/sdc1
        |
        |
Physical Volume
        |
        |
Volume Group
vg_lab16 (<10G)
        |
        |
Logical Volume
lv_data (8G)
        |
        |
Filesystem
ext4
        |
        |
Mount Point
/data
```

---

# 🧠 Skills Learned

During this lab, the following Linux administration skills were practiced:

- Adding and preparing storage devices
- Creating LVM partitions
- Managing Physical Volumes (PV)
- Creating Volume Groups (VG)
- Creating Logical Volumes (LV)
- Formatting filesystems
- Persistent filesystem mounting
- Extending storage dynamically
- Online filesystem resizing

---

# 🏁 Conclusion

This lab introduced **Logical Volume Management**, a key technology used in Linux server environments.

Unlike traditional partitions, LVM provides flexibility by allowing administrators to increase storage capacity without recreating partitions or interrupting services.

The final configuration successfully demonstrated a complete storage lifecycle:

**Disk → Partition → PV → VG → LV → Filesystem → Mount → Resize**
