# 🐧 Day 02 – Bash Operators & Conditionals

---

## Table of Contents

- [1. Bash Operators](#1-bash-operators)
- [2. Bash If...Else Statements](#2-bash-ifelse-statements)
- [3. Loops](#1-loops)
- [4. Quick Command Summary](#3-quick-command-summary)

---

<details>
<summary><strong>1. Bash Operators</strong></summary>

### Theory & Notes

#### Comparison Operators

These are used for comparing numerical values inside `if` statements.

- `-eq`: Equal to  
- `-ne`: Not equal to  
- `-lt`: Less than  
- `-le`: Less than or equal to  
- `-gt`: Greater than  
- `-ge`: Greater than or equal to  

#### String Comparison Operators

Used for comparing strings. For ASCII alphabetical order (`<` and `>`), use `[[ ]]` instead of `[ ]`.

- `=`: Equal to  
- `!=`: Not equal to  
- `<`: Less than, in ASCII alphabetical order  
- `>`: Greater than, in ASCII alphabetical order  

#### Arithmetic Operators

Perform mathematical calculations within `$((...))`.

- `+`: Addition  
- `-`: Subtraction  
- `*`: Multiplication  
- `/`: Division  
- `%`: Modulus (remainder)  

#### Logical Operators

Combine multiple conditions.

- `&&`: Logical AND  
- `||`: Logical OR  
- `!`: Logical NOT  

#### File Test Operators

Check attributes of files and directories.

- `-e`: Checks if a file exists  
- `-d`: Checks if a directory exists  
- `-f`: Checks if a regular file exists  
- `-s`: Checks if a file is not empty  

### Example

```bash
#!/bin/bash

# A) Comparison & Logical
num1=15
num2=10
if [ $num1 -gt 10 ] && [ $num2 -lt 15 ]; then
  echo "Both conditions are true."
fi

# B) String Comparison
string1="apple"
string2="banana"
if [ "$string1" != "$string2" ]; then
  echo "Strings are not equal."
fi

# C) File Test
file="example.txt"
touch "$file"
if [ -e "$file" ]; then
  echo "File exists."
fi
rm "$file"
````

```output
Both conditions are true.
Strings are not equal.
File exists.
```

| Operator   | Purpose                         | Example                  |
| ---------- | ------------------------------- | ------------------------ |
| `-eq`      | Numeric equality                | `[ $num -eq 10 ]`        |
| `=`        | String equality                 | `[ "$name" = "user" ]`   |
| `&&`       | Logical AND                     | `[ cond1 ] && [ cond2 ]` |
| `[[ < ]]`  | String less than (alphabetical) | `[[ "a" < "b" ]]`        |
| `-f`       | Test for regular file           | `[ -f "/etc/passwd" ]`   |
| `$((...))` | Arithmetic expression           | `sum=$((num1 + num2))`   |

</details>

---


<details>
<summary><strong>2. Bash If...Else Statements</strong></summary>

### Theory & Notes

* **`if` statement:** Executes a block of code if a condition is true. The condition is enclosed in square brackets `[ ]`, and the block ends with `fi`.
* **`if...else` statement:** Executes a different block of code if the initial condition is false. The alternative block is introduced by `else`.
* **`elif` statement:** Allows checking multiple conditions in sequence. If the first condition is false, the next `elif` is checked.
* **Nested `if` statements:** An `if` can be placed inside another `if` for more complex logic. Each block must be closed with `fi`.

### Example

```bash
#!/bin/bash

# 1) If Statement
num=15
if [ "$num" -gt 10 ]; then
  echo "Number is greater than 10"
fi

# 2) If...Else Statement
num=8
if [ "$num" -gt 10 ]; then
  echo "Number is greater than 10"
else
  echo "Number is 10 or less"
fi

# 3) If...Elif...Else Statement
num=10
if [ "$num" -gt 10 ]; then
  echo "Number is greater than 10"
elif [ "$num" -eq 10 ]; then
  echo "Number is exactly 10"
else
  echo "Number is less than 10"
fi

# 4) Nested If Statement
num=5
if [ "$num" -gt 0 ]; then
  if [ "$num" -lt 10 ]; then
    echo "Number is between 1 and 9"
  fi
fi
```

```output
Number is greater than 10
Number is 10 or less
Number is exactly 10
Number is between 1 and 9
```

| Command              | Purpose                      | Example               |
| -------------------- | ---------------------------- | --------------------- |
| `if [ condition ]`   | Start conditional block      | `if [ $num -eq 10 ]`  |
| `then`               | Mark start of code block     | N/A                   |
| `else`               | Start alternative code block | N/A                   |
| `elif [ condition ]` | Check a secondary condition  | `elif [ $num -gt 5 ]` |
| `fi`                 | End conditional block        | N/A                   |

</details>

---

<details>
<summary><strong>3. Loops</strong></summary>

### Theory & Notes

Loops let you repeat the same block of code multiple times—handy for batch tasks or polling:

* **For Loop**  
  Use when you know the items or range up front.  
  ```bash
  for i in {1..5}; do
    echo "Iteration $i"
  done
  #output
  Iteration 1
  Iteration 2
  Iteration 3
  Iteration 4
  Iteration 5
  ```

  This runs the body five times, setting `i` from 1 to 5.

* **While Loop**   
  Best for “run until condition changes.”

  ```bash
  count=1
  while [ "$count" -le 5 ]; do
    echo "Count is $count"
    ((count++))
  done
  #output
  Count is 1
  Count is 2
  Count is 3
  Count is 4
  Count is 5
  ```

  Checks the condition each time before entering the loop.

* **Until Loop**   
  Like `while` but inverted: “run until this becomes true.”

  ```bash
  count=1
  until [ "$count" -gt 5 ]; do
    echo "Count is $count"
    ((count++))
  done
  #output
  Count is 1
  Count is 2
  Count is 3
  Count is 4
  Count is 5
  ```

  Stops once the test succeeds.

**Controlling Flow & Nested Loops**

* **`break`**  
  Stops the **current** loop immediately and continues after its `done`.  
  ```bash
  for i in {1..5}; do
    if [ "$i" -eq 3 ]; then
      echo "Stopping at 3"
      break
    fi
    echo "i = $i"
  done
   #Output:
   i = 1
   i = 2
   Stopping at 3
   ```

* **`continue`**   
  Skips the rest of the **current** iteration and jumps to the next one.

  ```bash
  #!/bin/bash
  for i in {1..5}; do
    if [ "$i" -eq 3 ]; then
    echo "Skipping 3"
    continue
    fi
    echo "Number $i"
  done
   #Output:
   Number 1
   Number 2
   Skipping 3
   Number 4
   Number 5
   ```

* **What happens:**

   1. `i` goes from 1 to 5.
   2. When `i` is 3, `continue` skips the `echo "Number $i"` and jumps to the next iteration.

* **Nested Loops**
  One loop inside another.

  ```bash
  for outer in A B; do
    echo "Outer: $outer"
    for inner in 1 2; do
      [ "$inner" = 2 ] && continue    # skips just inner loop’s iteration
      echo "  Inner: $inner"
    done
  done
  # Output:
  # Outer: A
  #   Inner: 1
  # Outer: B
  #   Inner: 1
  ```

  * Use `break 2` to exit **both** loops at once:

    ```bash
    break 2
    ```
</details>

---

<details>
<summary><strong>4. Quick Command Summary</strong></summary>

### Quick Command Summary

| Command                                        | Purpose                                 |
| ---------------------------------------------- | --------------------------------------- |
| `-eq`, `-ne`, `-gt`, `-ge`, `-lt`, `-le`       | Numeric comparison operators            |
| `=`, `!=`, `<`, `>`                            | String comparison operators             |
| `&&`, `||`, `!`                                | Logical operators                       |
| `$(( expression ))`                            | Arithmetic evaluation                   |
| `-e`, `-f`, `-d`, `-s`                         | File test operators                     |
| `if [ condition ]`                             | Start conditional statement             |
| `then`                                         | Begin “true” branch                     |
| `else`                                         | Begin “false” branch                    |
| `elif [ condition ]`                           | Additional condition in `if...else`     |
| `fi`                                           | End conditional statement               |
| `for …; do …; done`                            | For loop                                |
| `while [ condition ]; do …; done`              | While loop                              |
| `until [ condition ]; do …; done`              | Until loop                              |
| `break`                                        | Exit current loop                       |
| `continue`                                     | Skip to next iteration in loop          |

</details>