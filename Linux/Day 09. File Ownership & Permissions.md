# 🐧 Day 09 – File Ownership & Permissions

## Table of Contents
- [1. Quick Syntax & Real-World Examples](#1-quick-syntax--real-world-examples)
- [2. `ls -l` Output Fields](#2-ls--l-output-fields)
- [3. Ownership & Groups](#3-ownership--groups)
- [4. Standard Permissions](#4-standard-permissions)
- [5. Special Permissions (SUID/SGID/Sticky)](#5-special-permissions-suidsgidsticky)
- [6. Access Control Lists (ACLs)](#6-access-control-lists-acls)
- [7. `umask` (User File-Creation Mask)](#7-umask-user-file-creation-mask)

---

<details>
<summary><strong>1. Quick Syntax & Real-World Examples</strong></summary>

Every file has three permission sets: user (u), group (g), others (o).  
Permissions: read (r), write (w), execute (x).  

```text
# rw-r--r-- on pets.txt (owner: akhil-teja-doosari, group: bestfriends)
# user (u): rw-  (read/write)
# group (g): r-- (read)
# others (o): r-- (read)
```

**Chmod Examples**
```bash
chmod u+x pets.txt                         # akhil-teja-doosari can execute pets.txt
chmod g+w samplelog.txt                    # bestfriends group can write samplelog.txt
chmod o-x employees.txt                    # remove execute from others on employees.txt
chmod u-x employees.txt                    # remove execute from owner on employees.txt
```

**Numeric Permissions**  
Permissions map to bits: execute = 2⁰ (1), write = 2¹ (2), read = 2² (4).

**Bit Values Quick Reference**
| Permission | Exponent | Value |
|:----------:|:--------:|:-----:|
| x          | 2⁰       | 1     |
| w          | 2¹       | 2     |
| r          | 2²       | 4     |

| Octal | Symbolic | Calculation      | Meaning               |
|:-----:|:--------:|------------------|-----------------------|
| 0     | ---      | 0                | none                  |
| 1     | --x      | 2⁰ = 1           | execute only          |
| 2     | -w-      | 2¹ = 2           | write only            |
| 3     | -wx      | 2¹+2⁰ = 3        | write+execute         |
| 4     | r--      | 2² = 4           | read only             |
| 5     | r-x      | 2²+2⁰ = 5        | read+execute          |
| 6     | rw-      | 2²+2¹ = 6        | read+write            |
| 7     | rwx      | 2²+2¹+2⁰ =7      | read+write+execute    |
```
chmod 400 employees.txt                    # r--------
chmod 666 samplelog.txt                    # rw-rw-rw-
chmod 444 samplelog.txt                    # r--r--r--
chmod 777 pets.txt                         # rwxrwxrwx
```

**Defaults**
- Default directory: 755 (`rwxr-xr-x`)
- Default file: 644 (`rw-r--r--`)
- Highest: 777 (`rwxrwxrwx`)

</details>

---

<details>
<summary><strong>2. `ls -l` Output Fields</strong></summary>

- Running `ls -l` displays seven columns:

| Col | Field                              | Description                                      |
|:---:|------------------------------------|--------------------------------------------------|
| 1   | File type & permissions (10 chars) | Type (`d`/`-`/`l`) + permissions                  |
| 2   | Hard-link count                    | Number of directory entries to the inode         |
| 3   | Owner (user)                       | e.g. `akhil-teja-doosari`                        |
| 4   | Group                              | e.g. `bestfriends`                               |
| 5   | Size                               | Bytes (use `-h` for human-readable)              |
| 6   | Timestamp                          | Last modification date & time                    |
| 7   | Name                               | Filename, e.g. `pets.txt`                        |

```bash
ls -lh /home/akhil-teja-doosari/shared
# -rw-r--r-- 1 akhil-teja-doosari bestfriends 1.2K Jun 25 22:00 pets.txt
```
</details>

---

<details>
<summary><strong>3. Ownership & Groups</strong></summary>

**Theory & Notes**
- **Owner (u):** primary user (e.g., `akhil-teja-doosari`).
- **Group (g):** shared team (e.g., `bestfriends`).
- **Others (o):** all other users (e.g., `navya`, `charan`, `pramod`).

**Commands & Examples**
| Action                        | Command                                                               |
|-------------------------------|-----------------------------------------------------------------------|
| Set owner & group             | `sudo chown akhil-teja-doosari:bestfriends pets.txt`                  |
| Change owner only             | `sudo chown navya samplelog.txt`                                      |
| Change group only (chown)     | `sudo chown :bestfriends samplelog.txt`                               |
| Change group (chgrp)          | `sudo chgrp bestfriends employees.txt`                                |

</details>

---

<details>
<summary><strong>4. Standard Permissions</strong></summary>

Permissions = three triads for u, g, o:
```
 USER    GROUP    OTHERS
 r w x   r w x   r w x
```
- `r`=4, `w`=2, `x`=1

**Symbolic Method**
```bash
chmod u+x pets.txt           # akhil-teja-doosari adds execute
chmod g-r samplelog.txt      # remove group read on samplelog.txt
chmod o=w employees.txt      # set others=write on employees.txt
```

**Octal Method**
```bash
chmod 750 /home/shared       # u=rwx, g=rx, o=—
chmod 644 pets.txt           # u=rw, g=r, o=r
```
</details>

---

<details>
<summary><strong>5. Special Permissions (SUID/SGID/Sticky)</strong></summary>

- **SUID (4):** executable runs with file owner’s UID (e.g. `/usr/bin/passwd`).
- **SGID (2):**
  - File: runs with file’s GID.
  - Dir: new files inherit directory’s group.
- **Sticky (1):** on dir: only file-owner/dir-owner/root can delete/rename.

**Symbolic**
```bash
chmod u+s /usr/bin/passwd        # SUID
chmod g+s /home/shared           # SGID
chmod o+t /tmp                   # sticky
```

**Octal**
```bash
chmod 4755 /usr/bin/passwd       # SUID + rwxr-xr-x
chmod 2770 /home/shared          # SGID + rwxrwx---
chmod 1777 /tmp                  # Sticky + rwxrwxrwx
```
</details>

---

<details>
<summary><strong>6. Access Control Lists (ACLs)</strong></summary>

ACLs grant per-user/group perms alongside standard ones.

```bash
sudo apt install acl
getfacl /home/shared
setfacl -m u:navya:rwX /home/shared   # grant navya rwX
getfacl /home/shared                  # confirm + indicator
setfacl -x u:navya /home/shared       # revoke navya
```

**Default ACL**
```bash
setfacl -d -m u:navya:rwX /home/shared
```  
New files/directories inherit ACL for navya.
</details>

---

<details>
<summary><strong>7. `umask` (User File-Creation Mask)</strong></summary>

New files default to `0666` minus `umask`; dirs to `0777` minus `umask`.

```bash
umask         # e.g. 0002 → files=664, dirs=775
umask 0077    # files=600, dirs=700 (akhil-only)
```

**Persist**
```bash
echo "umask 0077" >> ~/.bashrc
source ~/.bashrc
```
</details>

---

<details>
<summary><strong>8. Inodes, Hard Links, and Soft Links</strong></summary>

## Theory & Notes

### Inode (Index Node)
- Every file and directory in Linux is tracked by a unique **inode (index node) number**.
- An inode stores metadata such as:
  - File permissions (read, write, execute)
  - Ownership (user ID and group ID)
  - Timestamps (created, modified, accessed)
  - Data block pointers (locations where actual content resides on disk)
- The **inode does NOT store the file name** — that mapping happens inside the directory structure.

```bash
# View inode number of a file
ls -li file1.txt  # -l = long listing, -i = show inode
````

---

### Hard Link (Physical Link to Inode)

* A **hard link** creates an additional name (entry) for a file that directly points to the **same inode**.
* All hard-linked files:

  * Share the same **inode number**
  * Reflect changes instantly (edit one, all reflect it)
  * Are indistinguishable from the original file
* Deleting the **original file name** does **not delete the data**, as long as another hard link still exists.
* ❌ Cannot link to **directories**
* ❌ Cannot span across **file systems**

```bash
# Create a hard link
ln file1.txt hardlink.txt

# Check inode and link count
ls -li file1.txt hardlink.txt
```

---

### Soft Link (Symbolic Link or Symlink)

* A **soft link** (also called **symbolic link** or **symlink**) is a shortcut file that **points to a file path**, not the inode directly.
* Soft links:

  * Have their **own inode**
  * Store the path of the target file
  * ✅ Can point to **directories**
  * ✅ Can span across **file systems**
* If the **target is deleted**, the soft link becomes **dangling** (broken) and unusable.

```bash
# Create a symbolic link
ln -s file1.txt softlink.txt  # -s = symbolic
ls -li softlink.txt
```

---

## 🧪 Step-by-Step Practice

```bash
# Clean up first
rm -f file1.txt hardlink.txt softlink.txt

# 1. Create a new file
echo "Linux Inodes" > file1.txt

# 2. View inode
ls -li file1.txt

# 3. Create a hard link
ln file1.txt hardlink.txt
ls -li file1.txt hardlink.txt

# 4. Edit via hard link
echo "Modified via hardlink" >> hardlink.txt

# 5. Verify change is reflected
cat file1.txt

# 6. Create symbolic link to hard link
ln -s hardlink.txt softlink.txt
ls -li softlink.txt

# 7. Delete target of symbolic link
rm hardlink.txt

# 8. Try accessing soft link
cat softlink.txt  # Expect error: No such file or directory
```

---
<details>
<summary><strong>Summary Table: Hard Link vs Soft Link</strong></summary>

---

### **Detailed Comparison: Hard Link vs Soft Link**

| Feature                         | 🔗 Hard Link                                                                 | 🧭 Soft Link (Symbolic Link)                                                  |
| ------------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **What it points to**           | Points directly to the **same inode (index node)** as the original file      | Points to the **file path**, not the inode                                    |
| **Inode number**                | ✅ Same inode number as the original file                                     | ❌ Different inode (it’s its own file)                                         |
| **Storage behavior**            | No additional storage used (shares same data blocks)                         | Small amount of storage used to store target path                             |
| **Effect of editing**           | Changes reflect in both files instantly — it’s the **same file**             | Changes go to the target file (if it exists); symlink itself contains no data |
| **If original file is deleted** | File data stays alive — other hard links still work                          | Symbolic link breaks (becomes **dangling**) and gives “No such file” error    |
| **Appearance in `ls -l`**       | Appears like a normal file                                                   | Shows with an arrow `→` pointing to target (`softlink.txt → file.txt`)        |
| **Supports directories?**       | ❌ No (to prevent loops and confusion)                                        | ✅ Yes — can point to directories                                              |
| **Crosses file systems?**       | ❌ No — must be on the same file system                                       | ✅ Yes — can link to files/directories on other mounted partitions             |
| **Use case**                    | Best when you want **redundant access** to the same file with full integrity | Best for **shortcuts, navigation**, and **configuration redirection**         |
| **Creation command**            | `ln original.txt hardlink.txt`                                               | `ln -s target.txt softlink.txt`                                               |

---