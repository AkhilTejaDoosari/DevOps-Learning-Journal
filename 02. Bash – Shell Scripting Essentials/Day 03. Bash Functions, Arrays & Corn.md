# 🐧 Day 03 – Bash Functions, Arrays & Corn

---

## Table of Contents

- [1. Functions](#2-functions)
- [2. Arrays](#3-arrays)
- [3. Cron & Scheduling](#3-cron-scheduling)
- [4. Quick Command Summary](#4-quick-command-summary)

---

<details>
<summary><strong>1. Functions</strong></summary>

### Theory & Notes

Functions package reusable logic into named blocks. They make scripts cleaner and easier to maintain.

* **Defining**

  ```bash
  my_function() {
    echo "Hello, World!"
  }
  ```

* **Calling**

  Execute by its name:

  ```bash
  my_function
  ```

* **Arguments & Scope**

  * Access positional parameters as `$1`, `$2`, etc.
  * Use `local var` to prevent leaking variables into the global scope.

* **Example**

  ```bash
  greet() {
    local name=$1
    echo "Hello, $name!"
  }

  greet "Akhil"
  ```

  ```output
  Hello, Akhil!
  ```

* **Returning Values**
  Bash functions don’t return data like other languages—instead you `echo` output and capture it:

  ```bash
  add() {
    local sum=$(( $1 + $2 ))
    echo "$sum"
  }

  result=$(add 5 3)
  echo "Sum is $result"
  ```

  ```output
  Sum is 8
  ```

### Example

```bash
#!/bin/bash

# A simple greeting function
greet() {
  local name=$1
  echo "Hello, $name!"
}

echo "→ Greetings:"
greet "Alice"
greet "Bob"

# Adding numbers via function
add() {
  echo $(( $1 + $2 ))
}

sum=$(add 10 7)
echo "→ Sum is $sum"
```

```output
→ Greetings:
Hello, Alice!
Hello, Bob!
→ Sum is 17
```

</details>

---

<details>
<summary><strong>2. Arrays</strong></summary>

### Theory & Notes

Arrays let you bundle multiple values under one variable:

* **Create**

  
  ```bash
   fruits=("apple" "banana" "cherry")
  ```

* **Access**

  * Single element: ${fruits[1]}
  * All elements: ${fruits[@]}

* **Length**

   
   ```bash
   echo "Total fruits: ${#fruits[@]}"
    #          ↑^^^^^
    #          count operator
   ```
   The # right before the name tells Bash “give me the length,” not the contents.
   1. Bash sees #fruits[@] and expands it to 3.
   2. Then echo prints "Total fruits: 3".

   Without the #, you get back the actual items:

   
   ```bash
   echo "${fruits[@]}"   # apple banana cherry
  ```

   * **${fruits[@]}** — expands to **all** elements of the array.
   * **${#fruits[@]}** — expands to the **number** of elements in the array.

* **Modify**

   
   ```bash
   fruits[2]="date"
   ```
   #### Here’s how Bash array indexing (value positions) works, step by step:
   Arrays start at index 0, not 1.
   The first element is at ${array[0]}, the second at ${array[1]}, and so on.

   
   ```bash
   fruits=( "apple" "banana" "cherry" )
   # indices:    0         1         2
   ```

   | Index syntax   | Expands to |
   | -------------- | ---------- |
   | ${fruits[0]} | apple    |
   | ${fruits[1]} | banana   |
   | ${fruits[2]} | cherry   |

   
   ```bash
   echo "${fruits[0]}"   # prints "apple"
   echo "${fruits[1]}"   # prints "banana"
   echo "${fruits[2]}"   # prints "cherry"
   ```
   > **Tip:** Always count your elements starting from 0. If you try ${array[3]} on a 3-element array, you’ll just get an empty string.

* **Append & Remove**

  
   ```bash
   fruits+=("elderberry")    # add
   unset fruits[0]           # remove “apple”
   ```


**1. Command-Line Arguments**

| Variable | Description                      | Example Usage    | Example Output     |
|----------|----------------------------------|------------------|--------------------|
| `$0`     | Script file name                 | `echo "$0"`      | `telusko.sh`       |
| `$*`     | All arguments as a single string | `echo "$*"`      | `java devops aws`  |
| `$1`     | First argument                   | `echo "$1"`      | `java`             |
| `$2`     | Second argument                  | `echo "$2"`      | `devops`           |
| `$#`     | Total number of arguments        | `echo "$#"`      | `3`                |

---

**2. Arrays**

| Action        | Syntax                                 | Description                                       | Example Output                       |
|---------------|----------------------------------------|---------------------------------------------------|--------------------------------------|
| Create        | `fruits=("apple" "banana" "cherry")`   | Define an array of items                          | —                                    |
| Access single | `${fruits[1]}`                         | Retrieve the element at index 1 (second item)     | `banana`                             |
| Access all    | `${fruits[@]}`                         | Retrieve all elements                             | `apple banana cherry`                |
| Length        | `${#fruits[@]}`                        | Count of elements                                 | `3`                                  |
| Modify        | `fruits[2]="date"`                     | Change the element at index 2                     | —                                    |
| Append        | `fruits+=("elderberry")`               | Add a new element to the end                      | —                                    |
| Remove        | `unset fruits[0]`                      | Remove the element at index 0 (does not re-index) | —                                    |


### Example
* **Example: 1**
```bash
$ vi arguments.sh
#! /bin/bash

echo "Script file name : $0"
echo "All cmd line args : $*"
echo "First arg : $1"
echo "Second args : $2"
echo "Total Number of args : $#"

#Output
Script file name : arguments.sh
All cmd line args : Akhil Navya Indhu
First arg : Akhil
Second args : Navya
Total Number of args : 3
```
* **Example: 2**
```bash
$ vi fruits.sh
#!/bin/bash

fruits=("apple" "banana" "cherry")
echo "All fruits: ${fruits[@]}"
echo "Second fruit: ${fruits[1]}"
echo "Count: ${#fruits[@]}"

# Update and extend
fruits[1]="grape"
fruits+=("date")
echo "Updated fruits: ${fruits[@]}"

# Remove first item
unset fruits[0]
echo "After removal: ${fruits[@]}"

#Output
All fruits: apple banana cherry
Second fruit: banana
Count: 3
Updated fruits: apple grape cherry date
After removal: grape cherry date
```

</details>

---

<details>
<summary><strong>3. Cron & Scheduling</strong></summary>

### Theory & Notes: Why & Real-World Use

**What is Cron?**  
Cron automates task execution at specific times—think of it as a calendar-based alarm for scripts.

**Real-World Scenarios:**  
- **Nightly backups:** Dump your database at 2 AM each day.  
- **Log cleanup:** Delete `/tmp/*.tmp` files every hour.  
- **Health checks:** Ping an API every 5 minutes and alert on failure.  
- **Monthly reports:** Generate billing reports on the first of each month.  
- **On startup:** Re-index a search service whenever the server reboots.

#### The Cron Daemon (`crond`)  
Runs in the background, checking schedules once per minute.  
```bash
sudo yum install cronie -y     # or apt-get install cron
sudo systemctl start crond
sudo systemctl enable crond
```
#### Cron Field Layout (labels below)

```bash
 ┌────────── minute (0 - 59)
 │ ┌──────── hour (0 - 23)
 │ │ ┌────── day of month (1 - 31)
 │ │ │ ┌──── month (1 - 12)
 │ │ │ │ ┌── day of week (0 - 6 ⇒ Sunday - Saturday, or
 │ │ │ │ │                1 - 7 ⇒ Monday - Sunday)
 ↓ ↓ ↓ ↓ ↓
 * * * * * command to be executed
```
* (0 and 7 are Sunday)

#### Example Jobs

```bash
# 1) Every minute (testing)
* * * * * echo "Heartbeat: $(date)" >> /var/log/heartbeat.log

# 2) Hourly at minute 15
15 * * * * /usr/local/bin/cleanup_tmp.sh

# 3) Daily at 2 AM
0 2 * * * /usr/local/bin/db_backup.sh >> /var/log/db_backup.log 2>&1

# 4) Weekly on Sunday at 3 AM
0 3 * * 0 /usr/local/bin/weekly_report.sh

# 5) Month-end on the last day at 23:59
59 23 L * * /usr/local/bin/month_end_tasks.sh

# 6) Every 10 minutes on weekdays
*/10 * * * 1-5 /usr/local/bin/monitor.sh

# 7) At reboot
@reboot /usr/local/bin/startup_tasks.sh
```

</details>

---

<details>
<summary><strong>4. Quick Command Summary</strong></summary>

| Command / Pattern        | Purpose                                 | Example                                   |
| ------------------------ | --------------------------------------- | ----------------------------------------- |
| `func_name() { … }`      | **Define** a function                   | `greet() { echo "Hi"; }`                  |
| `func_name args…`        | **Call** a function                     | `greet "Akhil"`                           |
| `result=$(func_name)`    | **Capture** function output             | `sum=$(add 5 3)`                          |
| `arr=(…)`                | **Create** an indexed array             | `colors=("red" "green" "blue")`           |
| `${arr[@]}`              | **Expand** all array elements           | `echo "${colors[@]}"`                     |
| `${#arr[@]}`             | **Count** elements in an array          | `echo "Count: ${#colors[@]}"`             |
| `arr+=(val)`             | **Append** an element to an array       | `colors+=("yellow")`                      |
| `unset arr[index]`       | **Remove** an element from an array     | `unset colors[1]`                         |
| `crontab -e`             | **Edit** your user’s crontab file       | —                                         |
| `systemctl start crond`  | **Start** the cron daemon (CentOS/RHEL) | —                                         |
| `systemctl enable crond` | **Enable** cron at boot                 | —                                         |
| `* * * * * command`      | **Run** `command` every minute          | `* * * * * echo "Heartbeat"`              |
| `0 2 * * * command`      | **Run** `command` daily at 2 AM         | `0 2 * * * /usr/local/bin/db_backup.sh`   |
| `@reboot command`        | **Run** `command` at system startup     | `@reboot /usr/local/bin/startup_tasks.sh` |

</details>