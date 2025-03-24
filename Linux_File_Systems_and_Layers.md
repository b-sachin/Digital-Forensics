## **1. Introduction to Linux File Systems**
### **What is a File System?**
- A file system is a method for **organizing, storing, and retrieving data** on a storage device.
- Linux supports multiple file systems, each optimized for different use cases.

### **Types of Linux File Systems**
| File System | Features |
|------------|----------|
| **Ext2** | No journaling, good for USB drives |
| **Ext3** | Introduced journaling, stable and reliable |
| **Ext4** | Faster, supports large files and partitions |
| **XFS** | High-performance journaling file system |
| **Btrfs** | Supports snapshots, checksums, and RAID |
| **ReiserFS** | Fast metadata handling |

### **Checking File System Type**
```bash
lsblk -f  # List file systems of all mounted devices
blkid     # Display file system type for a device
```

## **2. File System Layers**
A Linux file system is structured into multiple layers:

### **1. File System Layer**
- Manages the **overall structure** of files and directories.
- Includes **file allocation tables, journaling, and data recovery mechanisms**.

### **2. File Name Layer**
- Handles **file naming conventions** and directory structures.
- Ensures **unique file names within a directory**.

### **3. Metadata Layer**
- Stores **file attributes** (permissions, timestamps, owner, group).
- Managed using **inodes**.

### **4. Data Unit Layer**
- Handles actual **data storage in blocks and clusters**.
- Optimizes read/write operations for performance.

### **Viewing File System Information**
```bash
df -Th    # Show mounted file systems with type
stat file.txt  # Show metadata of a file
```

## **3. Journaling and File System Recovery**
### **What is Journaling?**
- A journal is a **log of file system changes** recorded before applying them.
- Helps in **fast recovery after a crash**.

### **Types of Journaling**
| Type | Description |
|------|------------|
| **Writeback** | Only logs metadata, fastest but least reliable |
| **Ordered** | Logs metadata first, then data (default in Ext4) |
| **Full Journaling** | Logs both metadata and data, slowest but most reliable |

### **Journal Tools**
```bash
fsck /dev/sda1  # Check and repair file system errors
tune2fs -O has_journal /dev/sda1  # Enable journaling on Ext2
journalctl -xe  # View journal logs (systemd)
```

## **4. Deleted Data and File Recovery**
### **How Linux Handles Deleted Files**
- When a file is deleted, only its **inode reference** is removed.
- The **actual data remains on disk** until overwritten.

### **Recovery Tools**
| Tool | Description |
|------|------------|
| **extundelete** | Recovers files from ext3/ext4 file systems |
| **testdisk** | Restores lost partitions and files |
| **photorec** | Recovers deleted multimedia files |

### **Recovering a Deleted File**
```bash
extundelete /dev/sda1 --restore-file /home/user/deleted.txt
```

## **5. Hands-on Practice Commands**
### **Check File System Type**
```bash
lsblk -f
```
### **List Mounted File Systems**
```bash
mount
```
### **Check File System Health**
```bash
fsck /dev/sda1
```
### **Recover a Deleted File**
```bash
extundelete /dev/sda1 --restore-file /home/user/file.txt
```

## **6. Summary**
- Linux supports various **file systems** like **Ext4, XFS, Btrfs**.  
- The file system consists of **multiple layers** (File Name, Metadata, Data Unit).  
- **Journaling** prevents data corruption and speeds up recovery.  
- **Deleted files** can be recovered using forensic tools before they are overwritten.  
