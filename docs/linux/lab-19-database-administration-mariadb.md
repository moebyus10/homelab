# 🗄️ Database Administration with MariaDB

## 🎯 Objective

The goal of this lab is to understand database administration on Linux using MariaDB.

During this lab, the following concepts were covered:

* Installing and managing a database server
* Managing services with systemd
* Creating databases and users
* Managing SQL privileges
* Creating and manipulating tables
* Performing database backups
* Restoring databases

---

# 📦 Installing MariaDB

MariaDB server was installed:

```bash
sudo apt install mariadb-server
```

The service status was checked:

```bash
sudo systemctl status mariadb
```

Result:

```
Active: active (running)
```

MariaDB is running correctly.

---

# 🔎 Checking MariaDB Version

A root connection was tested:

```bash
sudo mariadb
```

Version verification:

```sql
SELECT VERSION();
```

Result:

```
11.8.6-MariaDB-5ubuntu0.1 from Ubuntu
```

---

# 🔌 Checking Database Port

Listening ports were checked:

```bash
sudo ss -tulnp | grep mariadb
```

Result:

```
tcp LISTEN 127.0.0.1:3306
```

MariaDB is only accessible locally, improving security.

---

# 🏗️ Creating Database

A dedicated database was created:

```sql
CREATE DATABASE homelab_db;
```

Verification:

```sql
SHOW DATABASES;
```

Result:

```
homelab_db
```

---

# 👤 Creating Database User

A dedicated user was created:

```sql
CREATE USER 'homelab_user'@'localhost'
IDENTIFIED BY 'LinuxLab2026!';
```

Privileges were granted:

```sql
GRANT ALL PRIVILEGES
ON homelab_db.*
TO 'homelab_user'@'localhost';

FLUSH PRIVILEGES;
```

Permissions verification:

```sql
SHOW GRANTS FOR 'homelab_user'@'localhost';
```

The user has full privileges on `homelab_db`.

---

# 🔑 Testing User Access

Connection test:

```bash
mariadb -u homelab_user -p
```

Available databases:

```sql
SHOW DATABASES;
```

Result:

```
homelab_db
information_schema
```

The user can only access authorized databases.

---

# 🗃️ Creating Database Structure

Database selection:

```sql
USE homelab_db;
```

A server inventory table was created:

```sql
CREATE TABLE servers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    hostname VARCHAR(50),
    ip_address VARCHAR(15),
    status VARCHAR(20)
);
```

---

# ➕ Adding Data

Sample data was inserted:

```sql
INSERT INTO servers
(hostname, ip_address, status)
VALUES
('Vega', '127.0.0.1', 'online'),
('Ubuntu-Web', '192.168.1.10', 'online'),
('Backup-Server', '192.168.1.20', 'offline');
```

Data verification:

```sql
SELECT * FROM servers;
```

Result:

```
Vega          127.0.0.1      online
Ubuntu-Web    192.168.1.10   online
Backup-Server 192.168.1.20   offline
```

---

# 💾 Database Backup

Backup directory creation:

```bash
mkdir ~/mariadb-backups
```

Database export:

```bash
mysqldump -u homelab_user -p homelab_db \
> ~/mariadb-backups/homelab_db_backup.sql
```

Backup verification:

```bash
ls -lh ~/mariadb-backups/
```

Backup file created successfully:

```
homelab_db_backup.sql
```

---

# ♻️ Database Restore Test

A restore database was created:

```sql
CREATE DATABASE homelab_restore;
```

The first restore attempt failed because the user did not have privileges on the new database.

After granting permissions:

```sql
GRANT ALL PRIVILEGES
ON homelab_restore.*
TO 'homelab_user'@'localhost';

FLUSH PRIVILEGES;
```

The restore was successful:

```bash
mariadb -u homelab_user -p homelab_restore \
< ~/mariadb-backups/homelab_db_backup.sql
```

Verification:

```sql
USE homelab_restore;

SHOW TABLES;

SELECT * FROM servers;
```

The table and data were successfully restored.

---

# ✅ Lab Validation

The following objectives were successfully completed:

✔ MariaDB installed and running
✔ Database service managed with systemd
✔ Database created
✔ SQL user created
✔ Privileges configured
✔ Tables created and populated
✔ Backup created with mysqldump
✔ Database restored successfully

---

# 🧠 Skills Practiced

* Linux database administration
* MariaDB management
* SQL user permissions
* Database creation
* Backup and restore procedures
* Data recovery testing

---

# 🏁 Conclusion

This lab demonstrated the fundamentals of database administration on Linux.

MariaDB was installed, configured and tested.
A controlled database environment was created with dedicated users and permissions.

Backup and restore procedures were validated to ensure database recovery capability.

The next step will focus on advanced Linux administration and automation.
