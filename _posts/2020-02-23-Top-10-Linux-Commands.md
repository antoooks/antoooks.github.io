---
layout: post
title: Top 10 Linux Commands
---

You maybe often heard or use about these linux commands as they do basic operations such as `mkdir`,`cat`,`ls`, `rm`. But there are broader variants that usually appear on online assessment of job interview. Today I will compile top 10 linux commands that may appear on test and what they do.

## 1. `head` and `tail`

use `head` to print first 10 lines of a file, specify how many lines you want to show with `-n`. e.g. `cat file.txt | head -n 5`

use `tail` to print last 10 lines of a file, specify how many lines you want to show with `-n` e.g. `cat file.txt | tail -n 10`. You can also follow the bottom of a file with `-f` argument, if the file keeps adding lines.

```
You CANNOT get lines information or file size with this command.
```

## 2. `top`

`top` display CPU and memory usage of processes running at the moment.

You can also retrieve other information such as free memory, used memory, how many user sessions, how many process, syetem time, and system uptime.

RAM information displayed after `KiB Mem` field, `KiB Swap` IS NOT RAM memory it is a hard disk memory which treated like RAM as temporary memory. The 

More on `top` command: [here](https://www.booleanworld.com/guide-linux-top-command/)

```
You CAN see CPU and RAM usage with this command.
```

## 3. `netstat`

`netstat` will display network information like:
- ports in use and its status
- which connection coming in
- which process use that port

```
You CANNOT see CPU and RAM usage with this command or total port of a system.
```

## 4. lsof

`lsof` list open files in your system and which processes use it. An open files could be a regular file, a directory, a block special file, a character special file, an executing  text reference, a library, a stream or a network file (Internet socket, NFS file or UNIX domain socket.)

```
You CAN see port and pid information with this command.
```

## 5. Preserving Environment Variable Across Session

You can preserve environment variable across terminal session such as:
`EXPORT token='123456abc'`, by doing so:

- Adding the export statement to `$HOME/.bashrc` or `$HOME/.bash_profile` if you use Bash terminal.
- Adding the export statement to `/etc/.profile`. You have to know that `.profile` is not guaranteed to read, it depends on how your terminal session start. 
- Writing the env vars to a file and then source the file on start command. e.g. `source ~/myenv.txt`, yes you can put it on .txt file or .sh because the bash terminal will read the content as input anyway.

## 6. `wc`, `grep`, and `cat` to count number of lines

- `wc` - display number of lines, character count, and byte count
- `grep` - search files or file content with user defined pattern
- `cat` - concat 2 files and print the content

You can use these commands to count how many new line inside a file.

- `wc -l file.txt`
- `grep "" -c file.txt`
- `cat -n file.txt`

```
You can use wc to view size of file using wc -c
```

## 7. `stat`

- `stat` - display information of a file. This command will display file name, file size in bytes, permission of the file, inode number, last modified date and time, etc.  

```
You can use this command to view size of file and inode number which represents it.
```

## 8. `du` and `df`

- `du` - display disk usage statistic, it also display how much a file used the disk space in human readable format and which file.
- `df` - display free disk space statistic, it displays total size of disk, how much is used, available space, used inode, free inode, and mount point.

```
You CANNOT get CPU or RAM usage with this command, but you can get DISK usage.
```

## 9. `vmstat` and `free`

- `vmstat` reports  information about processes, memory, paging, block IO, traps, disks and cpu activity

- `free` reports memory usage in details, including used memory, free memory, shared memory, memory used for buffer, and memory used for cache.

```
You CAN get CPU Usage, RAM, and DISK information with this command.
```

## 10. `dig`

`dig` command is used to lookup a DNS name to DNS resolver. DNS resolver will answer with IP address and other information of the DNS. The information provided are DNS name, TTL, resource CLASS (usually IN for internet), record type, and IP address or other DNS.

# Closing

Note:

`top` and `lsof` is the most difficult command to understand because it has many features build inside.

Happy reading, if you have any suggestion on the next article please let me know! :)

<hr>

23 February 2020