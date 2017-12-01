---
title: CS2106 by donjar (Page 1)
description: Introduction to Operating Systems
---

![stack](/img/cs2106/stack.png "Stack")
![phases](/img/cs2106/phases.png "Phases")
![processtable](/img/cs2106/table.png "Process Table")
![processlifecycle](/img/cs2106/lifecycle.png "Process Life Cycle")

## Processing
- Batch (no user)
- Interactive (responsive)
- Real time

### Batch
- First Come First Served: put in queue, guarantee no starvation
- Shortest Job First: guarantees smallest average waiting time
  - Need total CPU time in advance
- Shortest Remaining Time Next: good service for shortest jobs

### Interactive
- Round Robin: queue, turn around every time quantum passes / blocked / give up
- Priority-based: get lowest priority every time interval (can starve)
- Multi-level Feedback Queue: priority A > B then A runs, if A = B run in round robin. Job fully utilize time -> decrement priority
- Lottery Scheduling: draw lots every interrupt

## Process vs Thread
- Thread is light
- Thread share code/**data**/files
- **User** (library) vs **kernel** (OS scheduling)
- Process context switch: OS, hardware, memory
- Thread context switch: hardware (registers, stack)

## Critical Section
- Mutual Exclusion
- Progress: if no process in CS, one should be granted access
- Bounded Wait: cannot be stalled
- Independence: process not in CS should not block other processes

- Deadlock: no progress
- Livelock: deadlock avoidance, no progress
- Starvation: some process stalled forever

## Peterson
```
Want[0] = 1
Turn = 1
while (want[1] && Turn == 1);
// CS
Want[0] == 0
//////
Want[1] = 1
Turn = 0
while (want[0] && Turn == 0);
// CS
Want[1] == 0
```

## Semaphore
- Wait/Decrement/Down/P and Signal/Increment/Up/V
- If S <= 0, blocks. Block then decrement

## Producer-Consumer
Producer produce items if buffer not full, consumer remove items if buffer not empty

Producer:
```
wait(notFull)
wait(mutex)
// ...
signal(mutex)
signal(notEmpty)
```

Consumer:
```
wait(notEmpty)
wait(mutex)
// ...
signal(mutex)
signal(notFull)
```

## Readers-Writers
Writes wrote alone, readers read together

Writer:
```
wait(roomEmpty);
// Modifies data
signal(roomEmpty);
```

Reader:
```
wait(mutex);
nReader++;
if (nReader == 1) signal(mutex);
// Reads data
wait(mutex);
nReader--;
if (nReader == 0) signal(roomEmpty);
signal(mutex);
```

## Dining Philosophers
Tanenbaum, Limited Eater

## Memory Management

### Fixed Allocation
![fixedmem](/img/cs2106/fixedmem.png "Fixed Memory")

### Dynamic Allocation
![dynamicmem](/img/cs2106/dynamicmem.png "Dynamic Memory")

### Buddy Allocation
![buddy](/img/cs2106/buddy.png "Buddy Memory")

### Page Tables
![pagetables](/img/cs2106/pagetables.png "Page Tables")

### TLB
![tlb](/img/cs2106/tlb.png "TLB")

### Accessing Page X
- Check page table: if page X valid, access
- Else: page fault, trap to OS
- Locate page X in secondary storage
- Load page X into a physical memory frame
- Update page table
- Retry

### Page Replacement Algorithms
- First In First Out
- Least Recently Used
- CLOCK: if accessed, give "second chance"

### Frame Allocation
Working Set Model: how many frames is used in the last N accesses?

## Files
- Sequential: read in order, cannot skip
- Random access: `read` or `seek`
- Direct access: file for fixed-length records; allow random access

![fileunix](/img/cs2106/fileunix.png "File Operations in Unix")

### Directories
- Single-level (root + files)
- Trees
- DAG (with links)
- General graph with cycles

### Links
- Hard link (file only, separate pointers)
- Symbolic link (link to path name)
