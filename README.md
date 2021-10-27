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
- Edited `clockintr()` to track runtime
- Added a new sycall `set_priority` to change the priority of a process
- Edited `wakeup()`, `yield()`, `kill()` to record when process finished running/waiting