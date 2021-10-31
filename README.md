# Report
This document describes the implementation of the specifications as mentioned in the assignment.

## Specification 1: syscall tracing
- Added $U/_strace to UPROGS in Makefile
- Added `sys_trace()` in kernel/sysproc.c that reads the syscall argument and calls the
`trace()` defined in proc.c with the argument
- `trace()` sets the `mask` field of the process struct
- Modified `fork()` to copy `mask` from parent to child
- Modified `syscall()` to print syscall info if the syscall is traced
- Created user program `strace` in user/strace.c that calls the trace system call to set the mask of the process and run the function passed.

## Specification 2: scheduling
- Modified Makefile to take argument which then defines a macro with the compiler to identify the scheduling algorithm

### FCFS Policy
- Edited `struct proc` to store the time it was created
- Edited `allocproc()` to initialise the new variable created above
- Edited `scheduler()` to run the process with the lowest time created
- Edited `kerneltrap()` in kernel/trap.c to disable premption with timer interrupts

### PBS Policy
- Edited `struct proc` to store the priority, time dispatched, runtime during allocated time, and time when it ready to run
- Edited `allocproc()` to initialise the new variables created above
- Edited `scheduler()` to run the process with the highest priority
- Edited `clockintr()` to track runtime and wait time
- Added a new sycall `set_priority` to change the priority of a process

### MLFQ Policy
- Edited `struct proc` to store the priority, allocated time, times dispatched, time added to queue, and time spent in each queue
- Edited `allocproc()` to initialise the new variables created above
- Created 5 queues of different priority
- Edited `scheduler()` to run the process with the highest priority and handle aging of processes
- Edited `clockintr()` to track runtime and
- Edited `kerneltrap()` and `usertrap()` to yield when process has exhausted its time slice



## Specification 3: procdump
- Edited `procdump()` in kernel/proc.c to print data from the process struct

## Additional Implementations
- `waitx()` syscall is implemented that funtions similar to `wait()` but also returns the runtime and wait time of the child process

## Specification 4: 
- Answer to question in specification 2: A malicious process can exploit the given condition by yielding the CPU before finishing its allocated time, retaining its priority and blocking lower priority processes from running unless aging is implemented.