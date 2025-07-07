# 🐧 Day 01 – Bash Basics

---

## Table of Contents

- [1. Syntax & Script Basics](#1-syntax--script-basics)  
- [2. Variables](#2-variables)  
- [3. Operators](#3-operators)  
- [4. Quick Command Summary](#4-quick-command-summary)  

---

<details>
<summary><strong>1. Syntax & Script Basics</strong></summary>

### Theory & Notes

- **Shebang (`#! /bin/bash`)**  
  Tells the OS to use Bash to interpret this script.
- **Comments (`#`)**  
  Lines beginning with `#` are ignored by Bash; use them to document your code.
- **Command Order**  
  Commands execute sequentially from top to bottom.
- **Semicolons (`;`)**  
  Separate multiple commands on the same line.
- **Script Workflow**  
  1. **Create** with `vi script.sh`  
  2. **Add shebang** as first line: `#! /bin/bash`  
  3. **Make executable:** `chmod +x script.sh`  
  4. **Run:** `./script.sh`

### Example

```bash
#!/bin/bash
# greet.sh — print greeting
echo "Hello, World!"
````

```output
Hello, World!
```

---

| Command            | Purpose                  | Example              |
| ------------------ | ------------------------ | -------------------- |
| `#! /bin/bash`     | Specify Bash interpreter | N/A                  |
| `# comment`        | Ignore line              | `# backup script`    |
| `chmod +x file.sh` | Make script executable   | `chmod +x backup.sh` |
| `./script.sh`      | Run the script           | `./backup.sh`        |

</details>

---

<details>
<summary><strong>2. Variables</strong></summary>

### Theory & Notes

* **User-Defined Variables:** custom variables you set in your session (analogy: sticky note on your desk).  
  - Syntax: `NAME="value"`  
  - Example: `FAVORITE_SNACK="cookies"`

* **Environment Variables (Predefined):** system-set variables available to all shells and subprocesses (analogy: office hallway bulletin board).  
  - Syntax: `export NAME="value"`  
  - Example: `export FAVORITE_SNACK="cookies"`  
  - **Permanent via `~/.bashrc`:** append `export NAME="value"` to your `~/.bashrc` so it loads in every new session.

* **Access:** `$NAME` or `${NAME}`  
* **Read Input:** `read VAR` stores user input  
* **Unset:** `unset VAR` deletes the variable  
* **Naming Rules:** start with letter/underscore, case-sensitive, no special chars  * **Naming Rules:**  
  1. **Start Character:** must begin with a letter (`A–Z`, `a–z`) or underscore (`_`).  
     * Valid: `VAR1`, `_temp`, `myVar`  
     * Invalid: `1VAR`, `-name`, `@home`  
  2. **Allowed Characters:** letters, digits (`0–9`), and underscores only.  
  3. **No Spaces or Symbols:** cannot include spaces, hyphens, punctuation, or special chars.  
  4. **Case-Sensitive:** `MyVar` ≠ `myvar`.  
  5. **Convention:**  
     * Uppercase for variables meant as environment/config (e.g., `PATH`, `FAVORITE_SNACK`).  
     * Lowercase for internal script vars (e.g., `count`, `file_path`).


### Example

```bash
#!/bin/bash

# 1) User-defined (deck-board):
FAVORITE_SNACK="cookies"
echo "Deck-board: $FAVORITE_SNACK"

# 2) Check before export:
python3 -c 'import os; print(os.getenv("FAVORITE_SNACK"))'   # None

# 3) Export to environment (session-only):
export FAVORITE_SNACK
echo "Hallway board: $FAVORITE_SNACK"

# 4) Make permanent via ~/.bashrc:
echo 'export FAVORITE_SNACK="cookies"' >> ~/.bashrc
source ~/.bashrc

# 5) In a new shell session, verify permanence:
bash -c 'python3 -c "import os; print(os.getenv(\"FAVORITE_SNACK\"))"'
````

```output
Deck-board: cookies
None
Hallway board: cookies
cookies
cookies
```

---

| Command                           | Purpose                               | Example                                                      |
| --------------------------------- | ------------------------------------- | ------------------------------------------------------------ |
| `NAME="value"`                    | Create user-defined variable          | `FAVORITE_SNACK="cookies"`                                   |
| `echo "$NAME"`                    | Display variable value                | `echo "$FAVORITE_SNACK"`                                     |
| `export NAME`                     | Convert to environment variable       | `export FAVORITE_SNACK`                                      |
| `python3 -c '…os.getenv("NAME")'` | Check environment variable via Python | `python3 -c 'import os; print(os.getenv("FAVORITE_SNACK"))'` |
| `unset NAME`                      | Remove variable                       | `unset FAVORITE_SNACK`                                       |

</details>

---

<details>
<summary><strong>3. Operators</strong></summary>

### Theory & Notes

* **Arithmetic Operators** (inside `$((…))`)

  * Addition: `((a + b))`
  * Subtraction: `((a - b))`
  * Multiplication: `((a * b))`
  * Division: `((a / b))`
  * Modulo: `((a % b))`

* **Comparison Operators** (inside `[[ ]]`)
  * Numeric:  
    * `-eq` (equal)  
    * `-ne` (not equal)  
    * `-lt` (less than)  
    * `-gt` (greater than)  
    * `-le` (less than or equal)  
    * `-ge` (greater than or equal)
  * String:  
    * `=` (equal)  
    * `!=` (not equal)  
    * `-z` (zero-length)  
    * `-n` (non-zero length)

* **Logical Operators:**  
  * `&&` (AND)  
  * `||` (OR)  
  * `!`  (NOT)

* **Redirection & Pipes:**  
  * Output:  
    * `>`  (overwrite to file)  
    * `>>` (append to file)  
  * Input:  
    * `<`  (read from file)  
  * Pipe:  
    * `cmd1 | cmd2`  (send output of `cmd1` to input of `cmd2`)

### Example

```bash
#!/bin/bash
# operator-demo.sh
echo "Enter two numbers:"
read X Y
echo "Sum     = $((X + Y))"
echo "Product = $((X * Y))"
```

```output
Enter two numbers:
4 5
Sum     = 9
Product = 20
```
```bash
#!/bin/bash

# Numeric comparison
a=5
b=3
if [[ $a -gt $b ]]; then
  echo "$a is greater than $b"
fi

# String comparison
str1="hello"
str2="world"
if [[ $str1 != $str2 ]]; then
  echo "$str1 is not equal to $str2"
fi

# Logical operators
count=1
name=""
if [[ $count -eq 1 && -z "$name" ]]; then
  echo "Count is one AND name is empty"
fi

# Redirection & pipes
echo "Line 1" > output.txt
echo "Line 2" >> output.txt
grep "Line" < output.txt
echo "apple.txt" | grep ".txt"
```

---

| Category         | Syntax             | Purpose                |                            |                              |
| ---------------- | ------------------ | ---------------------- | -------------------------- | ---------------------------- |
| Arithmetic       | `$((num1 + num2))` | Calculate expressions  |                            |                              |
| Comparison (num) | `[[ $a -eq $b ]]`  | Compare numeric values |                            |                              |
| Comparison (str) | `[[ -z "$str" ]]`  | Test string emptiness  |                            |                              |
| Logical          | `&&`, \`           |                        | `, `!\`                    | Combine or invert conditions |
| Redirection      | `>`, `>>`, `<`     | Redirect I/O           |                            |                              |
| Pipe             | \`cmd1             | cmd2\`                 | Chain command output/input |                              |

</details>

---

<details>
<summary><strong>4. Quick Command Summary</strong></summary>

## Quick Command Summary

| Command              | Purpose                        |                       |
| -------------------- | ------------------------------ | --------------------- |
| `vi script.sh`       | Create or edit a script file   |                       |
| `chmod +x script.sh` | Make script executable         |                       |
| `./script.sh`        | Execute the script             |                       |
| `NAME="value"`       | Assign variable                |                       |
| `export VAR="value"` | Export variable to environment |                       |
| `unset VAR`          | Unset variable                 |                       |
| `$((a + b))`         | Perform arithmetic             |                       |
| `[[ ... ]]`          | Test expressions               |                       |
| \`                   | `, `>`, `>>`, `<\`             | Pipe and redirect I/O |

</details>