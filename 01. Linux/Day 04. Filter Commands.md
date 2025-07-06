# 🐧 Day 04 - Filter Commands

## Table of Contents
- [1. Pattern Searching with grep](#1-pattern-searching-with-grep)  
- [2. Most-Used grep Flags](#2-most-used-grep-flags)  
- [3. Comparing & Counting](#3-comparing--counting)  
- [4. Piping & Filtering](#4-piping--filtering)  
- [5. Real-World Examples](#5-real-world-examples)  
- [6. Quick Command Summary](#6-quick-command-summary)  
- [7. Tips](#7-tips)  
- [8. Appendix: pets.txt](#8-appendix-petstxt)  

---

<details>
<summary><strong>1. Pattern Searching with grep</strong></summary>

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

```bash
grep [OPTIONS] <pattern> <file(s)>
````

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
<summary><strong>2. Most-Used grep Flags</strong></summary>

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
<summary><strong>3. Comparing & Counting</strong></summary>

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
<summary><strong>4. Piping & Filtering</strong></summary>

## Theory & Notes

- **column:** Aligns text into columns using delimiters (tabs by default).  
- **cut:** Extracts specific fields or character ranges from each line.  
- **nl:** Adds line numbers to input.  
- **pr:** Formats text for print layouts (e.g., multi-column, headers/footers).  
- **sort:** Orders lines lexicographically, numerically, by month-name, or custom keys.  
- **tee:** Splits a stream, writing to both stdout and files.  
- **tr:** Translates or deletes characters (e.g., convert delimiters).  
- **uniq:** Filters or counts adjacent duplicate lines—useful after sorting.


| Command    | Description                                                    | Syntax                                  | Usage Example (using `pets.txt`)                                                                 |
|------------|----------------------------------------------------------------|-----------------------------------------|--------------------------------------------------------------------------------------------------|
| **column** | Formats input into a well-aligned table using delimiters       | `column [options] <filename>`           | `tr ',' '\t' < pets.txt \| column -t \| head -5`                                                 |
| **cut**    | Prints selected parts of lines (fields) from each file         | `cut -d<DELIM> -f<fields> <filename>`   | `cut -d',' -f1 pets.txt \| head -5`                                                              |
| **nl**     | Numbers the lines of a file or standard input                  | `nl <filename>`                         | `nl pets.txt \| head -5`                                                                          |
| **pr**     | Prepares a text file for printing (columns, headers, footers)  | `pr [options] <filename>`               | `pr -t -2 pets.txt \| head -20`  *(two-column layout, no header)*                                |
| **sort**   | Sorts lines of text in various orders (lexicographic, numeric, month) | `sort [options] <filename>`             | `sort -f pets.txt \| head -5` *(case-insensitive)*<br>`sort -M pets.txt \| head -5` *(month-name sort)*<br>`sort -n pets.txt \| head -5` *(numeric sort on “Age”)*<br>`sort -t',' -k1,1 pets.txt \| head -5` *(comma-delimited, sort by Owner)* |
| **tee**    | Reads from stdin and writes to both stdout and files           | `tee <filename>`                        | `pr -t -2 pets.txt \| tee formatted_pets.txt`  *(capture two-column output)*                      |
| **tr**     | Translates or deletes characters in input                      | `tr <set1> <set2> < <filename>`         | `tr ',' '\\t' < pets.txt \| head -5`<br>`tr ',' '\\t' < pets.txt \| column -t \| head -5`          |
| **uniq**   | Filters out or reports adjacent duplicate lines                | `uniq [options] <filename>`             | `cut -d',' -f2 pets.txt \| sort \| uniq -c \| head -5` *(count pet types)*<br>`cut -d',' -f2 pets.txt \| sort \| uniq -d` *(show duplicated types)* |

</details>

---

<details>
<summary><strong>5. Real-World Examples</strong></summary>

## Theory & Notes

* Examples illustrate common tasks you’ll encounter in logs, CSVs, and scripts.
* Adapt patterns and file globs to your own data.

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
<summary><strong>6. Quick Command Summary</strong></summary>

## Theory & Notes

* A rapid reminder of the most common filter commands.
* Good for a quick reference or adding to your shell aliases.

---

```bash
grep 'cat' pets.txt               # basic search
grep -i 'dog' pets.txt            # ignore case
grep -n 'rabbit' pets.txt         # with line numbers
grep -c 'snake' pets.txt          # count matches
grep -v 'cat' pets.txt            # invert match
grep -i 'dog' *                   # search all files
wc -l pets.txt                    # count lines
```

</details>

---

<details>
<summary><strong>7. Tips</strong></summary>

## Theory & Notes

* Use anchors (`^`, `$`) to match start or end of line.
* Highlight matches with `--color=auto`.
* Combine with `head`/`tail` for sampling output.
* Remember `-R` follows symlinks, while `-r` does not.

---

* Anchor start/end with `^` or `$`:

  ```bash
  grep '^Marley' pets.txt
  grep 'Ln$' pets.txt
  ```
* Highlight matches with color:

  ```bash
  grep --color=auto -i 'cat' pets.txt
  ```
* Preview first matches:

  ```bash
  grep -i 'dog' pets.txt \| head -n3
  ```
* Use `-r` vs `-R`—`-R` follows symlinks.

</details>

---

<details>
<summary><strong>8. Appendix: pets.txt</strong></summary>

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

