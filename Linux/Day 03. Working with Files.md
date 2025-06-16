# 🐧 Day 03 - Working with Files & File Content

## Table of Contents
- [1. Create & Inspect Files](#1-create--inspect-files)  
- [2. Copying Files](#2-copying-files)  
- [3. Moving / Renaming Files](#3-moving--renaming-files)  
- [4. Deleting Files](#4-deleting-files)  
- [5. Absolute vs Relative Paths](#5-absolute-vs-relative-paths)  
- [6. Viewing File Contents](#6-viewing-file-contents)  
- [7. Previewing File Sections](#7-previewing-file-sections)  
- [8. Page-by-Page Navigation](#8-page-by-page-navigation)  
- [9. Filesystem Types in Linux](#9-filesystem-types-in-linux)  
- [10. Quick Command Summary](#10-quick-command-summary)  
- [11. Interview Tips](#11-interview-tips)  

---

<details>
<summary><strong>1. Create & Inspect Files</strong></summary>

| Command | Description                                   | Syntax             | Example           |
| ------- | --------------------------------------------- | ------------------ | ----------------- |
| `touch` | Creates an empty file or updates timestamp    | `touch &lt;filename&gt;` | `touch first.txt` |
| `file`  | Identifies the type of a file                 | `file &lt;filename&gt;`  | `file first.txt`  |
| `stat`  | Displays file metadata like size, access time | `stat &lt;filename&gt;`  | `stat first.txt`  |

</details>

---

<details>
<summary><strong>2. Copying Files</strong></summary>

| Command | Description                              | Syntax                      | Example                    |
| ------- | ---------------------------------------- | --------------------------- | -------------------------- |
| `cp`    | Copies files or directories              | `cp &lt;source&gt; &lt;destination&gt;` | `cp first.txt copy.txt`    |
| `cp -i` | Interactive mode (asks before overwrite) | `cp -i &lt;src&gt; &lt;dest&gt;`        | `cp -i first.txt test.txt` |
| `cp -v` | Verbose output (shows copying process)   | `cp -v &lt;src&gt; &lt;dest&gt;`        | `cp -v first.txt test.txt` |
| `cp -r` | Copies entire directories recursively    | `cp -r dir1/ dir2/`         | `cp -irv src/ backup/`     |

> 💡 **Tip**: Use `cp -i` to avoid accidental overwrites; `-v` for visibility; `-r` for directories.

</details>

---

<details>
<summary><strong>3. Moving / Renaming Files</strong></summary>

| Command | Description                        | Syntax            | Example                 |
| ------- | ---------------------------------- | ----------------- | ----------------------- |
| `mv`    | Moves or renames files/directories | `mv &lt;src&gt; &lt;dest&gt;` | `mv test.txt final.txt` |

> If `<dest>` is a directory, it moves inside; if a filename, it renames.

</details>

---

<details>
<summary><strong>4. Deleting Files</strong></summary>

| Command | Description                    | Syntax             | Example           |
| ------- | ------------------------------ | ------------------ | ----------------- |
| `rm`    | Removes a file                 | `rm &lt;filename&gt;`    | `rm final.txt`    |
| `rm -i` | Prompts before deletion        | `rm -i &lt;filename&gt;` | `rm -i file1.txt` |
| `rm -r` | Recursively delete a directory | `rm -r folder/`    | `rm -r old_data/` |
| `rm -f` | Force delete without prompts   | `rm -f &lt;filename&gt;` | `rm -f risky.txt` |

</details>

---

<details>
<summary><strong>5. Absolute vs Relative Paths</strong></summary>

| Type              | Description                    | Example                       | Use Case                     |
| ----------------- | ------------------------------ | ----------------------------- | ---------------------------- |
| **Absolute Path** | Starts from root `/`           | `/home/akhil/devops/file.txt` | Scripts and clarity          |
| **Relative Path** | From current directory         | `../docs/info.md`             | Quick terminal navigation    |

**Helpers:** `.` = current dir, `..` = parent dir

</details>

---

<details>
<summary><strong>6. Viewing File Contents</strong></summary>

| Command  | Description                               | Syntax            | Example        |
| -------- | ----------------------------------------- | ----------------- | -------------- |
| `cat`    | Print file content                        | `cat &lt;file&gt;`      | `cat demo.txt` |
| `cat -n` | Number lines                             | `cat -n &lt;file&gt;`   |                |
| `tac`    | Reverse print (last first)               | `tac &lt;file&gt;`      | `tac demo.txt` |
| `nl`     | Number lines (alternate)                 | `nl &lt;file&gt;`       | `nl script.sh` |

</details>

---

<details>
<summary><strong>7. Previewing File Sections</strong></summary>

| Command     | Description           | Syntax                    | Example            |
| ----------- | --------------------- | ------------------------- | ------------------ |
| `head`      | Show first 10 lines   | `head &lt;file&gt;`             | `head notes.txt`   |
| `head -n N` | Show first N lines    | `head -n 5 &lt;file&gt;`        |                    |
| `tail`      | Show last 10 lines    | `tail &lt;file&gt;`             | `tail logfile.txt` |
| `tail -n N` | Show last N lines     | `tail -n 7 &lt;file&gt;`        |                    |
| `tail -f`   | Follow file updates   | `tail -f &lt;file&gt;`          | `tail -f /var/log/syslog` |

</details>

---

<details>
<summary><strong>8. Page-by-Page Navigation</strong></summary>

| Command | Description                           | Syntax        | Example          |
| ------- | ------------------------------------- | ------------- | ---------------- |
| `more`  | Paginate forward only                 | `more &lt;file&gt;` | `more long.txt`  |
| `less`  | Paginate with forward/back navigation | `less &lt;file&gt;` | `less journal.txt` |

</details>

---

<details>
<summary><strong>9. Filesystem Types in Linux</strong></summary>

| Type        | Description        | Indicator |
| ----------- | ------------------ | --------- |
| Directory   | A folder           | `d`       |
| File        | Text or binary     | `-`       |
| Symlink     | Link to another    | `l`       |

Check: `ls -l`

</details>

---

<details>
<summary><strong>10. Quick Command Summary</strong></summary>

```bash
touch file.txt        # create file
file file.txt         # show type
cp -i a b             # copy with prompt
mv a b                # move/rename
rm -i a               # delete with prompt
cat file.txt          # display content
head -n 5 file.txt    # first lines
tail -f file.txt      # follow file
less file.txt         # scroll
stat file.txt         # metadata

---
