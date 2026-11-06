Aim:
To compare file-copying performance using different buffer sizes and evaluate the number of read() system calls.


Explanation:

1. The program creates an input file containing 10,000 bytes.
2. It copies the file using three different buffer sizes: 1, 512 and 4096 bytes.
3. A 1-byte buffer requires 10,000 successful read calls and one additional call to detect the end of the file.
4. A 512-byte buffer requires 20 successful read calls and one additional call.
5. A 4096-byte buffer requires 3 successful read calls and one additional call.
6. Larger buffer sizes reduce the number of system calls and improve file-copying efficiency.


Run Using strace,

strace -c -e trace=read ./task6

This command displays a summary of the read() system calls made by the program.


Observation:

The number of read() calls decreases as the buffer size increases. The program's printed counts specifically measure reads from the input file; the strace summary may also include other reads made by the process.


Alternative Concept: mmap()

mmap() is an alternative method for accessing file contents.
- Buffered I/O:
  - Uses read() and write() system calls.
  - Data is copied through a user-space buffer.
  - Easy to implement.
  - Suitable for simple file copying.
- Memory Mapping (mmap):
  - Maps file contents into virtual memory.
  - File contents can be accessed through memory addresses.
  - Requires memory mapping and synchronization management.
  - Useful for accessing and processing large files.


Comparison:
- Buffered I/O is simple and suitable for normal file-copying operations.
- mmap() can reduce explicit read calls but still involves page faults and memory-management overhead.
- The best method depends on file size and workload.


Conclusion

Thus, file copying was successfully performed using different buffer sizes. The experiment demonstrates that increasing the buffer size reduces the number of read() system calls and improves file I/O efficiency. Among the tested buffer sizes, 4096 bytes required the fewest read calls.
