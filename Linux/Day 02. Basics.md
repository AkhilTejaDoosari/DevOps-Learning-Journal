# 🐧 Day 02 - Basics

---

## Table of Contents

- [Directory Navigation](#directory-navigation)  
- [Listing Directory Contents](#listing-directory-contents)  
- [Terminal Essentials](#terminal-essentials)  
- [System Information](#system-information)  
- [Getting Help](#getting-help)  
- [System Info via `uname`](#system-info-via-uname)  

---

<details>
<summary><strong>Directory Navigation</strong></summary>

| Command       | Description                | Syntax            | Example                         |
| ------------- | -------------------------- | ----------------- | ------------------------------- |
| `mkdir`       | Create a new directory     | `mkdir <dir>`     | `mkdir devops`                  |
| `mkdir -p`    | Create nested directory    | `mkdir -p a/b/c`  | `mkdir -p DevOps/linux/playground` |
| `pwd`         | Show current directory     | `pwd`             | `pwd`                           |
| `cd`          | Change directory           | `cd <dir>`        | `cd linux`                      |
| `cd ..`       | Go up one directory level  | `cd ..`           | `cd ..`                         |
| `rmdir`       | Remove empty directory     | `rmdir <dir>`     | `rmdir devops`                  |
| `rm -rf`      | Delete non-empty directory | `rm -rf <dir>`    | `rm -rf DevOps`                 |

</details>

---

<details>
<summary><strong>Listing Directory Contents</strong></summary>

| Command        | Description                     | Syntax / Example   |
| -------------- | ------------------------------- | ------------------ | 
| `ls`           | List files and directory        | `ls`               | 
| `ls -l`        | Detailed list (permissions, size)| `ls -l`           | 
| `ls -a`        | Show hidden files               | `ls -a`            | 
| `ls -lh`       | Human-readable sizes            | `ls -lh`           | 
| `ls -lt`       | Sort by modification time       | `ls -lt`           | 
| `ls -ltr`      | Oldest first                    | `ls -ltr`          | 
| `ls -ld`       | Show folder info only           | `ls -ld <dir>` Ex: `ls -ld devops/`   |

</details>

---

<details>
<summary><strong>Terminal Essentials</strong></summary>

| Command    | Description                  | Syntax / Example    |
| ---------- | ---------------------------- | --------- | 
| `clear`    | Clear the terminal screen    | `clear`   | 
| `history`  | Show command history         | `history` | 
| `!<num>`   | Re-run command by number     | `!42`     | 
| `!-1`      | Re-run last command          | `!-1`     | 

</details>

---

<details>
<summary><strong>System Information</strong></summary>

| Command    | Description             | Syntax / Example   |
| ---------- | ----------------------- | -------- | 
| `whoami`   | Show current user       | `whoami` | 
| `who`      | List logged-in users    | `who`    | 
| `uptime`   | Show system uptime      | `uptime` | 
| `date`     | Display date and time   | `date`   | 

</details>

---

<details>
<summary><strong>Getting Help</strong></summary>

| Command         | Description                   | Syntax           | Example        |
| --------------- | ----------------------------- | ---------------- | -------------- |
| `man`           | View manual page              | `man <cmd>`      | `man ls`       |
| `whatis`        | One-line description          | `whatis <cmd>`   | `whatis clear` |
| `whereis`       | Locate binary and docs        | `whereis <cmd>`  | `whereis uname`|
| `which`         | Show command path             | `which <cmd>`    | `which ls`     |

</details>

---

<details>
<summary><strong>System Info via `uname`</strong></summary>

| Option | Description            | Syntax / Example        |
| ------ | ---------------------- | ------------- | 
| `-s`   | Kernel name            | `uname -s`    | 
| `-r`   | Kernel release         | `uname -r`    | 
| `-n`   | Hostname               | `uname -n`    | 
| `-m`   | Machine type           | `uname -m`    | 
| `-a`   | All system info        | `uname -a`    |

</details>
