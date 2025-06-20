# 🐧 Day 05 - Advanced Commands (awk, sed, print etc)

## Table of Contents
- [1. Sed: Stream Editor](#1-sed-stream-editor)  
- [2. Awk: Pattern Scanning & Processing](#2-awk-pattern-scanning--processing)  
- [3. Print in Awk](#3-print-in-awk)  

---

<details>
<summary><strong>1. Sed: Stream Editor</strong></summary>

## Theory & Notes

- Sed processes text line-by-line (“streams”), applying editing commands without launching an interactive editor.  
- **Pattern Space**: Each line is read into the pattern space, edited, then output (unless suppressed by `-n`).  
- **Scripts**: Use `-e` to chain multiple commands or `-f script.sed` to load from a file.  
- **Substitution**: `s/old/new/[flags]` supports:
  - `g` – global replace on each line  
  - `i` – case-insensitive match  
  - Numeric (e.g. `2`) – replace only the 2nd occurrence  
- **In-Place Editing**: `-i[.bak]` modifies files directly; optional `.bak` suffix for backups.

---

| Command | Description                             | Syntax                             | Example                                  |
| ------- | --------------------------------------- | ---------------------------------- | ---------------------------------------- |
| `sed`   | Invoke sed with script/actions         | `sed [options] '<script>' <file>`  | `sed 's/foo/bar/' file.txt`              |
| `-n`    | Suppress automatic printing            | `sed -n '<script>' <file>`         | `sed -n 'p' file.txt`                    |
| `p`     | Print pattern space (inside script)    | `/pattern/ p`                      | `sed -n '/error/ p' logfile.log`         |
| `s`     | Substitute text matching a regex       | `s/<regex>/<replacement>/[flags]`  | `sed 's/^[ \t]*//g' file.txt`            |
| `-i`    | Edit file in place (with optional .bak)| `sed -i '<script>' <file>`         | `sed -i 's/foo/bar/g' file.txt`          |

</details>

---

<details>
<summary><strong>2. Awk: Pattern Scanning & Processing</strong></summary>

## Theory & Notes

- Awk is a line-oriented scripting language designed for text processing and reporting.  
- **Records & Fields**: By default, each line is a record (`$0`); fields are `$1`, `$2`, … split by `FS` (default whitespace).  
- **BEGIN/END Blocks**:  
  - `BEGIN { … }` runs before any input is read.  
  - `END { … }` runs after all input is processed.  
- **Built-in Variables**:  
  - `NR` – current record number  
  - `NF` – number of fields in current record  
  - `FS` – input field separator  
  - `OFS` – output field separator  
- Complex Awk programs can be loaded with `-f script.awk`.

---

| Command | Description                                                  | Syntax                                        | Example                                                     |
| ------- | ------------------------------------------------------------ | --------------------------------------------- | ----------------------------------------------------------- |
| `awk`   | Invoke awk with program                                      | `awk [options] 'pattern { action }' <file>`   | `awk '/ERROR/ { print $0 }' logfile.log`                    |
| `-F`    | Specify input field separator                                | `awk -F '<sep>' '{ action }' <file>`          | `awk -F ',' '{ print $1 }' data.csv`                       |
| `BEGIN` | Block executed before processing any lines                   | part of `'BEGIN { … } … END { … }'`           | `awk 'BEGIN { print "Start" } {print $2} END {print "End"}' file.txt` |
| `END`   | Block executed after processing all lines                    | see above                                     | see above                                                   |
| `NR`    | Built-in variable for current record (line) number           | used inside action block                      | `awk '{ print NR, $0 }' file.txt`                           |
| `FS`    | Input field separator variable                               | set in `BEGIN` block                          | `awk 'BEGIN { FS = ":" } { print $1 }' /etc/passwd`         |
| `OFS`   | Output field separator variable                              | set in `BEGIN` block                          | `awk 'BEGIN { OFS = " | " } { print $1, $2 }' data.txt`     |

</details>

---

<details>
<summary><strong>3. Print in Awk</strong></summary>

## Theory & Notes

- `print` outputs data: entire records, specific fields, expressions, or string literals.  
- **Output Field Separator** (`OFS`) defines how multiple arguments are joined (default is space).  
- **Redirection**:  
  - `>` overwrites the target file  
  - `>>` appends to the target file  

---

| Usage                    | Description                                                   | Example                                           |
| ------------------------ | ------------------------------------------------------------- | ------------------------------------------------- |
| `print`                  | Print the entire record (`$0`)                                | `awk '{ print $0 }' file.txt`                     |
| `print var1, var2`       | Print multiple fields/variables separated by `OFS`            | `awk '{ print $1, $3 }' records.txt`              |
| `print "text"`           | Print a literal string                                        | `awk 'BEGIN { print "Hello, World!" }'`           |
| `print $1 + 2`           | Evaluate expression before printing                           | `awk '{ print $1 + 10 }' numbers.txt`             |
| `print $0 >> "out.txt"`  | Redirect the output of `print` to a file (append mode)        | `awk '{ print >> "results.txt" }' data.txt`       |

</details>
