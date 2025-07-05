# 🐧 Day 07 - Text Editor

> **Purpose:** Core vi/vim commands & concepts you’ll use ~80% of the time, plus common editing workflows.

---

## Table of Contents

- [1. Why vi/vim?](#1-why-vi-vim)
- [2. Core Modes](#2-core-modes)
- [3. Navigation](#3-navigation)
- [4. Basic Editing](#4-basic-editing)
- [5. Copy & Paste](#5-copy--paste)
- [6. Save & Exit](#6-save--exit)
- [7. Searching & Replacing](#7-searching--replacing)
- [8. Widely Used Workflows](#8-widely-used-workflows)

---

<details>
<summary><strong>1. Why vi/vim?</strong></summary>

- **vi:** the original Unix editor always available.  
- **vim:** “vi IMproved” with extra features; fully backward compatible.  
- **Key benefit:** ultra lightweight modal editing → fast navigation & edits without leaving the keyboard.

</details>

---

<details>
<summary><strong>2. Core Modes</strong></summary>

- **Normal (Command):** navigate & issue commands  
- **Insert (`i`):** start typing; back to Normal with `Esc`  
- **Escape (Command-line)(`:`):** file-level ops (save, quit, replace)

</details>

---

<details>
<summary><strong>3. Navigation</strong></summary>

- `h`/`j`/`k`/`l` → left/down/up/right  
- `w` → jump to next word start  
- `b` → back to previous word start  
- `0` → line start; `$` → line end  
- **Repeat count:** prepend a number (e.g., `5j` moves down 5 lines)

</details>

---

<details>
<summary><strong>4. Basic Editing</strong></summary>

- **Enter Insert:** `i` (before cursor), `a` (after cursor), `o` (new line)  

- **Delete:**  
  - `x` → delete char under cursor  
  - `dd` → delete (cut) current line  

- **Change:**  
  - `cw` → change word (enters Insert)  

- **Undo/Redo:**  
  - `u` → undo  
  - `Ctrl+R` → redo  

</details>

---

<details>
<summary><strong>5. Copy & Paste</strong></summary>

- **Yank (copy):**  
  - `yy` → copy line  
  - `5yy` → copy 5 lines  

- **Put (paste):**  
  - `p` → after cursor/line  
  - `P` → before cursor/line  

</details>

---

<details>
<summary><strong>6. Save & Exit</strong></summary>

- `:w` → save  
- `:q` → quit (fails if unsaved changes)  
- `:wq` or `:x` → save + quit (`:x` skips if no edits)  
- `:q!` → quit without saving  

</details>

---

<details>
<summary><strong>7. Searching & Replacing</strong></summary>

| Command           | Description                                           |
|-------------------|-------------------------------------------------------|
| `/pattern`        | Search forward for pattern                            |
| `?pattern`        | Search backward for pattern                           |
| `n` / `N`         | Repeat search forward / backward                      |
| `:%s/old/new/g`   | Replace all occurrences of old with new in file       |
| `:%s/old/new/gc`  | Replace with confirmation for each change             |

</details>

---

<details>
<summary><strong>8. Widely Used Workflows</strong></summary>

- **Quick Edit & Save:**  
  ```bash
  vim file.txt      # open file
  20G                # jump to line 20
  iYour text<Esc>    # insert text and exit insert mode
  :wq                # save and quit


* **Global Replace:**

  ```vim
  vim file.txt
  :%s/is/will be/gc     # replace all 'is' → 'will be' with confirmation
  ```

* **Copy & Paste Between Files:**

  ```bash
  vim file1.txt
  y10y               # yank 10 lines
  :e file2.txt       # open target file
  p                  # paste
  :w                 # save
  ```

* **Undo & Redo:**

  ```vim
  u                  # undo
  Ctrl+R             # redo
  ```

</details>

---

<details>
<summary><strong>9. Quick File Edits (echo, cat, sed) – Detailed Examples</strong></summary>

### 9.1 Append a Single Line with `echo`

**Use case:** Add a new entry to a CSV or text file without opening an editor.

```bash
# File: employees.csv
Alice, January, 55000
Bob, February, 75000


# Append a new employee record
echo "3,Carol,Sales" >> employees.csv

# Verify by showing the last line
tail -n1 employees.csv
Navya ,October ,95000
````

* `>>` means “append”; if the file doesn’t exist, it’s created.
* Always verify with `tail -n1` or `tail -n2` after appending.

---

### 9.2 Append Multiple Lines via Here-Doc (`cat`)

**Use case:** Add several entries or a block of config to a file in one command.

```bash
# File: pets.csv
Zion Johnson, DOG, BEAGLE  
Quinn Phillips, Parrot, African Grey  
Payton Young, Dog, Labrador  

# Append two new pet records
cat <<EOF >> pets.txt
Akhil Teja, Cat, Persian
Navya Bommishetti, Cat, British shorthair
EOF

# Verify by viewing the last two lines
tail -n2 pets.txt
```

* `<<EOF` opens a “here-document”; everything until the next `EOF` is sent to `cat`.
* Good for appending structured blocks.

---

### 9.3 Insert a Header Row in a CSV with `sed`

**Use case:** Add or remove specific lines based on content.

```bash
# File: fruits.txt
Mango, Summer, 1.99
Apple, Autumn, 1.49
Kiwi, Winter, 2.50
Strawberry, Spring, 2.99
Banana, Year-round, 0.59

# Insert a header before the first line
sed -i '1i type,season,price' fruits.csv

# Verify:
head -n3 fruits.csv
type,season,price
Mango, Summer, 1.99
Apple, Autumn, 1.49
```

* `1i` tells `sed` to insert before line 1.
* `-i` edits the file in-place.
