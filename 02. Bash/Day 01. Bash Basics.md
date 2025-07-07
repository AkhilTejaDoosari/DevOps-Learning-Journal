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

* **Definition:** Named placeholders for values.
* **Assignment:** `NAME="value"` (no spaces around `=`).
* **Access:** `$NAME` or `${NAME}` retrieves the value.
* **Read Input:** `read VAR` prompts the user and stores input.
* **Export:** `export VAR="value"` makes it available to child processes.
* **Unset:** `unset VAR` deletes the variable.
* **Naming Rules:** Must start with a letter or underscore; case-sensitive; no special chars.

### Example

```bash
#!/bin/bash
# variable-demo.sh
read -p "Enter your name: " USER_NAME
echo "Hello, $USER_NAME!"
```

```output
Enter your name: Alice
Hello, Alice!
```

---

| Command              | Purpose                      | Example                         |
| -------------------- | ---------------------------- | ------------------------------- |
| `NAME="value"`       | Assign a value to a variable | `GREETING="Hi"`                 |
| `echo "$NAME"`       | Display variable value       | `echo "$GREETING"`              |
| `read VAR`           | Prompt and store user input  | `read CITY`                     |
| `export VAR="value"` | Share with subprocesses      | `export PATH="$HOME/bin:$PATH"` |
| `unset VAR`          | Remove the variable          | `unset TEMP_DIR`                |

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

  * Numeric: `-eq`, `-ne`, `-lt`, `-gt`, `-le`, `-ge`
  * String: `=`, `!=`, `-z` (empty), `-n` (non-empty)
* **Logical Operators:** `&&` (and), `||` (or), `!` (not)
* **Redirection & Pipes:**

  * Output: `> file`, `>> file`
  * Input: `< file`
  * Pipe: `cmd1 | cmd2`

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