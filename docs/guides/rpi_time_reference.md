# Time Reference for krsmith436

On a **Raspberry Pi 5 (or any Linux-based system like Raspberry Pi OS)**, time and date formatting uses the **same format codes** as the Python `datetime` module and the Linux `strftime` function.
Here’s a reference table of the most useful **time format codes**:

---

### 🕒 Common `strftime` / `datetime` Format Codes

| Code | Meaning                        | Example                    |
| ---- | ------------------------------ | -------------------------- |
| `%Y` | Year (4 digits)                | `2025`                     |
| `%y` | Year (2 digits)                | `25`                       |
| `%m` | Month (01–12)                  | `11`                       |
| `%B` | Full month name                | `November`                 |
| `%b` | Abbreviated month name         | `Nov`                      |
| `%d` | Day of the month (01–31)       | `06`                       |
| `%a` | Abbreviated weekday name       | `Thu`                      |
| `%A` | Full weekday name              | `Thursday`                 |
| `%H` | Hour (00–23)                   | `19`                       |
| `%I` | Hour (01–12, 12-hour clock)    | `07`                       |
| `%p` | AM / PM                        | `PM`                       |
| `%M` | Minute (00–59)                 | `45`                       |
| `%S` | Second (00–59)                 | `09`                       |
| `%f` | Microseconds (000000–999999)   | `348572`                   |
| `%z` | UTC offset                     | `-0500`                    |
| `%Z` | Time zone name                 | `EST`                      |
| `%j` | Day of year (001–366)          | `310`                      |
| `%U` | Week number (Sunday first day) | `45`                       |
| `%W` | Week number (Monday first day) | `44`                       |
| `%c` | Locale date & time             | `Thu Nov  6 19:45:09 2025` |
| `%x` | Locale date                    | `11/06/25`                 |
| `%X` | Locale time                    | `19:45:09`                 |
| `%%` | Literal `%`                    | `%`                        |

---

### 🕒 12-hour vs 24-hour Time Format Comparison

| Format                   | Code | Example Output | Notes                   |
| ------------------------ | ---- | -------------- | ----------------------- |
| 24-hour hour             | `%H` | `19`           | 00–23                   |
| 12-hour hour             | `%I` | `07`           | 01–12                   |
| Minutes                  | `%M` | `45`           | Same for both           |
| Seconds                  | `%S` | `09`           | Same for both           |
| AM / PM                  | `%p` | `PM`           | Only for 12-hour        |
| 24-hour (HH:MM)          | `%R` | `19:45`        | Shortcut for `%H:%M`    |
| 24-hour (HH:MM:SS)       | `%T` | `19:45:09`     | Shortcut for `%H:%M:%S` |
| 12-hour (HH:MM:SS AM/PM) | `%r` | `07:45:09 PM`  | Full 12-hour format     |

---

### 💻 Example (in Python)

```python
from datetime import datetime

now = datetime.now()
print(now.strftime("%Y-%m-%d %H:%M:%S"))
# Output: 2025-11-06 19:45:09
```

---

### 🧭 Example (in Linux Shell)

```bash
date +"%Y-%m-%d %H:%M:%S"
# Output: 2025-11-06 19:45:09
```

---

### 🧭 Examples

#### In Linux terminal:

```bash
date +"%R"
# Output: 19:45
```

#### In Python:

```python
from datetime import datetime
print(datetime.now().strftime("%R"))
# Output: 19:45
```

---

### 💻 Examples

#### Python:

```python
from datetime import datetime

now = datetime.now()
# Hour and minute (12-hour clock)
print(now.strftime("%I:%M %p"))  
# Example output: 07:45 PM

# Full 12-hour time with seconds
print(now.strftime("%r"))  
# Example output: 07:45:09 PM
```

#### Linux Shell:

```bash
# Hour and minute with AM/PM
date +"%I:%M %p"
# Output: 07:45 PM

# Full 12-hour time with seconds
date +"%r"
# Output: 07:45:09 PM
```


---

### 💻 Example (Python)

```python
from datetime import datetime

now = datetime.now()

print(now.strftime("%H:%M"))     # 24-hour → 19:45
print(now.strftime("%I:%M %p"))  # 12-hour → 07:45 PM
```

---

### 🖥 Example (Raspberry Pi Terminal)

```bash
date +"%H:%M"     # 24-hour
date +"%I:%M %p"  # 12-hour
```

---

💡 **Tip:** If you dislike the leading zero in 12-hour time (`07:45 PM`), many systems support:

```
%-I:%M %p
```

Example → `7:45 PM`

---

### Here are **10 very useful `date` command tricks** for **Raspberry Pi / Linux automation scripts**. These are commonly used in **cron jobs, logging, data capture, and embedded automation**. ⚙️

#### 1️⃣ Current Unix Timestamp

Seconds since **Jan 1, 1970**.

```bash
date +%s
```

Example:

```
1773075892
```

Useful for:

* Time comparisons
* Measuring elapsed time
* Unique IDs

Example timer:

```bash
start=$(date +%s)
sleep 5
end=$(date +%s)
echo "Elapsed: $((end-start)) seconds"
```

#### 2️⃣ Millisecond Timestamp

```bash
date +%s%3N
```

Example:

```
1773075892451
```

Useful for:

* High-resolution logs
* Performance timing

#### 3️⃣ Date for Filenames

Safe for filesystems.

```bash
date +"%Y%m%d_%H%M%S"
```

Example:

```
20260309_104955
```

Example backup script:

```bash
tar -czf backup_$(date +%Y%m%d_%H%M%S).tar.gz folder/
```

#### 4️⃣ Log Entry Timestamp

Very common format.

```bash
date +"%Y-%m-%d %H:%M:%S"
```

Example logging:

```bash
echo "$(date '+%Y-%m-%d %H:%M:%S') System started"
```

#### 5️⃣ Yesterday / Tomorrow

```bash
date -d "yesterday"
date -d "tomorrow"
```

Example:

```
Sun Mar  8 10:55:22 EDT 2026
```

Specific format:

```bash
date -d "yesterday" +"%Y-%m-%d"
```

#### 6️⃣ Add or Subtract Time

```bash
date -d "+5 minutes"
date -d "+2 hours"
date -d "-1 day"
```

Example:

```
date -d "+30 seconds"
```

#### 7️⃣ Convert Unix Timestamp → Date

```bash
date -d @1773075892
```

Example:

```
Mon Mar  9 10:44:52 EDT 2026
```

Very useful for **log analysis**.

#### 8️⃣ Countdown Timer

Example script:

```bash
future=$(date -d "+10 minutes" +%s)
now=$(date +%s)

echo "$((future-now)) seconds remaining"
```

#### 9️⃣ ISO-8601 Timestamp (API Standard)

```bash
date --iso-8601=seconds
```

Example:

```
2026-03-09T10:52:21-04:00
```

Common in:

* APIs
* JSON
* databases

#### 🔟 Detect Time Change

Useful for scheduled tasks.

Example: run something **every 5 minutes**

```bash
if [ $(date +%M) -eq 0 ]; then
  echo "Top of the hour"
fi
```

#### ⭐ Super Useful One-Line Logger

Many engineers use this pattern:

```bash
echo "$(date '+%Y-%m-%d %H:%M:%S') | message" >> app.log
```

Example output:

```
2026-03-09 10:56:11 | Sensor started
```

---

💡 **Raspberry Pi Pro Tip:**
If you're doing **data logging or automation**, this format is the best overall:

```
%Y-%m-%d %H:%M:%S.%3N
```

Example:

```
2026-03-09 10:56:11.347
```

It gives **milliseconds**, which is extremely useful for debugging.