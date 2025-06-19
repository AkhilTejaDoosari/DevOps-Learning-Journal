# Writing the improved Markdown file for Day 04 into `/mnt/data`
md_content = """# 🐧 Day 04 – Advanced Text Processing

---

## Table of Contents
- [Sed: Stream Editor](#sed-stream-editor)  
- [Awk: Pattern Scanning & Processing](#awk-pattern-scanning--processing)  
- [Print in Awk](#print-in-awk)  

---

<details>
<summary><strong>Sed: Stream Editor</strong></summary>

Sed is a non-interactive text editor: it reads input line by line (a “stream”), applies scripts or commands, and writes the results. It’s especially powerful for simple substitutions, deletions, insertions, and transformations without opening a full editor.

| Command     | Description                              | Syntax                             | Example                                  |
| ----------- | ---------------------------------------- | ---------------------------------- | ---------------------------------------- |
| `sed`       | Invoke sed with script/actions           | `sed [options] '<script>' <file>`  | `sed 's/foo/bar/' file.txt`              |
| `-n`        | Suppress automatic printing              | `sed -n '<script>' <file>`         | `sed -n 'p' file.txt`                    |
| `p`         | Print pattern space                      | used inside script: `/pat/ p`       | `sed -n '/error/ p' logfile.log`         |
| `s`         | Substitute text matching a regex         | `s/<regex>/<replacement>/[flags]`  | `sed 's/^[ \\t]*//g' file.txt`           |
| `-i`        | Edit file in place                       | `sed -i '<script>' <file>`         | `sed -i 's/foo/bar/g' file.txt`          |

**Theory & Notes**  
- **Streams & Pattern Space**: Sed reads each line into a “pattern space,” applies commands, then writes it out (unless `-n` is used).  
- **Scripts**: You can group multiple commands in `-e` scripts or load from a file via `-f script.sed`.  
- **Flags**: After `s/old/new/`, flags like `g` (global), `i` (ignore case), and numeric occurrences (`2`) control substitution behavior.  
- **In-Place Edits**: With `-i`, sed modifies the file directly—backup suffix can be provided (`-i.bak`).  

</details>

<details>
<summary><strong>Awk: Pattern Scanning & Processing</strong></summary>

Awk is a full-fledged line-oriented programming language designed for text processing and reporting. It splits each input line into fields, tests patterns, and performs actions—making it ideal for column-based data.

| Command   | Description                                                            | Syntax                                                     | Example                                               |
| --------- | ---------------------------------------------------------------------- | ---------------------------------------------------------- | ----------------------------------------------------- |
| `awk`     | Invoke awk with program                                                | `awk [options] 'pattern { action }' <file>`                | `awk '/ERROR/ { print $0 }' logfile.log`              |
| `-F`      | Specify input field separator                                          | `awk -F'<sep>' '{ action }' <file>`                        | `awk -F',' '{ print $1 }' data.csv`                   |
| `BEGIN`   | Actions before processing any lines                                     | `awk 'BEGIN { ... } … END { ... }' <file>`                 | `awk 'BEGIN { print "Start" } {print $2} END {print "End"}' file.txt` |
| `END`     | Actions after processing all lines                                      | see above                                                  | see above                                             |
| `NR`      | Built-in: current record (line) number                                  | `awk '{ print NR, $0 }' file.txt`                          | prefixes each line with its number                    |
| `FS`      | Input field separator variable                                          | `awk 'BEGIN { FS=":" } { print $1 }' /etc/passwd`          | parse colon-delimited entries                         |
| `OFS`     | Output field separator variable                                         | `awk 'BEGIN { OFS=" | " } { print $1, $2 }' data.txt`      | custom output delimiter                               |

**Theory & Notes**  
- **Fields & Records**: By default, Awk splits on whitespace; each line is a “record” (`$0`), and fields are `$1`, `$2`, …  
- **Patterns**: You can match on regex (`/regex/`) or numeric tests (`NR > 5`).  
- **Actions**: Enclosed in `{ }`, these can include `print`, variable assignments, arithmetic, and built-in functions like `length()`.  
- **Scripting**: Complex Awk programs can be placed in a file (e.g. `awk -f script.awk data.txt`).  

</details>

<details>
<summary><strong>Print in Awk</strong></summary>

In Awk, `print` is the simplest way to output data: either whole records, individual fields, or computed expressions.

| Usage                    | Description                                                              | Example                                            |
| ------------------------ | ------------------------------------------------------------------------ | -------------------------------------------------- |
| `print`                  | Print the entire record (fields joined by `OFS`)                         | `awk '{ print $0 }' file.txt`                     |
| `print var1, var2`       | Print multiple variables/fields, separated by `OFS`                      | `awk '{ print $1, $3 }' records.txt`              |
| `print "text"`           | Print a literal string                                                    | `awk 'BEGIN { print "Hello, World!" }'`           |
| `print $1+2`             | Evaluate an expression before printing                                   | `awk '{ print $1 + 10 }' numbers.txt`             |
| `print $0 >> "out.txt"`  | Redirect the output of `print` to a file                                 | `awk '{ print > "results.txt" }' data.txt`        |

**Theory & Notes**  
- **Output Field Separator** (`OFS`): Controls how multiple arguments to `print` are joined (default is a space).  
- **Redirection**: Use `>` to overwrite or `>>` to append to files.  
- **Built-ins**: Combine `print` with functions like `substr()`, `toupper()`, or user-defined variables for powerful reporting.  

</details>
"""
# Write to file
file_path = '/mnt/data/Day 04 - Advanced Text Processing.md'
with open(file_path, 'w') as f:
    f.write(md_content)

file_path
