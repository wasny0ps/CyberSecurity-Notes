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

## Other

| Command     | Param           | Beschreibung                               |
| ----------- | --------------- | ------------------------------------------ |
| `<command>` | `--help`        | Help of current command (not standardized) |
|             | `-h`            |                                            |
|             | `-?`            |                                            |
| `man`       | `<command>`     | Manual page of command                     |
|             | `-k keyword`    | Search command by keyword (oder `apropos`) |
| `alias`     |                 | Show aliases                               |
|             | `name='befehl'` | Create alias                               |
