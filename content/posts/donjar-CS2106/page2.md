---
title: CS2106 by donjar (Page 2)
description: Introduction to Operating Systems
---

## Disk Structure
- Array of logical blocks
- Logical block is the smallest accessible unit, usually 512 bytes to 4 Kilobytes
- Logical block is mapped into disk sectors. Layout of disk sector is hardware dependent

## Disk Organization
![diskorg](/img/cs2106/diskorg.png "Disk Organization")

## File Block Allocation
- Contiguous
- Linked List
- Linked List with File Allocation Table: move all block pointers into a single table
- Indexed Allocation: each file has an index block, which is an array of disk blocks

### Free Space Management
- Bitmap: each disk block is represented by 1 bit
- Linked list: linked list of disk blocks

### Directory Structure
- Linear List
- Hash Table of files

## Create File
- Use full path name to locate directory
- Use free space list to find free disk blocks
- Add an entry to parent directory

## Open File
- Search system-wide table for existing entry
- If not found, use full path name to locate file
- Add file to system-wide table and return a pointer to this entry

## Disk I/O Scheduling
![magneticdisk](/img/cs2106/magneticdisk.png "Magnetic Disk")
Time taken = seek time + rotational latency + transfer time

### Algorithms
- First Come First Served
- Shortest Seek First
- SCAN (Elevator): imagine tracks are floors in a building, disk head is elevator servicing the floors

## FAT
FAT entry:

- FREE
- Block number of next blocks
- EOF
- BAD

![fatdir](/img/cs2106/fatdir.png "FAT Directory Structure")
![fatexample](/img/cs2106/fatexample.png "FAT Example")

### Delete
- Delete the directory entry by setting the first letter of the filename into `0xE5`
- Free data blocks in FAT
- Can undelete

### Free Space Management
- Need to go through FAT

![fatlong](/img/cs2106/fatlong.png "FAT Long File Name")

## Ext2
![ext2](/img/cs2106/ext2.png "Ext2")

### Partition Info
- Superblock
  - describes the whole file system
    - Total I-Nodes number, I-Nodes per group
    - Total disk blocks, Disk Blocks per group
    - etc
  - Duplicated in each block group for redundancy
- Group Descriptors
  - Describe each of the block group
    - Number of free disk blocks, free I-nodes
    - Location of the bitmaps
  - Duplicated in each block group as well
- Block Bitmap: Keep track of the usage status of blocks of this block group (1 is occupied, 0 is free)
- I-Node Bitmap: Keep track of the usage status of I-Nodes of this block group (1 is occupied, 0 is free)
- I-Node table
  - An array of I-Nodes
    - Each I-Node can be access by an unique index
  - Contains only I-Nodes of this block group

![ext2inode](/img/cs2106/ext2inode.png "Ext2 I-Node Structure")

### I-Node Data Blocks
- 15 disk block pointers (disk block numbers):
  - 12 to disk blocks that stores actual data (direct blocks)
  - 13th pointer points to a disk block that stores direct pointers (single indirect block)
  - 14th pointer points to a disk block that contains a number of single indirect blocks (double indirect block)
  - 15th pointer points to a disk block that contains a number of double indirect blocks (triple indirect block)

![dirstructure](/img/cs2106/dirstructure.png "Directory Structure")
![filemetadata](/img/cs2106/filemetadata.png "Ext2 File Metadata")

## C Libraries
0: false, rest: true

Default FDs: 0 for STDIN, 1 for STDOUT, 2 for STDERR

Functions:

- `int fork()`: parent is child, child: 0
- `int setpid()`: my PID
- `int getppid()`: my parent's PID
- `int execl(const char *path, const char *arg0, ..., const char *argN, NULL)` e.g. `execl("/bin/ls", "ls", "-la", NULL)`
- `void exit(int status)`: no return, end execution
- `int wait(int *status)`: return PID, `*status` is exit status. Waits for child, blocks
- `pthread_create(pthread_t* tidCreated, const pthread_attr_t* threadAttributes, void* (*starRoutine), void* argForStartRoutine)`
- Message Passing
  - `send(process2, message)` and `receive(process1, message)`
  - `send(mailbox, message)` and `receive(mailbox, message)`
- Unix pipes: `int pipe(int fd[])`. `fd[0]` is reading end and `fd[1]` is writing end
- Signalling: `void signal(int sig_num, void* handler)` e.g. `signal(SIGINT, handler)`
- `int pthread_exit(void* exitValue)`, the `exitValue` is the return value
- `int open(char* path, int flags)`, return the file descriptor
- `int read(int fd, void* buf, int n)` reads n bytes from current offset at fd into buf
- `int write(int fd, void* buf, int n)`
- `off_t lseek(int fd, off_t offset, int whence)` moves current position in file by `offset`
- `int close(int fd)` return 0 if success, -1 if error
