# 🐧 Day 06 – awk Text Processing

- [1. awk Overview](#1-awk-overview)  
  - [1.1 Basic Printing](#11-basic-printing)  
  - [1.2 Field Extraction](#12-field-extraction)  
  - [1.3 Pattern Matching](#13-pattern-matching)  
  - [1.4 Line Numbers & Field Counts](#14-line-numbers--field-counts)  
  - [1.5 Custom Field Separator](#15-custom-field-separator)  
  - [1.6 Conditionals](#16-conditionals)  
  - [1.7 Length Filtering](#17-length-filtering)  
  - [1.8 Quick Recap](#18-quick-recap)

---

<details>
<summary><strong>1. awk Overview</strong></summary>

**Note:**  
- In `awk`, the default field delimiter is whitespace.    
- `$0` → entire record (line)   
- `$1` → first field   
- `$2` → second field, etc     
- `NR` → current record number (line number)   
- `NF` → number of fields in the current record.   

- Following file is used in examples      
**samplelog.txt**

03/22 08:53:38 TRACE router_forward_getOI: source address 9.67.116.98     
03/22 08:53:38 TRACE router_forward_getOI:out inf 9.67.116.98      
03/22 08:53:38 INFO rsvp_flow_stateMachine: state RESVED, event T10UT      
03/22 08:53:38 TRACE rsvp_action_nHop:constructing a PATH    
03/22 08:53:38 TRACE flow_timer_start:started T1   
03/22 08:53:38 TRACE rsvp_flow_stateMachine: reentering state RESVED   
03/22 08:53:38 TRACE mailslot_send: sending to (9.67.116.99:0)    
03/22 08:53:52 TRACE rsvp_event: received event from RAW-IP on interface 9.67.116.98    
03/22 08:53:52 TRACE rsvp_explode_packet: v=1, flg=0, type=2, cksm=54875, ttl=255, rsv=0 len=84   
03/22 08:53:52 INFO rsvp_parse_objects: obj RSVP_HOP hop=9.67.116.99, lih=0    
03/22 08:53:52 TRACE rsvp_event_mapSession: Session=9.67.116.99:1047:6 exists    
03/22 08:53:52 INFO rsvp_flow_stateMachine: state RESVED, event RESV    
03/22 08:53:52 TRACE flow_timer_stop: Stop T4    
03/22 08:53:52 TRACE flow_timer_start: Start T4    
03/22 08:53:52 TRACE rsvp_flow_stateMachine: reentering state RESVED    
03/22 08:53:52 ERROR rsvp_flow_stateMachine: Error occurred while processing state transition  

</details>

<details>
<summary><strong>1.1 Basic Printing</strong></summary>

- **Print entire file** (like `cat`)  
  ```bash
  awk '{ print }' samplelog.txt


* `{ }` → action block
* `print` → prints `$0` by default

</details>

<details>
<summary><strong>1.2 Field Extraction</strong></summary>

* **Print only the date (field 1)**

  ```bash
  awk '{ print $1 }' samplelog.txt
  ```
* **Print date, time & log level (fields 1–3)**

  ```bash
  awk '{ print $1, $2, $3 }' samplelog.txt
  ```

  * `,` → output field separator (default is space)

</details>

<details>
<summary><strong>1.3 Pattern Matching</strong></summary>

* **Print only lines containing “ERROR”**

  ```bash
  awk '/ERROR/ { print }' samplelog.txt
  ```

  * `/…/` → pattern match
* **Print date & time for “ERROR” lines**

  ```bash
  awk '/ERROR/ { print $1, $2 }' samplelog.txt
  ```

</details>

<details>
<summary><strong>1.4 Line Numbers & Field Counts</strong></summary>

* **Print each line with its line number**

  ```bash
  awk '{ print NR, $0 }' samplelog.txt
  ```
* **Print number of fields in each line**

  ```bash
  awk '{ print NF }' samplelog.txt
  ```

</details>

<details>
<summary><strong>1.5 Custom Field Separator</strong></summary>

* **Use `:` as delimiter, then print field count**

  ```bash
  awk -F ':' '{ print NF }' samplelog.txt
  ```

  * `-F ':'` → set field separator to `:`

</details>

<details>
<summary><strong>1.6 Conditionals</strong></summary>

* **Print only “ERROR” lines via `if`**

  ```bash
  awk '{ if ($3 == "ERROR") print $0 }' samplelog.txt
  ```

  * `if (condition) action`

</details>

<details>
<summary><strong>1.7 Length Filtering</strong></summary>

* **Print only lines longer than 70 characters**

  ```bash
  awk 'length($0) > 70' samplelog.txt
  ```

  * `length($0)` → length of entire line

</details>

<details>
<summary><strong>1.8 Quick Recap</strong></summary>

| Command                               | Description                              |
| ------------------------------------- | ---------------------------------------- |
| `awk '{print}' file`                  | Print every line                         |
| `awk '{print $n}' file`               | Print only field *n*                     |
| `awk '/PAT/ {print}' file`            | Print lines matching pattern             |
| `awk '{print NR, $0}' file`           | Print line numbers with each line        |
| `awk '{print NF}' file`               | Print default field count                |
| `awk -F ':' '{print NF}' file`        | Print field count using `:` as separator |
| `awk '{if($n=="VAL") print $0}' file` | Conditional print based on field value   |
| `awk 'length($0)>N' file`             | Print lines longer than *N* characters   |

</details>