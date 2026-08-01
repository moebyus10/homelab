# 💾 Lab 15 — Backup & Restore

## 🎯 Objective

The objective of this lab is to understand Linux backup and restore strategies.

During this lab, I learned how to:

- Create compressed backups using `tar`
- Verify archive contents
- Restore deleted data from a backup
- Synchronize files using `rsync`
- Verify backup integrity

Backup and restore operations are essential skills for system administrators to protect data and ensure service continuity.

---

# 🖥️ Environment

- **Operating System:** Ubuntu Server
- **Hostname:** Vega
- **User:** moebyus
- **Shell:** Bash

---

# 📁 Preparing Test Data

A dedicated directory was created for the backup tests:

    mkdir ~/backup-lab

Test files were created:

    mkdir ~/backup-lab/data

    echo "Important system data" > ~/backup-lab/data/file1.txt
    echo "Configuration backup test" > ~/backup-lab/data/file2.txt

Verification:

    ls -R ~/backup-lab

The test environment was successfully created.

---

# 📦 Creating a Backup with tar

A compressed archive was created using `tar`:

    tar -czvf backup-test.tar.gz ~/backup-lab/data

Options used:

- `c` → Create archive
- `z` → Compress using gzip
- `v` → Display processed files
- `f` → Specify archive filename

The archive was created successfully.

Verification:

    ls -lh backup-test.tar.gz

---

# 🔎 Checking Archive Contents

The content of the backup archive was inspected:

    tar -tzvf backup-test.tar.gz

Output confirmed that the backup contained:

    home/moebyus/backup-lab/data/file1.txt
    home/moebyus/backup-lab/data/file2.txt

The backup archive was valid.

---

# 💥 Data Loss Simulation

To simulate accidental data deletion, the original files were removed:

    rm -rf ~/backup-lab/data

Verification:

    ls ~/backup-lab

The original data was no longer available.

This simulated a real data loss scenario.

---

# ♻️ Restoring Data from Backup

A restoration directory was created:

    mkdir ~/restore-test

The backup archive was extracted:

    tar -xzvf backup-test.tar.gz -C ~/restore-test

Verification:

    ls -R ~/restore-test

The original files were successfully restored:

    file1.txt
    file2.txt

The backup restoration process was successful.

---

# 🔄 Synchronization with rsync

A second backup location was created:

    mkdir ~/backup-rsync

The restored data was synchronized:

    rsync -av ~/restore-test/ ~/backup-rsync/

Options:

- `a` → Archive mode
- `v` → Verbose output

Verification:

    ls -R ~/backup-rsync

The files were successfully copied to the backup destination.

---

# ✅ Backup Integrity Verification

The two directories were compared:

    diff -r ~/restore-test ~/backup-rsync

No output was returned.

This confirmed that:

- Files were identical
- No data was lost
- The synchronization was successful

---

# 🧹 Cleanup

Temporary files created during the lab were removed:

    rm -rf ~/backup-lab

    rm -rf ~/restore-test

    rm -rf ~/backup-rsync

    rm backup-test.tar.gz

The system was returned to its initial state.

---

# 📚 Skills Learned

- Linux backup management
- Archive creation with tar
- Data restoration procedures
- File synchronization with rsync
- Backup integrity checking
- Disaster recovery fundamentals

---

# 🏁 Conclusion

This lab demonstrated the importance of reliable backup and restore procedures in Linux administration.

I learned how to create backups, simulate data loss, restore files, and verify that recovered data was identical to the original source.

These techniques are essential for system administrators to protect critical information, recover from failures, and maintain service availability.

The combination of backup tools such as `tar`, `rsync`, and automation tools like `cron` provides the foundation for building reliable backup strategies in production environments.
