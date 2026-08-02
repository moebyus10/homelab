# 🧪 NFS File Sharing

## 🎯 Objective

The goal of this lab is to learn how to configure and use **NFS (Network File System)** to share directories across a network.

NFS is widely used in Linux environments to provide centralized storage accessible from multiple clients while maintaining a native filesystem experience.

In this lab, a single Ubuntu virtual machine acts as both the **NFS server** and the **NFS client** using the loopback interface (`127.0.0.1`).

---

# 💻 Environment

- OS: Ubuntu 26.04 LTS
- Virtualization: VirtualBox
- VM Disk: `ubuntu-26.04-Lab_2.vdi`
- Hostname: `Vega`
- Server: NFS Server
- Client: Same machine (localhost)
- Protocol: NFSv4.2

---

# 🏗️ Lab Architecture

```
                Ubuntu Vega

          +-----------------------+
          |                       |
          |   NFS Server          |
          |                       |
          |  /srv/nfs/share       |
          |         ▲             |
          |         │             |
          |      NFS v4.2         |
          |         │             |
          |         ▼             |
          |  /mnt/nfs-share       |
          |   NFS Client          |
          |                       |
          +-----------------------+
```

---

# 1️⃣ Installing the NFS Server

Package index update:

```bash
sudo apt update
```

NFS server installation:

```bash
sudo apt install nfs-kernel-server
```

Service verification:

```bash
sudo systemctl status nfs-server
```

Result:

```
Active: active (exited)
```

The NFS server is installed and enabled.

---

# 2️⃣ Creating the Shared Directory

A dedicated directory was created for sharing.

```bash
sudo mkdir -p /srv/nfs/share
```

Permissions configured for the lab:

```bash
sudo chown nobody:nogroup /srv/nfs/share
sudo chmod 777 /srv/nfs/share
```

Verification:

```bash
ls -ld /srv/nfs/share
```

Result:

```
drwxrwxrwx 2 nobody nogroup ... /srv/nfs/share
```

---

# 3️⃣ Configuring NFS Exports

The export configuration was added to:

```
/etc/exports
```

Configuration:

```
/srv/nfs/share 127.0.0.1(rw,sync,no_subtree_check)
```

Export table reloaded:

```bash
sudo exportfs -ra
```

Verification:

```bash
sudo exportfs -v
```

Result:

```
/srv/nfs/share
127.0.0.1(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)
```

The directory is now exported through NFS.

---

# 4️⃣ Creating the Client Mount Point

Mount point creation:

```bash
sudo mkdir -p /mnt/nfs-share
```

---

# 5️⃣ Mounting the NFS Share

The exported directory was mounted locally through the loopback interface.

```bash
sudo mount -t nfs 127.0.0.1:/srv/nfs/share /mnt/nfs-share
```

Verification:

```bash
df -h /mnt/nfs-share
```

Result:

```
127.0.0.1:/srv/nfs/share
```

Mounted filesystem verification:

```bash
mount | grep nfs
```

Result:

```
127.0.0.1:/srv/nfs/share on /mnt/nfs-share type nfs4
```

The NFS share is successfully mounted.

---

# 6️⃣ Testing Read and Write Operations

A test file was created through the mounted NFS share.

```bash
echo "NFS is working!" | sudo tee /mnt/nfs-share/test.txt
```

Verification from the client:

```bash
ls -l /mnt/nfs-share
```

Result:

```
test.txt
```

Verification from the server:

```bash
ls -l /srv/nfs/share
```

Result:

```
test.txt
```

The file immediately appeared on both sides, confirming that the NFS share is functioning correctly.

---

# 7️⃣ Configuring Automatic Mounting

A backup of the filesystem table was created.

```bash
sudo cp /etc/fstab /etc/fstab.bak
```

The following line was added to `/etc/fstab`:

```
127.0.0.1:/srv/nfs/share   /mnt/nfs-share   nfs   defaults,_netdev   0   0
```

The share was unmounted:

```bash
sudo umount /mnt/nfs-share
```

Mount verification:

```bash
mount | grep nfs
```

Only the NFS server remained active.

The configuration was tested:

```bash
sudo mount -a
```

Verification:

```bash
df -h /mnt/nfs-share
```

Result:

```
127.0.0.1:/srv/nfs/share
```

Final verification:

```bash
ls -l /mnt/nfs-share
```

Result:

```
test.txt
```

The share is automatically mounted through `/etc/fstab`.

---

# 🔍 Useful Commands

Display exported shares:

```bash
sudo exportfs -v
```

Display mounted NFS filesystems:

```bash
mount | grep nfs
```

Display disk usage:

```bash
df -h
```

Display NFS server status:

```bash
sudo systemctl status nfs-server
```

Reload exports:

```bash
sudo exportfs -ra
```

---

# 📁 Final Architecture

```
Virtual Machine (Vega)
│
├── NFS Server
│      │
│      └── /srv/nfs/share
│
└── NFS Client
       │
       └── /mnt/nfs-share
              │
              ▼
      Mounted through NFSv4.2
```

---

# 🧠 Skills Learned

During this lab, the following Linux administration skills were practiced:

- Installing an NFS server
- Creating and exporting shared directories
- Configuring `/etc/exports`
- Managing NFS permissions
- Mounting NFS shares
- Verifying NFS mounts
- Testing read and write operations
- Configuring persistent mounts with `/etc/fstab`
- Understanding client/server storage architecture

---

# 🏁 Conclusion

This lab introduced **Network File System (NFS)**, one of the most widely used file-sharing protocols in Linux environments.

A complete NFS workflow was implemented, including server installation, shared directory configuration, client mounting, access verification, and persistent mounting.

Although the server and client were hosted on the same virtual machine for learning purposes, the exact same configuration can be applied between multiple Linux servers on a network.

This lab provides a solid foundation for centralized storage solutions commonly found in enterprise infrastructures.
