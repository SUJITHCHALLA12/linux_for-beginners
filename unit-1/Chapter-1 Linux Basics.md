---
title: "Chapter-1 Linux Basics"
tags: [linux, beginner, terminal, cybersecurity, hackers, chapter-1]
type: typed-notes
source: handwritten-study-notes
---

# Chapter-1 Linux Basics
**Typed beginner-friendly notes from your handwritten Linux Basics / Hackers bootcamp notes**
> [!TIP] Sticky Note
> Goal of this chapter: become comfortable moving around the terminal, finding files, creating/removing files and directories, and using pipes/grep for filtering output.


## Learning Outcomes
- Understand Linux command structure and how paths work.
- Move around the filesystem using pwd, cd, and ls.
- Use help tools: --help, -h, man, locate, whereis, and which.
- Search files with find using name, type, size, time, user, group, permission, and empty-file filters.
- Create, view, copy, move, rename, and remove files/directories safely.
- Use pipes and grep to filter command output like a beginner Linux hacker.

## Table of Contents
1. Linux terminal mindset
1. Linux filesystem hierarchy
1. Basic navigation commands
1. Getting help for commands
1. Searching files with find
1. Fast searching with locate
1. Finding command locations with whereis and which
1. Piping and filtering with grep
1. Creating files
1. Creating directories with mkdir
1. Removing files and directories
1. Moving and renaming with mv
1. Copying with cp
1. Revision cheat sheet and practice lab

## 1. Linux Terminal Mindset
Linux is mostly controlled through commands typed in a terminal. A command is a small program that performs one job. In cybersecurity and hacking labs, the terminal is very important because tools are often faster, more flexible, and easier to automate from the command line.

### Basic command syntax
```bash
command [options] [arguments]

# Example
ls -la /home
```
| Part | Meaning | Example |
| --- | --- | --- |
| command | The program you want to run | ls, cd, find, cp, mv |
| options / flags | Modify the command behavior. Usually start with - or -- | -l, -a, --help |
| arguments | The target of the command, such as file, directory, username, or pattern | /home, file.txt, "*.txt" |

> [!TIP] Sticky Note
> Most Linux commands follow this pattern: command + options + target. Learn the pattern, not only the examples.


### Absolute path vs relative path
| Path type | Meaning | Example |
| --- | --- | --- |
| Absolute path | Starts from root directory / | /home/sujith/Documents |
| Relative path | Starts from current working directory | Documents/file.txt |
| Current directory | The directory you are currently inside | . |
| Parent directory | One level above current directory | .. |
| Home directory | Your personal user folder | ~ or /home/sujith |

> [!WARNING] Careful
> Linux is case-sensitive. File.txt, file.txt, and FILE.txt are three different names.


## 2. Linux Filesystem Hierarchy
The Linux filesystem starts from a single root directory called /. Everything is stored under this root, including user files, system configuration files, commands, devices, and logs.
![Linux filesystem hierarchy](assets/linux-filesystem.png)

| Directory | Purpose | Beginner example |
| --- | --- | --- |
| / | Root of the entire filesystem | All paths start here |
| /home | Normal users personal directories | /home/sujith |
| /root | Home directory of the superuser/root account | Used by administrator |
| /boot | Boot files and kernel image | System startup files |
| /etc | System configuration files | /etc/ssh/sshd_config |
| /mnt | Temporary/manual mount point | Mount USB or external disk |
| /proc | Virtual view of running processes and kernel data | /proc/cpuinfo |
| /sys | Virtual view of hardware/kernel devices | Hardware info |
| /dev | Device files | /dev/sda, /dev/null |
| /bin | Essential user binaries/commands | ls, cp, mv |
| /sbin | System/admin binaries | fdisk, reboot |
| /lib | Shared libraries needed by binaries | System libraries |
| /usr/bin | More user commands and apps | python3, gcc |
| /usr/sbin | More system binaries | Admin tools |
| /usr/lib | More libraries | Application libraries |
| /var/log | System and application logs | auth.log, syslog |

> [!TIP] Sticky Note
> For hacking and system administration, /etc, /var/log, /home, /usr/bin, /bin, and /sbin are very important directories.


## 3. Basic Navigation Commands

### pwd - print working directory
pwd shows the exact directory where you are currently working.
```bash
pwd
# Example output:
/home/sujith
```

### cd - change directory
cd is used to move from one directory to another.
```bash
cd /home            # go to /home
cd ..               # go back one level
cd ../..            # go back two levels
cd ~                # go to your home directory
cd -                # go to previous directory
```
> [!TIP] Sticky Note
> Use pwd after cd when you feel lost. pwd tells you exactly where you are.


### ls - list directory contents
ls shows files and directories inside the current directory or a specified path.
```bash
ls                 # normal list
ls /home           # list /home directory
ls -l              # long listing with permissions, owner, size, time
ls -a              # show hidden files also
ls -la             # long listing including hidden files
ls -lh             # human-readable sizes
ls -R              # recursive listing
ls -t              # sort by modification time
ls -S              # sort by size
```
| Flag | Meaning |
| --- | --- |
| -l | Long format: permissions, owner, group, size, date/time, name |
| -a | All files, including hidden files that start with dot . |
| -h | Human-readable sizes like K, M, G. Usually used with -l |
| -R | Recursive listing: includes subdirectories |
| -t | Sort by modification time |
| -S | Sort by file size |

> [!TIP] Sticky Note
> Hidden files in Linux start with a dot, for example .bashrc. Use ls -la to see them.


## 4. Getting Help for Commands
Linux has built-in help systems. When you forget syntax or flags, use these commands instead of guessing.

### --help and -h
--help gives quick guidance for command usage. Some tools also support -h.
```bash
ls --help
find --help
python3 -h
```

### man - manual pages
man opens the detailed manual page for a command.
```bash
man ls
man find
man cp
```
> [!TIP] Sticky Note
> Inside man pages: press q to quit, /word to search, n to go to next match, and Space to move down.


### Quick command location tools
| Tool | Main use | Example |
| --- | --- | --- |
| which | Shows the executable being used from PATH | which bash |
| whereis | Shows binary, source, and manual page locations | whereis ls |
| locate | Fast search from a prebuilt database | locate file.txt |
| find | Live real-time search with filters | find /home -name "*.txt" |


## 5. Searching Files with find
find searches files and directories in real time. It is powerful because it can filter by name, type, size, modification time, owner, group, permissions, and can also perform actions on results.

### Basic syntax
```bash
find [starting-path] [tests] [actions]

# Example
find /home -name "file.txt"
```
| Part | Meaning | Examples |
| --- | --- | --- |
| starting-path | Where the search starts | /, /home, /var/log, . |
| tests | Conditions used to match files | -name, -iname, -type, -size, -mtime |
| actions | What to do with matched files | -print, -delete, -exec, -ls |

> [!WARNING] Careful
> Before using -delete or rm with find, first run the same find command without delete and check the output carefully.


### 5.1 Find by name
```bash
find /home -name "file.txt"
find /home -name "*.txt"
find . -name "report.pdf"
```
> [!TIP] Sticky Note
> Use quotes around patterns like "*.txt" so the shell does not expand them before find receives them.


### 5.2 Case-insensitive name search
```bash
find /home -iname "file.txt"
find /home -iname "*.PDF"
```
-iname ignores uppercase/lowercase differences.

### 5.3 Find files only or directories only
```bash
find /home -type f        # regular files only
find /home -type d        # directories only
```
| Type | Meaning |
| --- | --- |
| f | Regular file |
| d | Directory |
| l | Symbolic link |


### 5.4 Find by size
```bash
find /home -size +100M     # bigger than 100 MB
find /home -size -10M      # smaller than 10 MB
find /home -size +500k     # bigger than 500 KB
```
| Size suffix | Meaning |
| --- | --- |
| c | bytes |
| k | kilobytes |
| M | megabytes |
| G | gigabytes |

> [!TIP] Sticky Note
> + means greater than. - means less than. Example: +100M means bigger than 100 MB.


### 5.5 Find by modification/access/status-change time
```bash
find /home -mtime -7      # modified within last 7 days
find /home -mtime +30     # modified more than 30 days ago
find /home -atime -2      # accessed within last 2 days
find /home -ctime -1      # status changed within last 1 day
```
| Time test | Meaning |
| --- | --- |
| -mtime | File content modification time |
| -atime | Last access time |
| -ctime | Metadata/status change time, such as permission or owner change |


### 5.6 Find by permissions
```bash
find /home -perm 755
find /home -type f -perm 644
```
| Permission number | Meaning |
| --- | --- |
| 7 | read + write + execute = 4 + 2 + 1 |
| 6 | read + write = 4 + 2 |
| 5 | read + execute = 4 + 1 |
| 4 | read only |
| 755 | Owner rwx, group r-x, others r-x |
| 644 | Owner rw-, group r--, others r-- |

> [!TIP] Sticky Note
> Permission math: read=4, write=2, execute=1. Add them to form permission digits.


### 5.7 Find by owner and group
```bash
find /home -user sujith
find /home -group students
```

### 5.8 Find empty files or directories
```bash
find /tmp -empty
find /tmp -type f -empty
find /tmp -type d -empty
```

### 5.9 Delete matching files
```bash
find /tmp -name "*.tmp" -print
# If the above output is correct, then run:
find /tmp -name "*.tmp" -delete
```
> [!WARNING] Careful
> Never copy-paste destructive commands blindly. Test with -print first.


### 5.10 Execute a command on each found file
```bash
find /home -name "*.txt" -exec cat {} \;
find /home -name "*.log" -exec ls -lh {} \;
```
| Symbol | Meaning |
| --- | --- |
| {} | Placeholder for each found file |
| \; | Ends the -exec command. The backslash protects semicolon from the shell |


### 5.11 Limit search depth
```bash
find /home -maxdepth 2 -name "*.txt"
find . -maxdepth 1 -type f
```
-maxdepth controls how many directory levels find enters.

### 5.12 Search from current directory
```bash
find . -name "report.pdf"
find . -type f -name "*.sh"
```

## 6. Fast Searching with locate
locate searches using a prebuilt database, so it is usually faster than find. It does not search live filesystem changes unless the database is updated.

### Basic syntax
```bash
locate [options] pattern
```

### Useful examples
```bash
locate file.txt           # search by name
locate -c file.txt        # count matches
locate -n 10 file         # limit output to 10 results
locate -b "\file.txt"    # match only the basename
locate -r "file.*txt"     # regex search
sudo updatedb             # update locate database
```
| Option | Meaning |
| --- | --- |
| -c | Count matches instead of listing all results |
| -n | Limit number of results |
| -b | Match only file basename |
| -r | Use regular expression search |

> [!TIP] Sticky Note
> Use locate for speed. Use find when you need live, accurate, filtered searching.


## 7. whereis and which

### whereis - find binary, source, and manual pages
whereis tells where a command binary, source code, and manual pages are located. It is not meant for searching normal user files.
```bash
whereis ls
whereis bash
whereis python3
```
| Option | Meaning | Example |
| --- | --- | --- |
| -b | Show only binary location | whereis -b ls |
| -m | Show only manual page location | whereis -m ls |
| -s | Show only source location | whereis -s gcc |
| -B ... -f | Limit binary search paths | whereis -B /bin /usr/bin -f ls |
| -M ... -f | Limit manual search paths | whereis -M /usr/share/man -f ls |
| -S ... -f | Limit source search paths | whereis -S /usr/src -f kernel |


### which - show the executable from PATH
which shows the executable file that will run when you type a command.
```bash
which bash
which python3
which gcc
command -v python3
```
> [!TIP] Sticky Note
> PATH is the list of directories where Linux searches for commands. which checks that path.


## 8. Piping and Filtering with grep

### Pipe concept
Piping connects the output of one command directly into the input of another command. The pipe symbol is |.
```bash
command1 | command2
```
> [!TIP] Sticky Note
> Think of a pipe like water flow: output from the first command flows into the second command.


### grep concept
grep searches for patterns in text. It scans text and shows only matching lines.
```bash
grep "keyword" filename
ls -l | grep ".txt"
ps aux | grep "apache2"
```
| grep option | Meaning | Example |
| --- | --- | --- |
| -i | Ignore case | grep -i "error" app.log |
| -n | Show line numbers | grep -n "main" script.py |
| -r | Recursive search in directories | grep -r "TODO" . |
| -v | Invert match, show lines that do not match | grep -v "success" log.txt |
| -c | Count matching lines | grep -c "failed" auth.log |
| -l | Show filenames that contain matches | grep -l "root" *.txt |


### Practical examples
```bash
ls -l | grep ".txt"              # filter text files from long listing
ps aux | grep "apache2"          # check apache process
cat /var/log/auth.log | grep "failed"
grep -r "password" .             # search current folder recursively
```
> [!WARNING] Careful
> When using grep -r on system folders, you may get permission errors. That is normal unless you run with sudo.


## 9. Creating Files
Linux provides many ways to create files. The best method depends on whether you want an empty file, a file with simple content, or a file edited manually.

### 9.1 touch - create an empty file
```bash
touch filename.txt
touch notes.txt
```

### 9.2 Redirect output into a file
```bash
echo "Hello, world!" > filename.txt
echo "Second line" >> filename.txt
```
| Operator | Meaning |
| --- | --- |
| > | Overwrite the file or create it if it does not exist |
| >> | Append to the file without deleting old content |

> [!WARNING] Careful
> > overwrites existing content. Use >> when you want to add content to the end.


### 9.3 cat - type content into a file
```bash
cat > filename.txt
# Type your content
# Press Ctrl + D to save and exit
```

### 9.4 Text editors
```bash
nano filename.txt
vim filename.txt
gedit filename.txt
```
| Editor | Beginner use |
| --- | --- |
| nano | Easy terminal editor for beginners |
| vim | Powerful terminal editor; needs practice |
| gedit | Graphical text editor on desktop Linux |


### 9.5 tee - write and display output
```bash
echo "Hello" | tee filename.txt
echo "Another line" | tee -a filename.txt
```
tee writes output to a file and also displays it on the screen. Use -a to append.

### 9.6 Here document - write multiple lines
```bash
cat > filename.txt << EOF
Line one
Line two
Line three
EOF
```

### 9.7 Create file using Python scripting
```bash
python3 -c "open('file.txt', 'w').write('Hello\n')"
```
> [!TIP] Sticky Note
> For automation, scripting methods like Python and Bash are very useful.


## 10. Creating Directories with mkdir

### Basic syntax
```bash
mkdir [options] directory-name
```

### Usage examples
```bash
mkdir mydir                         # create one directory
mkdir dir1 dir2 dir3                 # create multiple directories
mkdir -p a/b/c                       # create nested directories
mkdir -m 755 mydir                   # create with permissions
mkdir -v mydir                       # verbose output
mkdir -pv /tmp/a/b/c                 # nested + show each created directory
```

### Project folder example
```bash
mkdir -p project/{src,bin,docs,tests}
```
This creates a project folder with src, bin, docs, and tests subdirectories.

### mkdir options
| Option | Meaning |
| --- | --- |
| -p | Parents: no error if directory exists; create parent directories as needed |
| -m | Mode: set permission bits while creating |
| -v | Verbose: print message for each created directory |
| -Z | Set SELinux security context where supported |

> [!TIP] Sticky Note
> Use mkdir -p for nested directories. It prevents errors when parent folders do not exist.


## 11. Removing Files and Directories

### rmdir - remove empty directories
rmdir removes only empty directories. It will not remove directories that contain files.
```bash
rmdir mydir
rmdir dir1 dir2 dir3
rmdir -p a/b/c
rmdir -v mydir
```
| rmdir option | Meaning |
| --- | --- |
| -p | Remove directory and its empty parent directories |
| -v | Verbose output for every directory processed |


### rm - remove files or non-empty directories
```bash
rm file.txt
rm -r mydir       # recursively remove directory and contents
rm -rf mydir      # force delete without prompts
rm -ri mydir      # interactive prompt before each removal
rm -rv mydir      # verbose recursive removal
rm -d emptydir    # remove empty directory
```
| rm option | Meaning |
| --- | --- |
| -r or -R | Recursive: remove directory and its contents |
| -f | Force: ignore missing files and never prompt |
| -i | Interactive: ask before each removal |
| -v | Verbose: show what is being removed |
| -d | Remove empty directories |

> [!WARNING] Careful
> rm -rf is powerful and dangerous. A wrong path can delete important files quickly. Prefer rm -ri while learning.


## 12. Moving and Renaming with mv

### Basic syntax
```bash
mv [options] source destination
```

### Usage examples
```bash
mv dir1 dir2                         # rename dir1 to dir2 if dir2 does not exist
mv dir1 /path/to/dir2                # move dir1 into an existing location
mv dir1 dir2/                        # move dir1 inside dir2
mv -i dir1 dir2                      # ask before overwriting
mv -n dir1 dir2                      # do not overwrite existing destination
mv -v dir1 dir2                      # show what is being moved
mv dir1 dir2 dir3 target/            # move multiple directories into target
```

### mv options
| Option | Meaning |
| --- | --- |
| -i | Interactive: prompt before overwrite |
| -n | No-clobber: do not overwrite existing file |
| -f | Force: overwrite without prompt |
| -v | Verbose: explain what is being done |
| -u | Update: move only when source is newer than destination |

> [!TIP] Sticky Note
> mv is used for both moving and renaming. If destination is a new name, it renames. If destination is an existing directory, it moves into that directory.


## 13. Copying Files and Directories with cp

### Basic syntax
```bash
cp [options] source destination

# Multiple sources
cp [options] source1 source2 source3 directory/
```

### Simple file copy examples
```bash
cp file1.txt file2.txt                       # copy file to a new name
cp a.txt /home/Downloads/practice/            # copy file into another folder
cp a.txt b.txt /home/Documents/               # copy multiple files into a folder
```

### Copy directories
```bash
cp -r newdir backupdir                    # copy a directory recursively
cp -R dir1 dir2                            # same idea: recursive directory copy
```
> [!TIP] Sticky Note
> To copy a directory, you must use -r or -R. Without it, cp will usually refuse to copy directories.


### Common cp flags
| Flag | Full form / meaning | Example |
| --- | --- | --- |
| -r / -R | Recursive: copy folders and their contents | cp -r dir backupdir |
| -i | Interactive: ask before overwrite | cp -i a.txt b.txt |
| -f | Force: overwrite destination | cp -f a.txt b.txt |
| -v | Verbose: show what is being copied | cp -v a.txt b.txt |
| -u | Update: copy only if source is newer | cp -u c.txt /backup/ |
| -a | Archive: preserve structure and attributes as much as possible | cp -a folder folder_backup |
| -p | Preserve timestamps, ownership, and permissions when possible | cp -p a.txt b.txt |
| -n | No-clobber: do not overwrite existing destination | cp -n a.txt b.txt |
| -l | Create hard links instead of copying file data | cp -l file1.txt file2.txt |
| -s | Create symbolic links instead of copying file data | cp -s a.txt link_to_a.txt |
| --backup | Create a backup of destination before overwriting | cp --backup file.txt backup/ |
| --parents | Preserve full source path inside destination | cp --parents /home/sujith/docs/file.txt /backup/ |
| -t | Specify target directory first, then list sources | cp -t backup/ file1.txt file2.txt |
| -T | Treat destination as a normal file, not a directory | cp -T source.txt dest.txt |


### Backup-style examples
```bash
cp -a folder folder_backup
cp -u *.txt /backup/
cp --backup=numbered file.txt backup/
cp --parents /home/sujith/docs/file.txt /backup/
```
> [!WARNING] Careful
> Be careful with cp -f because it overwrites. While learning, use cp -i for protection.


## 14. Quick Revision Cheat Sheet
| Task | Command |
| --- | --- |
| Show current directory | pwd |
| Go to home directory | cd ~ |
| Go one level back | cd .. |
| List all files in long format | ls -la |
| Get command help | command --help or man command |
| Search live by name | find /home -name "file.txt" |
| Search case-insensitive | find /home -iname "file.txt" |
| Find files only | find /home -type f |
| Find directories only | find /home -type d |
| Find large files | find /home -size +100M |
| Find modified recently | find /home -mtime -7 |
| Fast database search | locate file.txt |
| Update locate DB | sudo updatedb |
| Find command binary | which python3 |
| Find binary/manual/source | whereis ls |
| Filter output | command | grep "keyword" |
| Create empty file | touch file.txt |
| Write text to file | echo "hello" > file.txt |
| Append text to file | echo "hello" >> file.txt |
| Create directory | mkdir mydir |
| Create nested directories | mkdir -p a/b/c |
| Remove empty directory | rmdir mydir |
| Remove non-empty directory | rm -r mydir |
| Rename file/directory | mv oldname newname |
| Copy file | cp a.txt b.txt |
| Copy directory | cp -r dir backupdir |


## 15. Beginner Practice Lab
Practice these commands in a safe test directory. Do not use system folders for practice.
```bash
mkdir -p ~/chapter1-lab/{notes,logs,backup}
cd ~/chapter1-lab
pwd
ls -la
```

### Create files
```bash
touch notes/linux.txt
echo "Linux basics" > notes/linux.txt
echo "grep practice line" >> notes/linux.txt
cat notes/linux.txt
```

### Find files
```bash
find . -name "*.txt"
find . -type d
find . -type f -size -1M
```

### Copy and move
```bash
cp notes/linux.txt backup/linux-copy.txt
mv backup/linux-copy.txt backup/linux-backup.txt
ls -l backup
```

### Use grep and pipe
```bash
ls -l notes | grep "linux"
grep -n "Linux" notes/linux.txt
```

### Clean up safely
```bash
cd ~
rm -ri chapter1-lab
```
> [!WARNING] Careful
> Use rm -ri during practice so Linux asks before removing each file/directory.


## 16. Memory Sticky Notes
- pwd = Where am I?
- cd = Move to another folder.
- ls = What is inside this folder?
- find = Search live with filters.
- locate = Search fast using database.
- whereis = Where are binary/source/manual files?
- which = Which executable will run?
- grep = Show only matching lines.
- pipe | = Send output of one command into another.
- touch = Create empty file.
- mkdir -p = Create nested folders safely.
- cp -r = Copy directories.
- mv = Move or rename.
- rm -ri = Safer delete while learning.
> [!TIP] Sticky Note
> The fastest way to learn Linux is not memorizing everything. Practice small commands daily, read the output, break things only inside test folders, and repeat.

