# 🐧 Day 04 - Filter Commands

## Table of Contents
- [1. Find](#1-find)
- [2. Locate](#2-locate)
- [3. Pattern Searching with grep](#3-pattern-searching-with-grep)
- [4. Most-Used grep Flags](#4-most-used-grep-flags)  
- [5. Comparing & Counting](#5-comparing--counting)  
- [6. Piping & Filtering](#6-piping--filtering)  
- [7. Real-World Examples](#7-real-world-examples)  
- [8. Quick Command Summary](#8-quick-command-summary)  
- [9. Appendix: pets.txt](#9-appendix-petstxt)  

---

<details>
<summary><strong>1. Find</strong></summary>

## Theory & Notes

- **What it does**  
  Walks the filesystem tree in real time, filtering by name, type, size, time, ownership, permissions—and can even run commands on each match.  
- **Why use it**  
  When you need the absolute latest results or complex queries (e.g. “all `.log` files older than 7 days in my backup folder”).  
- **Trade-off**  
  Slower on very large trees, but infinitely flexible.

---

| Option               | Description                                      | Syntax                                          | Example                                                       |
| -------------------- | ------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------- |
| `-name <pattern>`    | Match filename using shell wildcards (`*`)       | `find <path> -name "*.txt"`                    | `find . -name "*.log"`                                        |
| `-type f`            | Filter for **regular files**                     | `find <path> -type f`                          | `find ~/akhil/linux/backup -type f`                           |
| `-type d`            | Filter for **directories**                       | `find <path> -type d`                          | `find ~/akhil/linux/backup -type d`                           |
| `-mtime N`           | Modified **exactly** N days ago                  | `find <path> -mtime 1`                         | `find ~/akhil/linux/backup -mtime 1`                          |
| `-mtime +N`          | Modified **more than** N days ago                | `find <path> -mtime +7`                        | `find ~/akhil/linux/backup -mtime +30`                        |
| `-mtime -N`          | Modified **less than** N days ago                | `find <path> -mtime -2`                        | `find ~/akhil/linux/backup -mtime -7`                         |
| `-size Nc`           | Size **exactly** N bytes                         | `find <path> -size 441c`                       | `find ~/akhil/linux/backup -size 269c`                        |
| `-size +Nk`          | Size **greater than** N KiB                      | `find <path> -size +1k`                        | `find ~/akhil/linux/backup -size +1k`                         |
| `-size -Nc`          | Size **less than** N bytes                       | `find <path> -size -500c`                      | `find ~/akhil/linux/backup -size -500c`                       |
| `-exec … {} \;`      | Execute a command on each match                  | `find <path> -name "*.tmp" -exec rm {} \;`     | `find ~/akhil/linux/backup -type f -name "*.tmp" -exec rm {} \;` |
</details>

---

<details>
<summary><strong>2. Locate</strong></summary>

## Theory & Notes

- **What it does**  
  Instantly searches a prebuilt database (`mlocate.db`) of all filenames on disk.  
- **Why use it**  
  For lightning-fast lookups by name when you don’t need the absolute newest filesystem changes.  
- **Trade-off**  
  Results are only as fresh as the last `updatedb` run (often daily).

---

| Option                      | Description                                    | Syntax                             | Example                                              |
| --------------------------- | ---------------------------------------------- | ---------------------------------- | ---------------------------------------------------- |
| `<pattern>`                 | Substring or glob match on full path           | `locate report.pdf`                | `locate pets.txt`                                    |
| `-i`, `--ignore-case`       | Case-insensitive matching                      | `locate -i SAMPLELOG.TXT`          |                                                      |
| `-l N`, `--limit=N`         | Show only the first N results                  | `locate -l 5 pets.txt`             |                                                      |
| `-c`, `--count`             | Print the number of matches only               | `locate -c "/akhil/linux/backup/.*\.txt"` |                                               |


---

## Comparison

| Aspect           | find                                               | locate                                     |
| ---------------- | -------------------------------------------------- | ------------------------------------------ |
| **Speed**        | Slower (walks directory structure)                 | Instant (database lookup)                  |
| **Freshness**    | Always current                                     | Depends on last `updatedb`                 |
| **Flexibility**  | Match by name, type, size, time, ownership, etc.   | Match by path/name only                    |
| **Actions**      | Can run commands on each result (`-exec`)          | Returns list only                          |
| **Use case**     | Complex, precise searches                          | Quick “where is…” queries                  |

---

## Real-World Examples (using `~/akhil/linux/backup`)

Your backup folder lives at `/home/atd/akhil/linux/backup`, referenced as `~/akhil/linux/backup`.

1. **Find small files (< 500 B):**  
   ```bash
   find ~/akhil/linux/backup -type f -size -500c


Matches: `fruits.txt`, `movies.txt`, `students.txt`

2. **Find medium files (500 B – 2 KiB):**

   ```bash
   find ~/akhil/linux/backup -type f -size +500c -size -2k
   ```

   Matches: `samplelog.txt`

3. **Find large files (> 1 KiB):**

   ```bash
   find ~/akhil/linux/backup -type f -size +1k
   ```

   Matches: `pets.txt`, `samplelog.txt`

4. **Delete all `.tmp` files:**

   ```bash
   find ~/akhil/linux/backup -type f -name "*.tmp" -exec rm {} \;
   ```

5. **Locate any `pets.txt` instantly:**

   ```bash
   sudo updatedb
   locate -i pets.txt
   ```

   Returns: `/home/atd/akhil/linux/backup/pets.txt`

6. **Count backup `.txt` files with locate:**

   ```bash
   sudo updatedb
   locate -c "/akhil/linux/backup/.*\.txt"
   ```

   Returns: `5`

</details>

---

<details>
<summary><strong>3. Pattern Searching with grep</strong></summary>

## Theory & Notes

- **Command structure**  
  `grep [OPTIONS] <pattern> <file(s)>`  
- **Pattern**  
  A regular expression (or literal string) that `grep` will search for.  
- **Files**  
  One or more filenames, wildcards, or directories (with `-r`).  
- **Output**  
  By default, prints matching lines; options adjust colorization, context, counts, etc.

---


grep [OPTIONS] <pattern> <file(s)>


| Action                       | Command & Description                                               |
| ---------------------------- | ------------------------------------------------------------------- |
| Basic, case-sensitive search | `grep 'cat' pets.txt` – finds “cat” exactly as typed                |
| Ignore case-sensitive search | `grep -i 'dog' pets.txt` – matches “Dog”, “DOG”, etc.               |
| Show line numbers            | `grep -n 'rabbit' pets.txt` – prefixes lines with their line number |
| Invert match                 | `grep -v 'snake' pets.txt` – shows lines **without** “snake”        |
| Search in all files of cwd   | `grep -i 'dog' *` – searches every file in current directory        |

</details>

---

<details>
<summary><strong>4. Most-Used grep Flags</strong></summary>

## Theory & Notes

* Flags modify how `grep` interprets input and outputs results.
* Common flags often combined for powerful searches.

---

| Flag / Pattern     | Description                             | Syntax                     | Example Usage               |
| ------------------ | --------------------------------------- | -------------------------- | --------------------------- |
| **`-i`**           | Case-insensitive search                 | `grep -i <pattern> <file>` | `grep -i "cat" pets.txt`    |
| **`-w`**           | Match whole words only                  | `grep -w <pattern> <file>` | `grep -w "dog" pets.txt`    |
| **`-n`**           | Prefix matches with line numbers        | `grep -n <pattern> <file>` | `grep -n "rabbit" pets.txt` |
| **`-c`**           | Count matching lines                    | `grep -c <pattern> <file>` | `grep -c "snake" pets.txt`  |
| **`-v`**           | Invert match (show non-matching lines)  | `grep -v <pattern> <file>` | `grep -v "cat" pets.txt`    |
| **Search all**     | All files in current directory          | `grep <pattern> ./*`       | `grep -i "dog" *`           |
| **Search `*.txt`** | All `.txt` files in current directory   | `grep <pattern> *.txt`     | `grep -i "cat" *.txt`       |
| **`-r`**           | Recursive search through subdirectories | `grep -r "<pattern>" .`    | `grep -r "rabbit" .`        |

</details>

---

<details>
<summary><strong>5. Comparing & Counting</strong></summary>

## Theory & Notes

* **`wc` (“word count”)** reports counts for lines, words, and bytes.
* By default, `wc <file>` prints all three counts.
* Combine flags to focus on one metric.

---

| Task                  | Command          |
| --------------------- | ---------------- |
| Line/word/char count  | `wc pets.txt`    |
| Count only lines      | `wc -l pets.txt` |
| Count only words      | `wc -w pets.txt` |
| Count only characters | `wc -c pets.txt` |

</details>

---

<details>
<summary><strong>6. Piping & Filtering</strong></summary>

## Theory & Notes

- **Pipe (`|`)**  
  Connects the stdout of one command directly into the stdin of the next. Enables building complex, modular one-liners without temporary files.

- **sort**  
  Organizes lines of text from input or a file in ascending or descending order, based on specific criteria. By default follows ASCII ordering; use `-f` to ignore case, `-r` to reverse, `-n` for numeric sort, `-M` for month-name sort, and `-k`/`-t` to sort by a specific field.

- **uniq**  
  Removes consecutive duplicate lines from sorted input, leaving only unique ones for clarity. With `-c` prefixes each line with its occurrence count; `-d` shows one instance of each duplicate; `-D` prints all duplicate lines.

- **column**  
  Arranges input into neatly aligned columns, making data more readable. With `-t` auto-determines column widths; `-s` lets you specify a custom delimiter.

- **tr**  
  Translates or deletes characters in the input stream—useful for quick text substitutions. Specify two sets: characters in the first set are replaced by corresponding ones in the second; use `-d` to delete characters.

- **tee**  
  Reads from stdin and writes to both stdout and one or more files. Use `-a` to append rather than overwrite. Ideal for logging or capturing intermediate pipeline output.


Here are the files used in the following examples:

# Fruits.txt
Mango, Summer, 1.99   
Apple, Autumn, 1.49   
Kiwi, Winter, 2.50   
Strawberry, Spring, 2.99   
Banana, Year-round, 0.59   
Cherry, Summer, 3.99   
Pear, Autumn, 1.29   
Grapes, Autumn, 2.25   
Pineapple, Year-round, 1.75   
Blueberry, Summer, 4.50   
Watermelon, Summer, 0.39   
Peach, Summer, 1.89   
Lemon, Winter, 0.79   
Plum, Autumn, 1.99   
Apricot, Spring, 3.25   
Orange, Winter, 0.89   
Raspberry, Summer, 5.00   
Papaya, Year-round, 1.59   
Fig, Autumn, 2.75   
Blackberry, Summer, 4.75   


# employees.txt
Alice, January, 55000   
Alice, January, 55000   
Bob, February, 75000   
Bob, February, 75000   
David, March, 60000   
Alice, January, 55000   
David, March, 60000   
Alice, January, 55000   
Eve, April, 65000   
Alice, January, 55000     

### `|` (Pipe)

| Option | Description                     | Syntax               | Example                        |
|--------|---------------------------------|----------------------|--------------------------------|
| N/A    | Connect stdout of one command to stdin of the next | `<cmd1> \| <cmd2>`   | `cat Fruits.txt \| grep Summer`  |

---

### `sort`

| Option           | Description                                                      | Syntax                       | Example                                    |
|------------------|------------------------------------------------------------------|------------------------------|--------------------------------------------|
| `-t <delim>`     | Use `<delim>` as the field separator instead of whitespace       | `sort -t':' -k3 file.log`    | `sort -t',' -k3 Fruits.txt`                |
| `-k start[,end]` | Sort by a specific field (start to end positions)                | `sort -t',' -k2,2 file`      | `sort -t',' -k2,2 Fruits.txt`              |
| `-n`             | Interpret and sort by numeric value                              | `sort -n [file]`             | `sort -t',' -k3 -n Fruits.txt`             |
| `-M`             | Compare by month name                                            | `sort -M [file]`             | `sort -M Fruits.txt`                       |
| `-r`             | Reverse the sort order                                           | `sort -r [file]`             | `sort -r employees.txt`                    |
| `-f`             | Fold lower-case to upper-case (ignore case)                      | `sort -f [file]`             | `sort -f employees.txt`                    |


---

### `uniq`

| Option | Description                                                  | Syntax               | Example                                     |
|--------|--------------------------------------------------------------|----------------------|---------------------------------------------|
| `-c`   | Prefix each line with the count of occurrences               | `uniq -c [file]`     | `sort employees.txt \| uniq -c`             |
| `-d`   | Only print one instance of each group of duplicate lines     | `uniq -d [file]`     | `sort employees.txt \| uniq -d`             |
| `-D`   | Print all duplicate lines (every repeated occurrence)        | `uniq -D [file]`     | `sort employees.txt \| uniq -D`             |
| `-u`   | Only print lines that are not repeated (unique only)         | `uniq -u [file]`     | `sort employees.txt \| uniq -u`             |

---

### `column`

| Option              | Description                                      | Syntax                               | Example                                            |
|---------------------|--------------------------------------------------|--------------------------------------|----------------------------------------------------|
| `-t`                | Determine column widths and create a table       | `column -t [file]`                   | `cat Fruits.txt \| column -t -s ','`               |
| `-s <delim>`        | Specify input delimiter                          | `column -s ',' -t [file]`            | `column -s ',' -t employees.txt`                   |
| `-n`                | Do not reflow long lines                         | `column -n [file]`                   | `column -n Fruits.txt`                             |
| `-R <rowsep>`       | Specify output row separator between rows        | `column -t -R ' | ' [file]`          | `column -t -s ',' -R ' | ' Fruits.txt`             |
| `-c <columns>`      | Assume fixed output width of `<columns>` chars   | `column -c 80 [file]`                | `column -c 50 Fruits.txt`                          |

---

### `tee`

| Option | Description                                       | Syntax                  | Example                                       |
|--------|---------------------------------------------------|-------------------------|-----------------------------------------------|
| `-a`   | Append to the given file instead of overwriting   | `… \| tee -a file.log`  | `grep ERROR sample_log.txt \| tee -a errors.log` |
| `-i`   | Ignore SIGINT (Ctrl-C) while writing to files     | `… \| tee -i file.txt`  | `cat Fruits.txt \| tee -i fruits_backup.txt`     |

</details>

---

<details>
<summary><strong>7. Real-World Examples</strong></summary>

## Theory & Notes

* Examples illustrate common tasks you’ll encounter in logs, CSVs, and scripts.
* Adapt patterns and file globs to your own data.
- check 9. Appendix: pets.txt 

---

| Behavior                             | Command                     |
| ------------------------------------ | --------------------------- |
| Case-sensitive “cat” in `pets.txt`   | `grep 'cat' pets.txt`       |
| Case-insensitive “dog” in `pets.txt` | `grep -i 'dog' pets.txt`    |
| Show line numbers for “rabbit”       | `grep -n 'rabbit' pets.txt` |
| Invert match (non-“snake” lines)     | `grep -v 'snake' pets.txt`  |
| Search “dog” in all files            | `grep -i 'dog' *`           |

</details>

---

<details>
<summary><strong>8. Quick Command Summary</strong></summary>

### 1. Find

| Option               | Description                                      | Syntax                                          | Example                                                       |
| -------------------- | ------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------- |
| `-name <pattern>`    | Match filename using shell wildcards (`*`)       | `find <path> -name "*.txt"`                    | `find . -name "*.log"`                                        |
| `-type f`            | Filter for **regular files**                     | `find <path> -type f`                          | `find ~/akhil/linux/backup -type f`                           |
| `-type d`            | Filter for **directories**                       | `find <path> -type d`                          | `find ~/akhil/linux/backup -type d`                           |
| `-mtime N`           | Modified **exactly** N days ago                  | `find <path> -mtime 1`                         | `find ~/akhil/linux/backup -mtime 1`                          |
| `-mtime +N`          | Modified **more than** N days ago                | `find <path> -mtime +7`                        | `find ~/akhil/linux/backup -mtime +30`                        |
| `-mtime -N`          | Modified **less than** N days ago                | `find <path> -mtime -2`                        | `find ~/akhil/linux/backup -mtime -7`                         |
| `-size Nc`           | Size **exactly** N bytes                         | `find <path> -size 441c`                       | `find ~/akhil/linux/backup -size 269c`                        |
| `-size +Nk`          | Size **greater than** N KiB                      | `find <path> -size +1k`                        | `find ~/akhil/linux/backup -size +1k`                         |
| `-size -Nc`          | Size **less than** N bytes                       | `find <path> -size -500c`                      | `find ~/akhil/linux/backup -size -500c`                       |
| `-exec … {} \;`      | Execute a command on each match                  | `find <path> -name "*.tmp" -exec rm {} \;`     | `find ~/akhil/linux/backup -type f -name "*.tmp" -exec rm {} \;` |

---

### 2. Locate

| Option                      | Description                                    | Syntax                             | Example                                              |
| --------------------------- | ---------------------------------------------- | ---------------------------------- | ---------------------------------------------------- |
| `<pattern>`                 | Substring or glob match on full path           | `locate report.pdf`                | `locate pets.txt`                                    |
| `-i, --ignore-case`         | Case-insensitive matching                      | `locate -i SAMPLELOG.TXT`          |                                                      |
| `-l N, --limit=N`           | Show only the first N results                  | `locate -l 5 pets.txt`             |                                                      |
| `-c, --count`               | Print the number of matches only               | `locate -c "/akhil/linux/backup/.*\.txt"` |                                               |

---

### Comparison: find vs. locate

| Aspect           | find                                               | locate                                     |
| ---------------- | -------------------------------------------------- | ------------------------------------------ |
| **Speed**        | Slower (walks directory structure)                 | Instant (database lookup)                  |
| **Freshness**    | Always current                                     | Depends on last `updatedb`                 |
| **Flexibility**  | Match by name, type, size, time, ownership, etc.   | Match by path/name only                    |
| **Actions**      | Can run commands on each result (`-exec`)          | Returns list only                          |
| **Use case**     | Complex, precise searches                          | Quick “where is…” queries                  |

---

### 3. Pattern Searching with grep

| Action                       | Command & Description                                               |
| ---------------------------- | ------------------------------------------------------------------- |
| Basic, case-sensitive search | `grep 'cat' pets.txt` – finds “cat” exactly as typed                |
| Ignore case-sensitive search | `grep -i 'dog' pets.txt` – matches “Dog”, “DOG”, etc.               |
| Show line numbers            | `grep -n 'rabbit' pets.txt` – prefixes lines with their line number |
| Invert match                 | `grep -v 'snake' pets.txt` – shows lines **without** “snake”        |
| Search in all files of cwd   | `grep -i 'dog' *` – searches every file in current directory        |

---

### 4. Most-Used grep Flags

| Flag / Pattern     | Description                             | Syntax                     | Example Usage               |
| ------------------ | --------------------------------------- | -------------------------- | --------------------------- |
| **`-i`**           | Case-insensitive search                 | `grep -i <pattern> <file>` | `grep -i "cat" pets.txt`    |
| **`-w`**           | Match whole words only                  | `grep -w <pattern> <file>` | `grep -w "dog" pets.txt`    |
| **`-n`**           | Prefix matches with line numbers        | `grep -n <pattern> <file>` | `grep -n "rabbit" pets.txt` |
| **`-c`**           | Count matching lines                    | `grep -c <pattern> <file>` | `grep -c "snake" pets.txt`  |
| **`-v`**           | Invert match (show non-matching lines)  | `grep -v <pattern> <file>` | `grep -v "cat" pets.txt`    |
| **Search all**     | All files in current directory          | `grep <pattern> ./*`       | `grep -i "dog" *`           |
| **Search `*.txt`** | All `.txt` files in current directory   | `grep <pattern> *.txt`     | `grep -i "cat" *.txt`       |
| **`-r`**           | Recursive search through subdirectories | `grep -r "<pattern>" .`    | `grep -r "rabbit" .`        |

---

### 5. Comparing & Counting (wc)

| Task                  | Command          |
| --------------------- | ---------------- |
| Line/word/char count  | `wc pets.txt`    |
| Count only lines      | `wc -l pets.txt` |
| Count only words      | `wc -w pets.txt` |
| Count only characters | `wc -c pets.txt` |

---

### 6. Piping & Filtering

| Command    | Description                                                                                         | Syntax                            | Key Options                                                                                                    |
|------------|-----------------------------------------------------------------------------------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **\|**     | Chain commands by piping one’s output into another’s input                                          | `<cmd1> \| <cmd2>`                | N/A                                                                                                            |
| **sort**   | Order lines: ASCII, numeric (`-n`), month (`-M`), case-insensitive (`-f`), reverse (`-r`), or field | `sort [options] [file]`           | `-n`, `-r`, `-f`, `-M`, `-k start[,end]`, `-t <delim>`                                                         |
| **uniq**   | Filter or count adjacent duplicate lines                                                           | `uniq [options] [file]`           | `-c`, `-d`, `-D`, `-u`                                                                                         |
| **column** | Align fields into columns based on a delimiter                                                     | `column [options] [file]`         | `-t`, `-s <delim>`, `-n`, `-R <rowsep>`, `-c <columns>`                                                        |
| **tr**     | Translate or delete characters                                                                     | `tr [options] <set1> <set2> < file` | `-d`, `-s`, `-c`                                                                                                |
| **tee**    | Write stream to **stdout** and file                                                                | `… \| tee [options] <file>`       | `-a`, `-i`                                                                                                     |

---

### 7. Real-World Examples

| Behavior                             | Command                                         |
| ------------------------------------ | ----------------------------------------------- |
| Numeric sort of fruits by price      | `sort -t',' -k3 -n Fruits.txt`                  |
| List unique employees with counts    | `sort employees.txt \| uniq -c`                 |
| Tabulate fruits for readability      | `cat Fruits.txt \| tr ',' '\t' \| column -t`    |
| Count log levels in sample_log.txt   | `cat sample_log.txt \| cut -d' ' -f3 \| sort \| uniq -c \| sort -nr \| tee log_level_counts.txt` |
| Filter and save ERROR entries        | `grep ERROR sample_log.txt \| tee errors.log`   |

</details>

---

<details>
<summary><strong>9. Appendix: pets.txt</strong></summary>

Raw data file used in examples:

Zion Johnson ; DOG ; BEAGLE ; May 2013 ;10 ;  891 Coral Reef Ave City57 AZ 64920 ;555-2722
Quinn Phillips ,Parrot ,African Grey , Feb 2020 ,4,  1804 Crestwood Ave City10 TN 32918,555-6503
Payton Young , Dog ; Labrador , Jul 2022 ,2 , 2147 Lakeview Rd City36 CO 83816 ;555-6834
Taylor Brown ,Snake,Corn, Jan 2015 ,9 ,7639 Valley View Ln City91 TX 56672,555-3282
Marley Gonzales; Cat ;  Siamese ; Apr 2014; 10;342 Greenleaf Rd City2 MI 75513 ;555-5860
Chandler Lewis ,DOG ,Bulldog, Nov 2018 ,5, 909 Sandstone Way City45 NM 61113 ,555-4771
Remy Collins ;Rabbit;Dutch ;Jun 2021;2; 3294 Oak St City29 KY 75482;555-4998
Addison Stewart,cat, Persian ,  Jan 2013 , 11 ,2318 Oak St City51 AL 59025, 555-5319
Jamie Mitchell ,Dog  , Golden Retriever, Mar 2016 ,8,  711 Meadowbrook Ln City7 NC 28550 , 555-2406
Bailey Wright ;Goldfish ; Comet; Sep 2023 ;1 ;  4018 River Bend Rd City14 UT 31818 ;555-4573
Kennedy Hall ,Cat;Bengal;Aug 2015;9;6767 Willow Way City63 WI 70087;555-5964
Sawyer Evans ,dog,Poodle,Dec 2012,11 , 311 Garden Pkwy City40 CA 93218 , 555-3264
Reese Turner,GUINEA PIG,Abyssinian,Jun 2018,6,395 Valley View Ln City48 FL 72511,555-6802
Harper Allen ; Lizard ; Bearded Dragon ; Feb 2016 ; 7 ;109 Lakeview Rd City95 GA 28575 ;555-6917
Alex Johnson ,dog , Beagle  ,Jul 2020,3, 8020 Cedar Ln City22 SC 51828, 555-6098
Blake Lee ,CAT ,SIAMESE, May 2021 ,2 ,  283 Maple Ave City12 OR 95124 ,  555-4676
Presley Baker ;Snake ; Corn ;Oct 2017; 5 ;612 Garden Pkwy City44 CO 28315;555-6085
Jordan Smith ,TURTLE , Red-Eared Slider, Nov 2013 , 10 , 7110 Lakeview Rd City80 AL 61840 ,555-6261
Finley Walker,Dog ; Golden Retriever, Mar 2014 , 10 , 8521 Meadowbrook Ln City17 AZ 44760 ,555-6109
Robin Green ,cat  ,Ragdoll , Aug 2016,  8,236 Pine Rd City27 MN 44711,555-5526
Sydney Brown ;Parrot;African Grey ;Sep 2022;2 ;558 Willow Way City35 OH 75530;555-6821
Remy Harris ,Dog ,Poodle, Jan 2011 ,13  , 1807 Coral Reef Ave City21 NC 38829 ,555-6975
Zion Thompson,cat,BENGAL, Dec 2018 ,5 , 462 Lakeview Rd City30 TX 58119 ,555-4868
Tyler Davis ;Rabbit;Dutch ; Apr 2023 ;1; 259 Birch Blvd City43 ID 63981 ;555-6456
Lennon Carter , DOG ,Bulldog ,  Jun 2014 , 10 , 991 Maple Ave City34 IL 73214 ,555-5234
Bailey White,Cat , Maine Coon ,  Mar 2012 ,12 , 3420 River Bend Rd City60 OK 82557 , 555-2729
Charley Collins ,Lizard ,Bearded Dragon,May 2017,7,738 Greenleaf Rd City73 UT 29595,555-4113
Rowan Lopez ,cat, Siamese,Jan 2024,1 ,442 Valley View Ln City52 GA 60728 ,555-4870
Casey Nelson;DOG;Golden Retriever;May 2023;1;1882 Sandstone Way City3 TX 70721;555-2745
Tristan Edwards ,snake,Corn ,Oct 2021 , 3 , 2628 Coral Reef Ave City67 WA 38195 ,555-5869
Whitney Allen ,Hamster , Syrian ,  Jul 2019 ,5,651 Cedar Ln City15 NC 66245 ,555-3542
Jamie Johnson ,Dog ,Labrador, Feb 2012,12 ,849 Garden Pkwy City1 AZ 94123 ,555-4782
Blake Martinez ;Cat;Persian;Aug 2016 ;7;  909 Oak St City46 CO 21114 ;555-5730
Sydney Carter , DOG ,Poodle , Sep 2020 ,3, 430 Willow Way City77 VA 38955 , 555-6785
Reese Baker ,goldfish, Comet, Nov 2015 ,9 ,456 Hilltop St City41 CA 29621,555-5872
Quinn Gonzales ; Guinea Pig;Abyssinian ;  Jan 2017 ;7 ; 376 Maple Ave City25 WA 65833 ;555-3779
Dakota Wright,cat ,Maine Coon ,Jun 2022,2,  137 Birch Blvd City92 NM 53802 ,555-5946
Tatum Hill ,Dog ,Bulldog, Dec 2019 ,4 ,157 Cedar Ln City62 MI 73890 ,555-5451
Elliot Moore ;Rabbit ; Dutch ; Feb 2014 ;10; 669 Lakeview Rd City86 KY 42985 ;555-5156
Oakley Walker ,Turtle,Red-Eared Slider,Jul 2018,6, 224 Garden Pkwy City56 AL 35563 ,555-6244
Val Johnson ;cat ; Ragdoll ;Apr 2016;8;598 Maple Ave City72 CO 80511 ;555-6911
Peyton Young ,DOG ,Labrador , Oct 2022 ,1 , 575 Elm St City24 FL 51239 ,555-5257
Morgan Lee,Parrot,African Grey,Jun 2011 ,13 , 880 Crestwood Ave City59 TN 36981 ,555-3955
Kendall Baker ,dog  , Poodle , Mar 2013 ,11 ,3390 Lakeview Rd City85 OH 55401 ,555-2860
Harper Lewis ;Cat;Siamese ;  Dec 2023 ;0 ; 446 Valley View Ln City38 WI 74921 ; 555-6972
Casey Smith ,Snake ,cOrn , Aug 2014 , 9 ,  785 Greenleaf Rd City26 OR 60439 ,555-5147
Chandler Carter ,DOG ,Golden Retriever , Jan 2022 ,2 , 320 Pine Rd City20 VA 23815 ,

</details>

