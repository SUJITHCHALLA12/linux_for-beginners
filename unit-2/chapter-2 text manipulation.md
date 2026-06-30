# Chapter-2 Text Manipulation

> [!abstract] Chapter goal
> This chapter teaches how to **view, inspect, search, filter, number, and modify text files** in Linux. These skills are useful for beginners, programmers, system administrators, DevOps learners, and cybersecurity students.

---

## 1. What is Text Manipulation in Linux?

Text manipulation means working with text files and command output from the terminal. In Linux, many important things are stored as text files, such as configuration files, logs, scripts, and command outputs.

Common text manipulation tasks:

- Viewing a file
- Showing only the first or last lines
- Searching for a word or pattern
- Filtering command output
- Showing line numbers
- Replacing one word with another
- Reading long files page by page

> [!tip] Sticky Note - Beginner mindset
> Do not try to memorize every flag immediately. First remember **what the command is used for**, then learn the most common options.

---

## 2. Viewing Files with `cat`

`cat` stands for **concatenate**. It is used to display file content in the terminal. It is best for small files.

### Basic Syntax

```bash
cat filename
cat file1 file2
cat [options] filename
```

### Examples

```bash
cat file.txt
cat /etc/snort/snort.conf
cat file1.txt file2.txt
```

Explanation:

- `cat file.txt` displays the full content of the file.
- `cat /etc/snort/snort.conf` displays the Snort configuration file.
- `cat file1.txt file2.txt` displays the contents of multiple files one after another.

> [!warning] Sticky Note - Large files
> `cat` prints the entire file at once. For large files, use `head`, `tail`, `more`, or `less` instead.

---

## 3. Finding the Head: The `head` Command

`head` is used to show the **beginning part** of a file. By default, it displays the first **10 lines**.

### Basic Syntax

```bash
head [options] filename
head -n number filename
head -c number filename
```

### Common Examples

```bash
head file.txt
head -n 5 file.txt
head -5 file.txt
head -c 20 file.txt
head -c 1K file.txt
head -c 1M file.txt
head file1.txt file2.txt
```

### Meaning of Examples

| Command | Meaning |
|---|---|
| `head file.txt` | Shows first 10 lines |
| `head -n 5 file.txt` | Shows first 5 lines |
| `head -5 file.txt` | Short form for first 5 lines |
| `head -c 20 file.txt` | Shows first 20 bytes/characters |
| `head -c 1K file.txt` | Shows first 1 KB |
| `head -c 1M file.txt` | Shows first 1 MB |
| `head file1.txt file2.txt` | Shows beginning of multiple files |

### Important Options

| Option | Meaning |
|---|---|
| `-n NUMBER` | Show specific number of lines |
| `-c NUMBER` | Show specific number of bytes/characters |
| `-q` | Quiet mode; hide file headers when multiple files are shown |
| `-v` | Verbose mode; always show file headers |
| `--help` | Show help information |
| `--version` | Show version information |

> [!tip] Sticky Note - Quick memory
> `head` means **top of the file**. If no number is given, Linux shows the first **10 lines**.

---

## 4. Using `head` with Pipes

A pipe `|` sends the output of one command into another command.

### Examples

```bash
ls -l | head
ls -l | head -n 5
ps aux | head
history | head
head /var/log/syslog
head /var/log/auth.log
```

Explanation:

- `ls -l | head` shows only the first 10 lines of the long listing.
- `ls -l | head -n 5` shows the first 5 lines of the listing.
- `ps aux | head` shows the first 10 lines from the process list.
- `history | head` shows the earliest commands from terminal history.

---

## 5. Finding the Tail: The `tail` Command

`tail` is similar to `head`, but it shows the **last part** of a file. By default, it displays the last **10 lines**.

### Basic Syntax

```bash
tail [options] filename
tail -n number filename
tail -c number filename
```

### Common Examples

```bash
tail file.txt
tail -n 5 file.txt
tail -n 50 /var/log/syslog
tail -n +5 file.txt
tail -c 10 file.txt
tail -c +10 file.txt
tail file1.txt file2.txt
```

### Meaning of Examples

| Command | Meaning |
|---|---|
| `tail file.txt` | Shows last 10 lines |
| `tail -n 5 file.txt` | Shows last 5 lines |
| `tail -n 50 /var/log/syslog` | Shows last 50 log entries |
| `tail -n +5 file.txt` | Shows from line 5 to the end |
| `tail -c 10 file.txt` | Shows last 10 bytes |
| `tail -c +10 file.txt` | Shows from the 10th byte to the end |

### Live Monitoring with `tail`

```bash
tail -f /var/log/syslog
sudo tail -f /var/log/auth.log
tail -F /var/log/syslog
```

Explanation:

- `tail -f` follows a file and displays new lines as they are added.
- `tail -F` keeps following the file even if it is rotated or recreated.
- `sudo tail -f /var/log/auth.log` is useful for watching authentication logs.

> [!important] Sticky Note - Real-world use
> `tail -f` is very important for log monitoring. System admins and cybersecurity learners use it to watch events live.

---

## 6. Combining `head` and `tail`

You can combine `head` and `tail` to extract a specific range of lines.

### Examples

```bash
head -n 20 file.txt | tail -n 5
tail -n 20 file.txt | head -n 5
```

Explanation:

- `head -n 20 file.txt | tail -n 5` shows the last 5 lines from the first 20 lines.
- `tail -n 20 file.txt | head -n 5` shows the first 5 lines from the last 20 lines.

> [!tip] Sticky Note - Pipeline thinking
> Read a pipeline from left to right: **first do this**, then send the result to the next command.

---

## 7. Numbering the Lines

Numbering lines means displaying a file with line numbers beside each line. This is useful when reading code, configuration files, logs, and error messages.

### Method 1: Numbering with `cat`

```bash
cat -n file.txt
cat -b file.txt
```

| Command | Meaning |
|---|---|
| `cat -n file.txt` | Number every line |
| `cat -b file.txt` | Number only non-empty lines |

### Method 2: Numbering with `nl`

```bash
nl file.txt
nl -ba file.txt
nl -bt file.txt
nl -v 100 file.txt
nl -i 5 file.txt
nl -s ": " file.txt
```

| Command | Meaning |
|---|---|
| `nl file.txt` | Number lines using `nl` |
| `nl -ba file.txt` | Number all lines including blank lines |
| `nl -bt file.txt` | Number only non-empty lines |
| `nl -v 100 file.txt` | Start numbering from 100 |
| `nl -i 5 file.txt` | Increment line number by 5 |
| `nl -s ": " file.txt` | Use `: ` as separator between number and text |

### Method 3: Numbering with `grep -n`

```bash
grep -n "linux" file.txt
grep -n "alog" /etc/snort/snort.conf
sudo grep -n "alog" /etc/snort/snort.conf
```

`grep -n` shows the matching lines along with line numbers.

### Method 4: Numbering with `awk`

```bash
awk '{print NR, $0}' file.txt
```

Explanation:

- `NR` means **Number of Records**.
- In this context, each line is treated as one record.
- `$0` means the full current line.

### Method 5: Numbering with `sed`

```bash
sed = file.txt
```

This prints line numbers and file text.

### Method 6: Numbering with `less`

```bash
less -N file.txt
```

This opens the file page by page with line numbers.

### Method 7: Numbering inside `vim`

```vim
:set number
:set nu
:set nonumber
```

### Method 8: Numbering with `nano`

```bash
nano -l file.txt
```

> [!tip] Sticky Note - Best beginner choices
> Use `cat -n` for quick numbering, `grep -n` for search results, and `less -N` for reading long files with line numbers.

---

## 8. Useful Keys inside `less`

`less` is used to read large files page by page.

| Key | Action |
|---|---|
| `Space` | Move one page down |
| `b` | Move one page up |
| `Enter` | Move one line down |
| `/word` | Search for a word |
| `n` | Move to next search result |
| `q` | Quit |

---

## 9. Filtering Text with `grep`

`grep` stands for **Global Regular Expression Print**. It searches for a word or pattern and prints only matching lines.

### Basic Syntax

```bash
grep [options] "pattern" filename
```

### Common Examples

```bash
grep "linux" file1.txt
grep "root" /etc/passwd
grep -i "linux" file.txt
grep -n "error" log.txt
grep -w "admin" users.txt
grep -v "root" /etc/passwd
grep -c "error" log.txt
grep -o "error" log.txt
grep -r "password" /etc/
grep -l "main" *.c
grep -L "main" *.c
grep "error" file1.txt file2.txt file3.txt
grep "include" *.c
```

### Important `grep` Options

| Option | Meaning |
|---|---|
| `-i` | Ignore case |
| `-n` | Show line numbers |
| `-w` | Match whole word |
| `-v` | Invert match; show lines that do not match |
| `-c` | Count matching lines |
| `-o` | Show only matched text |
| `-r` | Search recursively in directories |
| `-l` | Show filenames that contain matches |
| `-L` | Show filenames that do not contain matches |
| `-A N` | Show N lines after the match |
| `-B N` | Show N lines before the match |
| `-C N` | Show N lines around the match |
| `-e` | Search multiple patterns |
| `-E` | Use extended regular expressions |

### Context Search Examples

```bash
grep -B 2 "error" log.txt
grep -A 3 "error" log.txt
grep -C 2 "error" log.txt
grep -e "error" -e "failed" log.txt
grep -E "error|failed|denied" log.txt
```

> [!important] Sticky Note - Cybersecurity use
> `grep` is very useful for searching logs. Search words like `failed`, `denied`, `error`, `root`, `password`, or suspicious usernames.

---

## 10. Using `sed` to Find and Replace

`sed` stands for **stream editor**. It is used to search, replace, delete, insert, or modify text from files or command output.

### Basic Syntax

```bash
sed 's/search_text/replace_text/options' filename
```

### Examples

```bash
sed 's/old-word/new-word/' file.txt
sed 's/mysql/MySQL/' snort.conf
sed 's/mysql/MySQL/g' snort.conf
sed 's/mysql/MySQL/g' snort.conf > snort2.conf
sed 's/mysql/MySQL/2' snort.conf
sed -i 's/mysql/MySQL/g' snort.conf
sed -i.bak 's/mysql/MySQL/g' snort.conf
```

### Explanation

| Command | Meaning |
|---|---|
| `sed 's/old/new/' file.txt` | Replace first occurrence on each line |
| `sed 's/old/new/g' file.txt` | Replace all occurrences on each line |
| `sed 's/old/new/2' file.txt` | Replace the second occurrence on each line |
| `> newfile.txt` | Save output into a new file |
| `-i` | Edit the original file in place |
| `-i.bak` | Edit in place and create a backup file |

> [!warning] Sticky Note - Safe editing
> Be careful with `sed -i`. It changes the original file. For beginners, first redirect output to a new file or use `-i.bak` to create a backup.

---

## 11. Viewing Long Files with `more`

`more` is used to view long text files page by page.

### Basic Syntax

```bash
more [options] filename
more +number filename
more +/word filename
```

### Examples

```bash
more file.txt
more /etc/snort/snort.conf
more +50 file.txt
more +/mysql /etc/snort/snort.conf
```

### Useful Keys inside `more`

| Key | Action |
|---|---|
| `Space` | Move forward one full screen |
| `Enter` | Move forward one line |
| `b` | Move backward one screen |
| `/word` | Search for a word |
| `n` | Go to next search result |
| `q` | Quit from more |
| `h` | Help |
| `=` | Show current line number |
| `v` | Open current file in editor |

> [!tip] Sticky Note - more vs cat
> Use `more` when `cat` output is too long and scrolls away quickly.

---

## 12. Displaying and Filtering with `less`

`less` is a file viewing command used to display large text files page by page. It is generally more powerful than `more`.

### Basic Syntax

```bash
less [options] filename
```

### Examples

```bash
less snort.conf
less +/word file.txt
less -N /etc/snort/snort.conf
less -i file.txt
grep "word" file.txt | less
```

### Explanation

| Command | Meaning |
|---|---|
| `less snort.conf` | Opens the file page by page |
| `less +/word file.txt` | Opens file and searches for a word immediately |
| `less -N file.txt` | Opens file with line numbers |
| `less -i file.txt` | Case-insensitive searching inside less |
| `grep "word" file.txt | less` | Filter with grep and read results in less |

> [!note]
> In the last command above, use the pipe symbol `|` between `file.txt` and `less`. It means: send grep output to less.

### Useful Keys inside `less`

| Key | Action |
|---|---|
| `Space` | Move one page down |
| `b` | Move one page up |
| `Enter` | Move one line down |
| `/word` | Search for a word |
| `n` | Go to next search result |
| `q` | Quit |

> [!tip] Sticky Note - less is powerful
> For learning and real work, prefer `less` over `more` because it supports better navigation and searching.

---

## 13. The Pipe Operator `|`

The pipe operator connects commands.

### Meaning

```bash
command1 | command2
```

This means: take the output of `command1` and send it as input to `command2`.

### Examples

```bash
ls -l | head
ls -l | head -n 5
ls -l | tail
history | tail -n 5
ps aux | grep "apache2"
cat file.txt | grep "linux"
grep "error" log.txt | less
```

> [!tip] Sticky Note - Read pipes like a sentence
> `ps aux | grep apache2` means: show all processes, then filter only the lines containing `apache2`.

---

## 14. Quick Command Cheat Sheet

| Task | Command |
|---|---|
| Show full small file | `cat file.txt` |
| Show first 10 lines | `head file.txt` |
| Show first 5 lines | `head -n 5 file.txt` |
| Show last 10 lines | `tail file.txt` |
| Watch log live | `tail -f /var/log/syslog` |
| Show line numbers | `cat -n file.txt` |
| Search word | `grep "word" file.txt` |
| Search ignoring case | `grep -i "word" file.txt` |
| Search with line numbers | `grep -n "word" file.txt` |
| Replace text safely | `sed 's/old/new/g' file.txt > newfile.txt` |
| Read long file | `less file.txt` |
| Read long file with line numbers | `less -N file.txt` |
| Combine commands | `command1 | command2` |

---

## 15. Practice Lab for Beginners

Create a practice file:

```bash
cat > practice.txt << EOF
Linux is powerful.
Linux is useful for cybersecurity.
Errors can happen in log files.
Failed login attempts are important.
MySQL is a database.
mysql should be written as MySQL.
EOF
```

Now practice:

```bash
cat practice.txt
head -n 3 practice.txt
tail -n 2 practice.txt
cat -n practice.txt
grep -i "linux" practice.txt
grep -n "Failed" practice.txt
sed 's/mysql/MySQL/g' practice.txt
sed 's/mysql/MySQL/g' practice.txt > fixed.txt
less -N practice.txt
```

> [!success] Sticky Note - Learning method
> Do not only read commands. Type them, break them, fix them, and repeat. Linux becomes easy through practice.

---

## 16. Original Handwritten Note Photos

These are the original uploaded photos used to prepare the typed notes.

![[assets/ch2-text-manipulation-01.jpeg]]

![[assets/ch2-text-manipulation-02.jpeg]]

![[assets/ch2-text-manipulation-03.jpeg]]

![[assets/ch2-text-manipulation-04.jpeg]]

![[assets/ch2-text-manipulation-05.jpeg]]

![[assets/ch2-text-manipulation-06.jpeg]]

![[assets/ch2-text-manipulation-07.jpeg]]
