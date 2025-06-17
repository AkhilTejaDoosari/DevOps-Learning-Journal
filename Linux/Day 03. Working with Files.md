# 🐧 Day 03 - Working with Files & File Content

## Table of Contents
- [1. Create & Inspect Files](#1-create--inspect-files)  
- [2. Copying / Moving / Renaming Files](#2-copying--moving--renaming-filesfiles)  
- [4. Deleting Files](#4-deleting-files)  
- [5. Absolute vs Relative Paths](#5-absolute-vs-relative-paths)  
- [6. Viewing File Contents](#6-viewing-file-contents)  
- [7. Previewing File Sections](#7-previewing-file-sections)  
- [8. Page-by-Page Navigation](#8-page-by-page-navigation)  
- [9. Filesystem Types in Linux](#9-filesystem-types-in-linux)  
- [10. Quick Command Summary](#10-quick-command-summary)  
- [7. Previewing File Sections](#7-previewing-file-sections)   
- [8. Filesystem Types in Linux](#9-filesystem-types-in-linux)  
- [9. Quick Command Summary](#10-quick-command-summary)  
- [10. Tips](#11-tips)  

---

<details>
<summary><strong>1. Create & Inspect Files</strong></summary>

| Command | Description                                | Syntax               | Example           |
| ------- | ------------------------------------------ | -------------------- | ----------------- |
| `touch` | Create an empty file or update timestamp   | `touch <filename>`   | `touch file1.txt` |
| `file`  | Identify the type of a file                | `file <filename>`    | `file file1.txt`  |
| `stat`  | Display file metadata (size, timestamps)   | `stat <filename>`    | `stat file1.txt`  |

</details>

---

<details>
<summary><strong>2. Copying / Moving / Renaming Files</strong></summary>

| Command  | Description                               | Syntax                          | Example                          |
| -------- | ----------------------------------------- | ------------------------------- | -------------------------------- |
| `cp`     | Copy files or directories                 | `cp <source> <destination>`     | `cp file1.txt file2.txt`         |
| `cp -i`  | Prompt before overwrite                   | `cp -i <src> <dest>`            | `cp -i file1.txt file2.txt`      |
| `cp -v`  | Show verbose output                       | `cp -v <src> <dest>`            | `cp -v ffile1.txt file2.txt`     |
| `cp -r`  | Copy directories recursively              | `cp -r <src_dir> <dest_dir>`    | `cp -r src/ backup/`             |
| `mv`    | Move or rename files or directories  | `mv <source> <dest>`   | `mv file2.txt file3.txt`     |
</details>

---

<details>
<summary><strong>3. Deleting Files</strong></summary>

| Command | Description                           | Syntax                 | Example                     |
| ------- | ------------------------------------- | ---------------------- | --------------------------- |
| `rm`    | Remove a file                         | `rm <filename>`        | `rm file3.txt`              |
| `rm -i` | Prompt before deletion                | `rm -i <filename>`     | `rm -i file3.txt`           |
| `rm -r` | Remove directories with existing files| `rm -r <directory>`    | `rm -r devops/`             |
| `rm -f` | Force delete without prompt           | `rm -f <filename>`     | `rm -f file3.txt`           |

</details>

---

<details>
<summary><strong>4. Absolute vs Relative Paths</strong></summary>

| Type             | Description                      | Example                            | Use Case                       |
| ---------------- | -------------------------------- | ---------------------------------- | ------------------------------ |
| Absolute Path    | Starts from root (`/`)           | `/home/devops/linux/file3.txt`      | Scripts, clarity              |
| Relative Path    | From current directory           | `../docs/info.md`                  | Quick navigation               |

. = current directory, .. = parent directory

</details>

---

<details>
<summary><strong>5. Viewing File Contents</strong></summary>

| Command   | Description                         | Syntax             | Example           |
| --------- | ----------------------------------- | ------------------ | ----------------- |
| `cat`     | Print file content                  | `cat <file>`       | `cat file1.txt`   |
| `cat -n`  | Print content with line numbers     | `cat -n <file>`    | `cat -n file1.txt`|
| `tac`     | Print file content in reverse order | `tac <file>`       | `tac file1.txt`   |
| `nl`      | Number lines                        | `nl <file>`        | `nl file1.txt`    |

</details>

---

<details>
<summary><strong>6. Previewing File Sections</strong></summary>

| Command     | Description                              | Syntax               | Example                      |
| ----------- | ---------------------------------------- | -------------------- | ---------------------------- |
| `head`      | Show first 10 lines                      | `head <file>`        | `head file2.txt`             |
| `head -n`   | Show first N lines                       | `head -n 5 <file>`   | `head -n 5 file2.txt`        |
| `tail`      | Show last 10 lines                       | `tail <file>`        | `tail file2.txt`             |
| `tail -n`   | Show last N lines                        | `tail -n 7 <file>`   | `tail -n 7 file2.txt`        |
| `more`      | Paginate forward only                    | `more <file>`        | `more long.txt`              |
| `less`      | Paginate with navigation (forward/back)  | `less <file>`        | `less journal.txt`           |
</details>

---

<details>
<summary><strong>7. Filesystem Types in Linux</strong></summary>

| Type      | Description          | Indicator |
| --------- | -------------------- | --------- |
| Directory | A folder             | `d`       |
| File      | Text or binary file  | `-`       |
| Symlink   | Link to another file | `l`       |

Use `ls -l` to check.

</details>

---

<details>
<summary><strong>8. Quick Command Summary</strong></summary>

```bash
touch file.txt                # create file or update timestamp
file file.txt                 # show file type
cp -i a.txt b.txt             # copy with prompt
mv a.txt b.txt                # move or rename
rm -i a.txt                   # delete with prompt
cat file.txt                  # display content
head -n 5 file.txt            # show first lines
tail -f file.txt              # follow updates
less file.txt                 # scroll interactively
stat file.txt                 # show metadata
````

</details>

---

<details>
<summary><strong>9. Tips</strong></summary>

* Describe `cp -i` as a safeguard against accidental overwrites.
* Explain absolute vs relative paths with `/` vs `.` and `..`.
* Use `ls -l` to identify file types.
* Recall the difference between `more` and `less`.

</details>

---