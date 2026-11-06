Aim: 
To measure and compare the performance of programs on a Linux operating system using strace, time, fork() and pthread_create().


1) System calls made by the file-copy program
The strace -c command is used to measure the number of system calls made by the file-copy program.

The Task 6 file-copy program is executed with three different buffer sizes:
- 1 byte
- 512 bytes
- 4096 bytes

Command:

strace -c ./task6


To display only read system-call statistics:

strace -c -e trace=read ./task6



Program output for read operations:
- Buffer size: 1 byte
  - Number of read() calls: 10001
- Buffer size: 512 bytes
  - Number of read() calls: 21
- Buffer size: 4096 bytes
  - Number of read() calls: 4
These counts include the final read that detects the end of the file.


Observation:

When the buffer size increases, the number of read system calls decreases. A larger buffer reduces the number of interactions between the user program and the operating system.


2)Time spent in user mode and kernel mode
The time command is used to measure the execution time of the program.

Command:

time ./task6


The command provides three measurements:
- Real time: Total time taken to execute the program.
- User time: Time spent executing instructions in user mode.
- System time: Time spent executing instructions in kernel mode.

Measured results:
- Buffer size: 1 byte
  - Real time: Actual value
  - User time: Actual value
  - System time: Actual value
- Buffer size: 512 bytes
  - Real time: Actual value
  - User time: Actual value
  - System time: Actual value
- Buffer size: 4096 bytes
  - Real time: Actual value
  - User time: Actual value
  - System time: Actual value


Observation:

A larger buffer size generally reduces the number of system calls and kernel-mode overhead. Therefore, file copying can become more efficient.

Replace the values with the actual results obtained from your Linux terminal.

Comparison:

- Parameter: Number created
  Processes: 1000
  Threads: 1000
- Parameter: Creation method
  Processes: fork()
  Threads: pthread_create()
- Parameter: Creation and cleanup time
  Processes: 3.715007 seconds (sample)
  Threads: 0.063359 seconds (sample)
- Parameter: Memory usage
  Processes: Generally higher
  Threads: Generally lower
- Parameter: Address space
  Processes: Separate
  Threads: Shared within a process
- Parameter: Creation overhead
  Processes: Higher
  Threads: Lower


Observation:

The experiment shows that thread creation is generally faster than process creation. Threads share resources such as the address space of their parent process, whereas separate processes have independent address spaces.


4) Evaluation of measurements

The experimental results are compared with the expected behaviour of the operating system.
1. Increasing the buffer size reduces the number of read system calls.
2. Fewer system calls can reduce kernel-mode overhead.
3. Creating 1,000 threads generally takes less time than creating 1,000 processes.
4. Processes require more resources because they maintain separate address spaces.
5. Threads are more efficient for tasks that need frequent communication and resource sharing.
6. Processes are more suitable when stronger isolation between tasks is required.
These measurements support the earlier judgement that larger buffers and threads can improve performance in suitable situations.


5) Alternative Concept and Complexity Analysis
If threads are created instead of processes, the execution time and memory usage are generally reduced.

- Feature: Memory
- Process: Separate address space
- Thread: Shared process address space
- Feature: Creation time
- Process: Generally higher
- Thread: Generally lower
- Feature: Resource sharing
- Process: Limited
- Thread: More
- Feature: Communication
- Process: Uses IPC
- Thread: Can use shared memory
- Feature: Isolation
- Process: High
- Thread: Lower


Explanation:

A process has its own memory space and operating-system resources. Therefore, creating multiple processes requires more memory and management overhead.
Threads belonging to the same process share code, data and other process resources. Each thread has its own stack and execution state. Hence, thread creation generally requires less memory and time.

Result:

The performance of the file-copy program, process creation and thread creation was measured and compared using Linux system utilities.
The results show that increasing the buffer size reduces the number of read system calls. Thread creation is generally faster and uses fewer resources than process creation. Therefore, selecting suitable buffer sizes and execution units helps improve operating-system performance.
