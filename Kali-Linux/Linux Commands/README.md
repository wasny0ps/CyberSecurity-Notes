# Linux Commands

## File System Commands

| Command | Param            | Description                                       |
| ------- | ---------------- | ------------------------------------------------- |
| `cd`    | `-`              | Navigate to last dir                              |
|         | `~`              | Navigate to home                                  |
|         | `~username`      | Navigate to home of specified user                |
| `pwd`   |                  | Print working dir                                 |
| `ls`    |                  | Print dir content                                 |
|         | `-l`             | Format as list                                    |
|         | `-a`             | Show hidden items (`-A` without `.` and `..`)     |
|         | `-r`             | Invert order                                      |
|         | `-R`             | Recurse                                           |
|         | `-S`             | Sort by size                                      |
|         | `-t`             | Sort by date modified                             |
| `mkdir` | `-p`             | Create dir with parents                           |
| `cp`    | `-r`             | Copy dir                                          |
| `rmdir` | `-p`             | Remove dir and empty parents                      |
| `rm`    | `-rf`            | Remove dir recursively, `-f` without confirmation |
| `mv`    |                  | Move recursively                                  |
| `find`  | `-iname pattern` | Search dir/file case-insensitive                  |
|         | `-mmin n`        | Last modified n minutes ago                       |
|         | `-mtime n`       | Last modified n days ago                          |
|         | `-regex pattern` | Path matches pattern                              |
|         | `-size n[kMG]`   | By file size (`-n` less than; `+n` greater than)  |
|         | `! searchparams` | Invert search                                     |
| `grep`  | `pattern file`    | Extended Regex |
|         | `-E pattern file` | Extended Regex |
|         | `-v pattern file` | Invert match   |
|         | `-w pattern file` | Word match     |
|         | `-i pattern file` | Ignore case    |
|`cal`||Show calendar|
|`date`|`--date="next friday"`|Show system time for next friday|
||`+%D`|Show current time in mounth/day/year format|
|`dig`||Retrieving information about DNS name servers|
|`wget`|`http://x.com/hello.png`|Download the file from the server even when the user has not logged on to the system |
|`tree`|`-a`|Show all directory and files in tree format|
||`-d`| Only show list directories |
|`ps`|`-aux`|Monitor processes running on Linux system|
|`top`||Displaying system-performance information with PIDs|


## File Manipulation

| Command | Param                                      | Description                                |
| ------- | ------------------------------------------ | ------------------------------------------ |
| `cat`   | `file`                                     | Print content                              |
| `tac`   | `file`                                     | Print content inverted                     |
| `sort`  | `file`                                     | Print sorted                               |
|         | `file -r -u`                               | Print sorted descending without dublicates |
| `wc`    | `file`                                     | Count Lines, Words, Chars (Bytes)          |
| `head`  | `-n10 file | tail -n5`                     | Print lines 5-10                           |
| `tail`  | `-f file`                                  | Print new lines automatically              |
| `cut`   | `-f -4,7-10,12,15- file`                   | Print selected fields (tab delimited)      |
|         | `-c -4,7-10,12,15- file`                   | Print selected characters positions        |
|         | `-f 2,4 -d, --output-delimiter=$'\t' file` | Change delimiter (but use tab for output)  |
| `uniq`  | `file`                                     | Hide consecutive identical lines           |
|         | `file -c`                                  | Show consecutive identical line count      |
|         | `file -u`                                  | Hide consecutive identical lines           |
| `file`  | `file`                                     | Get file type                              |
|`rev`|`file`|Reverse the lines characterwise|

## Other

| Command     | Param           | Description                               |
| ----------- | --------------- | ------------------------------------------ |
| `<command>` | `--help`        | Help of current command (not standardized) |
|             | `-h`            |                                            |
|             | `-?`            |                                            |
| `man`       | `<command>`     | Manual page of command                     |
|             | `-k keyword`    | Search command by keyword (oder `apropos`) |
| `alias`     |                 | Show aliases                               |
|             | `name='befehl'` | Create alias                               |
|`tr`|`echo Hello  tr 'e' 'a'`|Translates or deletes characters from standard input **(stdin)** and writes the result to standard output **(stdout)**|

## Stream redirection
- `>` overwrite
- `>>` append

| Character             | Description                     |
| --------------------- | ------------------------------- |
| `> file` or `1> file` | STDOUT to file                  |
| `< file`              | Datei to STDIN                  |
| `2> file`             | STDERR to file                  |
| `2>&1`                | STDERR to same target as STDOUT |
| `> file 2>&1`         | STDOUT and STDERR to file       |


# File System Permissions
Permissions can be set on:
- User (owner)
- Group (owner)
- Others

Only root can change *User*. *User* can change *Group*.

Basic permissions (Add binary flags to combine):

| Char | Binary Flag | Permission |
| ---- | ----------- | ---------- |
| r    | 4           | read       |
| w    | 2           | write      |
| x    | 1           | execute    |

Advanced permissions (place in front of basic permissions: `chmod 1777 shared`).:

| Char  | Binary Flag | Name       | Description                                                                |
| ----- | ----------- | ---------- | -------------------------------------------------------------------------- |
| t / T | 1           | Sticky Bit | *Others* can't delete content (only applicable for directories)            |
| s / S | 2           | SGID-Bit   | File: run with permissions of *Group*<br>Dir: New elements inherit *Group* |
| s / S | 4           | SUID-Bit   | File is run with permissions of *User* (only applicable for files)         |

Advanced permissions replace the **x** when using `ls -l`. Lower case if **x** is set, upper case if **x** is not set.

*Read* permission on a directory only allows to see the directory itself but not it's contents. Use *execute* permission to show contents.

**Commands**

| Command   | Param                    | Beschreibung                                      |
| --------- | ------------------------ | ------------------------------------------------- |
| `chmod`   | `-R [uog] dirname`       | Set permissions recursively using binary flags    |
|           | `+[suog] filename`       | Add permissions using binary flags                |
|           | `-[suog] filename`       | Remove permissions using binary flags             |
|           | `u+x filename`           | Add *execute* permission for *User*               |
|           | `g+wx filename`          | Add *write* and *execute* permissions for *Group* |
|           | `o-r filename`           | Remove *read* permission for *Others*             |
| `chown`   | `-R user:group filename` | Change owner (*User* & *Group*) recursively       |
|           | `user filename`          | Change owner (*User*)                             |
|           | `:group filename`        | Change owner (*Group*)                            |
| `chgroup` | `group filename`         | Change owner (*Group*)                            |
