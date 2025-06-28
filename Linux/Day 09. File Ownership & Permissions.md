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
