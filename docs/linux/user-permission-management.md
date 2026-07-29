# Linux User and Permission Management

## 📌 Objective

Learn how to manage Linux users, groups and permissions on Ubuntu Server.

---

## Environment

- Windows 11
- Oracle VirtualBox 7.2.12
- Ubuntu Server 24.04 LTS

---

## Tasks

- Created a dedicated administration group
- Created a new Linux user
- Assigned the user to the administration group
- Configured ownership with `chown`
- Configured permissions with `chmod`
- Applied the SGID bit to a shared directory
- Verified file inheritance

---

## Commands Used

```bash
groupadd
useradd
passwd
usermod
id
whoami
chown
chmod
ls -l
```

---

## Troubleshooting

When creating files inside the shared directory, new files initially inherited the user's primary group.

```
tachuser tachuser
```

To ensure all files inherit the shared administration group, the SGID bit was applied:

```bash
sudo chmod g+s /opt/sysadmins-lab
```

New files now inherit:

```
tachuser sysadmins
```

---

## Skills Demonstrated

- Linux user administration
- Linux group management
- File ownership
- File permissions
- SGID
- Principle of Least Privilege
- Linux CLI
