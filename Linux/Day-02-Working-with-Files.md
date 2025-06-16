# Day-03: Working with Files & File Content

## Table of Contents

- [Working with Files](#working-with-files)
  - [1. Create & Inspect Files](#1-create--inspec-files)
  - [2. Copying Files](#2-copying-files)
  - [3. Moving / Renaming Files](#3-moving---renaming-files)
  - [4. Deleting Files](#4-deleting-files)
  - [5. Absolute vs Relative Paths](#5-absolute-vs-relative-paths)
- [Working with File Content](#working-with-file-content)
  - [1. Viewing File Contents](#1-viewing-file-contents)
  - [2. Previewing File Sections](#2-previewing-file-sections)
  - [3. Page-by-Page Navigation](#3-page-by-page-navigation)
- [Filesystem Types in Linux](#filesystem-types-in-linux)
- [Quick Command Summary](#quick-command-summary)
- [Interview Tips](#interview-tips)

## Working with Files {#working-with-files}

### 1. Create & Inspect Files {#1-create--inspect-files}

| Command | Description                                   | Syntax             | Example           |
| ------- | --------------------------------------------- | ------------------ | ----------------- |
| `touch` | Creates an empty file or updates timestamp    | `touch <filename>` | `touch first.txt` |
| `file`  | Identifies the type of a file                 | `file <filename>`  | `file first.txt`  |
| `stat`  | Displays file metadata like size, access time | `stat <filename>`  | `stat first.txt`  |

---

### 2. Copying Files {#2-copying-files}

| Command | Description                              | Syntax                      | Example                    |
| ------- | ---------------------------------------- | --------------------------- | -------------------------- |
| `cp`    | Copies files or directories              | `cp <source> <destination>` | `cp first.txt copy.txt`    |
| `cp -i` | Interactive mode (asks before overwrite) | `cp -i <src> <dest>`        | `cp -i first.txt test.txt` |
| `cp -v` | Verbose output (shows copying process)   | `cp -v <src> <dest>`        | `cp -v first.txt test.txt` |
| `cp -r` | Copies entire directories recursively    | `cp -r dir1/ dir2/`         | `cp -irv src/ backup/`     |

>  **Tip**: The source is never modified; only the destination is created or overwritten.

---

### 3. Moving / Renaming Files {#3-moving--renaming-files}

| Command | Description                        | Syntax            | Example                 |
| ------- | ---------------------------------- | ----------------- | ----------------------- |
| `mv`    | Moves or renames files/directories | `mv <src> <dest>` | `mv test.txt final.txt` |

> If `dest` is a directory, the file is moved into it. If it’s a filename, it renames.

---

### 4. Deleting Files {#4-deleting-files}

| Command | Description                    | Syntax             | Example           |
| ------- | ------------------------------ | ------------------ | ----------------- |
| `rm`    | Removes a file                 | `rm <filename>`    | `rm final.txt`    |
| `rm -i` | Asks before deletion           | `rm -i <filename>` | `rm -i file1.txt` |
| `rm -r` | Recursively delete a directory | `rm -r folder/`    | `rm -r old_data/` |
| `rm -f` | Force delete (no prompts)      | `rm -f <filename>` | `rm -f risky.txt` |

---

### 5. Absolute vs Relative Paths {#5-absolute-vs-relative-paths}

| Type              | Description                    | Example                  | Use Case                     |
| ----------------- | ------------------------------ | ------------------------ | ---------------------------- |
| **Absolute Path** | Starts from `/` root directory | `/home/akhil/devops/file.txt` | For scripts and full clarity |
| **Relative Path** | Relative to current directory  | `../docs/info.md`        | For short terminal commands  |

**Navigation Helpers:**

- `.` → current directory
- `..` → one level up
- `./backup/` → subfolder in current dir
- `/etc/hosts` → absolute file

---

## Working with File Content {#working-with-file-content}

### 1. Viewing File Contents {#1-viewing-file-contents}

| Command  | Description                               | Syntax            | Example        |
| -------- | ----------------------------------------- | ----------------- | -------------- |
| `cat`    | Prints file content                       | `cat <file>`      | `cat demo.txt` |
| `cat -n` | Adds line numbers                         | `cat -n demo.txt` |                |
| `tac`    | Reversed `cat` (bottom to top)            | `tac <file>`      | `tac demo.txt` |
| `nl`     | Print with numbered lines (like `cat -n`) | `nl <file>`       | `nl script.sh` |

---

### 2. Previewing File Sections {#2-previewing-file-sections}

| Command     | Description           | Syntax                    | Example            |
| ----------- | --------------------- | ------------------------- | ------------------ |
| `head`      | Prints first 10 lines | `head <file>`             | `head notes.txt`   |
| `head -n 5` | First 5 lines         | `head -n 5 file.txt`      |                    |
| `tail`      | Prints last 10 lines  | `tail <file>`             | `tail logfile.txt` |
| `tail -n 7` | Last 7 lines          | `tail -n 7 file.txt`      |                    |
| `tail -f`   | Follows live updates  | `tail -f /var/log/syslog` | Real-time logs     |

---

### 3. Page-by-Page Navigation {#3-page-by-page-navigation}

| Command | Description                                   | Syntax        | Example            |
| ------- | --------------------------------------------- | ------------- | ------------------ |
| `more`  | View file page-by-page (forward only)         | `more <file>` | `more long.txt`    |
| `less`  | View file page-by-page (with back navigation) | `less <file>` | `less journal.txt` |

---

## Filesystem Types in Linux {#filesystem-types-in-linux}

Everything is treated as a file.

| Type        | Description              | Starts With |
| ----------- | ------------------------ | ----------- |
| Directory   | A folder                 | `d`         |
| Normal File | Text, binary, etc.       | `-`         |
| Link File   | Shortcut to another file | `l`         |

Check with:

```bash
ls -l
```

---

## Quick Command Summary {#quick-command-summary}

```bash
touch myfile.txt                  # Create file  
file myfile.txt                   # Check file type  
cp -i file1.txt file2.txt         # Copy with prompt  
mv oldname.txt newname.txt        # Rename file  
rm -i oldfile.txt                 # Delete with confirmation  
cat file.txt                      # Show contents  
head -n 5 file.txt                # First 5 lines  
tail -f /var/log/syslog           # Follow log file  
less hugefile.txt                 # Scroll forward/backward  
stat file.txt                     # File details  
```

---

## Interview Tips {#interview-tips}

- “Use `cp -i` to avoid accidental overwrites.”
- “Absolute paths are always reliable in scripts.”
- “`tail -f` is essential for monitoring live log files.”
