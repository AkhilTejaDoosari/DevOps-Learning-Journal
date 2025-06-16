
# 🐧 Linux Basics – Day 02 (80/20 Rule)

> Master 20% of Linux commands that give 80% of the results in daily DevOps practice.

---

## 📚 Table of Contents

- [📂 Directory Navigation](#-directory-navigation)
- [📁 Listing Directory Contents](#-listing-directory-contents)
- [🖥️ Terminal Essentials](#️-terminal-essentials)
- [🧠 System Information](#-system-information)
- [🧰 Getting Help](#-getting-help)
- [🔍 System Info via `uname`](#-system-info-via-uname)
- [✅ Quick Tips](#-quick-tips)

---

<details>
<summary><strong>📂 Directory Navigation</strong></summary>

| Command | Purpose | Example |
|--------|---------|---------|
| `pwd` | Show current directory path | `pwd` |
| `cd <dir>` | Change directory | `cd telusko` |
| `cd ..` | Go back one directory | `cd ..` |
| `mkdir <dir>` | Create a new folder | `mkdir telusko` |
| `mkdir -p a/b/c` | Create nested folders | `mkdir -p dev/linux/scripts` |
| `rmdir <dir>` | Remove empty directory | `rmdir tempdir` |
| `rm -rf <dir>` | Delete non-empty directory (be careful) | `rm -rf old_projects` |

</details>

---

<details>
<summary><strong>📁 Listing Directory Contents</strong></summary>

| Command | Purpose | Example |
|--------|---------|---------|
| `ls` | List contents of current dir | `ls` |
| `ls -l` | Long list with details | `ls -l` |
| `ls -a` | Show hidden files too | `ls -a` |
| `ls -lh` | Human-readable sizes | `ls -lh` |
| `ls -ltr` | Sorted by time, oldest first | `ls -ltr` |
| `ls -lt` | Sorted by time, newest first | `ls -lt` |
| `ls -ld <dir>` | List directory metadata only | `ls -ld devops/` |

</details>

---

<details>
<summary><strong>🖥️ Terminal Essentials</strong></summary>

| Command | Purpose | Example |
|--------|---------|---------|
| `clear` | Clear screen | `clear` |
| `history` | Show past commands | `history` |
| `!42` | Re-run command number 42 | `!42` |
| `!-1` | Re-run last command | `!-1` |

</details>

---

<details>
<summary><strong>🧠 System Information</strong></summary>

| Command | Purpose | Example |
|--------|---------|---------|
| `whoami` | Print current user | `whoami` |
| `who` | List logged-in users | `who` |
| `uptime` | Show system uptime | `uptime` |
| `date` | Current date and time | `date` |

</details>

---

<details>
<summary><strong>🧰 Getting Help</strong></summary>

| Command | Purpose | Example |
|--------|---------|---------|
| `man <cmd>` | Manual page for command | `man ls` |
| `whatis <cmd>` | One-line explanation | `whatis clear` |
| `whereis <cmd>` | Location of binaries, docs | `whereis uname` |
| `which <cmd>` | Path to command binary | `which ls` |

</details>

---

<details>
<summary><strong>🔍 System Info via uname</strong></summary>

| Option | What it shows | Command |
|--------|----------------|---------|
| `-s` | Kernel name | `uname -s` |
| `-r` | Kernel release | `uname -r` |
| `-n` | Hostname | `uname -n` |
| `-m` | Architecture | `uname -m` |
| `-a` | All above info | `uname -a` |

</details>

---

## ✅ Quick Tips

- Use `Tab` key to **auto-complete** commands and paths.
- Use `Up ↑` and `Down ↓` arrows to **recall past commands**.
- Combine options like `ls -lah` to see hidden files in human-readable format.
- Don’t panic! Use `man <command>` to check any command's usage.

---

📝 *Next up: File operations, redirection, and text processing tools (`cat`, `touch`, `cp`, `mv`, `rm`, etc.).*
