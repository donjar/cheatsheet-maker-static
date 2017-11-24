---
title: donjar-CS2106
description: Introduction to Operating Systems
---

![stack](img/cs2106/stack.png "Stack")
![phases](img/cs2106/phases.png "Phases")
![processtable](img/cs2106/table.png "Process Table")
![processlifecycle](img/cs2106/lifecycle.png "Process Life Cycle")

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

## C Libraries
0: false, rest: true

Libraries
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
