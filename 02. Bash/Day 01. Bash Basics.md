# 🐧 Day 01 – Bash Basics

---

## Table of Contents

- [1. Syntax & Script Basics](#1-syntax--script-basics)
- [2. Variables](#2-variables)
- [3. Bash Data Types](#3-bash-data-types)
- [4. Quick Command Summary](#4-quick-command-summary)

---

<details>
<summary><strong>1. Syntax & Script Basics</strong></summary>

### Theory & Notes

- **Shebang (`#!/bin/bash`)**  
  Tells the OS to use Bash to interpret this script.
- **Comments (`#`)**  
  Lines beginning with `#` are ignored by Bash; use them to document your code.
- **Command Order**  
  Commands execute sequentially from top to bottom.
- **Semicolons (`;`)**  
  Separate multiple commands on the same line.
- **Script Workflow**  
  1. **Create** with `vi script.sh`  
  2. **Add shebang** as first line: `#!/bin/bash`  
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

| Command            | Purpose                  | Example              |
| ------------------ | ------------------------ | -------------------- |
| `#!/bin/bash`      | Specify Bash interpreter | N/A                  |
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
<summary><strong>3. Bash Data Types</strong></summary>

### Understanding Bash Data Types

#### Strings

Strings store text and support concatenation and substring extraction.

**Example:**

```bash
greeting="Hello"
name="Akhil"
full="$greeting, $name!"
echo "$full"
```

#### Numbers

Bash supports integer arithmetic with `$((...))`.

**Example:**

```bash
# Number example
num1=5
num2=10
sum=$((num1 + num2))
Difference=$((num2 - num1))
Product=$((num1 * num2))
Division=$((num2 / num1))
Modulas=$((num1 % num2))
echo "Sum: $sum, Difference: $Difference, Product: $Product, Division: $Division, Modulas: $Modulas"
```

#### Arrays

Indexed arrays store multiple values.

**Example:**

```bash
fruits=("apple" "banana" "cherry")
for f in "${fruits[@]}"; do
  echo "$f"
done
```

#### Associative Arrays

Key-value pairs (declare with `-A`).

**Example:**

```bash
#!bin/bash
declare -A colors
colors[apple]="red"
colors[banana]="yellow"
echo ${colors[apple]} # red
echo ${colors[banana]} # yellow
```

**Note:** Bash lacks native floating-point. Use `bc` or `awk` for decimals.

</details>

---

<details>
<summary><strong>4. Quick Command Summary</strong></summary>

| Command              | Purpose                        |                       |
| -------------------- | ------------------------------ | --------------------- |
| `vi script.sh`       | Create/edit script             |                       |
| `chmod +x script.sh` | Make script executable         |                       |
| `./script.sh`        | Execute script                 |                       |
| `NAME="value"`       | Assign variable                |                       |
| `export VAR`         | Export variable to environment |                       |
| `unset VAR`          | Remove variable                |                       |
| `$((a + b))`         | Integer arithmetic             |                       |
| `\|`                 | `, `>`, `>>`, `<\`             | Pipe and redirect I/O |

</details>