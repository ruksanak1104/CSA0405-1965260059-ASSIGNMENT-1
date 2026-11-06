System Calls:

A system call is a mechanism through which an application program requests services from the operating system kernel. System calls act as an interface between user programs and the operating system.


1) Categories of System Calls
- Process Control:
  - Description: Creates, executes and terminates processes.
  - Examples: fork(), exec(), wait(), exit().
- File Management:
  - Description: Creates, reads, writes and deletes files.
  - Examples: open(), read(), write(), close().
- Device Management:
  - Description: Performs operations on hardware devices.
  - Examples: Device read and write operations.
- Information Maintenance:
  - Description: Obtains system information and process details.
  - Examples: getpid(), time().
- Communication:
  - Description: Allows processes to exchange information.
  - Examples: pipe(), shmget().
- Protection:
  - Description: Controls access permissions and security.
  - Examples: chmod(), chown().


2) Sequence of Operations in the read() System Call

When an application calls the read() system call, the following steps take place:
1. The application program calls the read() function.
2. The system call interface receives the request.
3. A trap transfers control from user mode to kernel mode.
4. The kernel checks the file descriptor and access permissions.
5. The kernel performs the required reading operation.
6. The requested data is transferred into the application's buffer.
7. The kernel returns the result to the application.
8. The program continues execution in user mode.
3. Methods of Passing System Call Parameters




System call parameters can be passed using three methods.

1. Passing Parameters Through Registers:

Parameters are stored directly in CPU registers. The operating system reads the values from these registers.

2. Passing Parameters Through a Memory Block:

Parameters are stored in a memory block, and the address of that block is passed to the operating system.

3. Passing Parameters Through the Stack:

Parameters are pushed onto the stack by the application program. The operating system retrieves them from the stack.

4. Using strace to Observe System Calls

The strace command is a Linux utility used to observe the system calls made by a program.




Example:
strace ls
This displays the system calls executed by the ls command.


To display system call statistics:
strace -c ls

The strace -c command displays the number of system calls and the time spent executing them.




C Program to Demonstrate System Calls:


#include <stdio.h>

#include <stdlib.h>

#include <fcntl.h>

#include <unistd.h>

int main()

{
    int fd;
    char buffer[100];
    ssize_t bytes;

    // Create and write into a file
    fd = open("sample.txt",
              O_WRONLY | O_CREAT | O_TRUNC, 0644);

    if (fd < 0)
    {
        perror("File creation failed");
        return 1;
    }

    char message[] = "Hello! This is a system call demonstration.\n";

    write(fd, message, sizeof(message) - 1);
    close(fd);

    // Open and read the file
    fd = open("sample.txt", O_RDONLY);

    if (fd < 0)
    {
        perror("File opening failed");
        return 1;
    }

    printf("Reading file contents:\n");

    while ((bytes = read(fd, buffer, sizeof(buffer) - 1)) > 0)
    {
        buffer[bytes] = '\0';
        printf("%s", buffer);
    }

    close(fd);

    printf("\nSystem calls demonstrated successfully.\n");

    return 0;
}



5) Comparison Between fopen/fread/fwrite and open/read/write
- Level:
  - fopen(), fread(), fwrite(): C library functions.
  - open(), read(), write(): System calls.
- Buffering:
  - fopen(), fread(), fwrite(): Generally uses library buffering.
  - open(), read(), write(): No automatic C stdio buffering.
- Performance:
  - fopen(), fread(), fwrite(): Can reduce system call frequency.
  - open(), read(), write(): Depends on the buffer size used.
- Control:
  - fopen(), fread(), fwrite(): Less direct control over system calls.
  - open(), read(), write(): More direct control over file operations.
- Usage:
  - fopen(), fread(), fwrite(): Convenient file handling.
  - open(), read(), write(): Low-level file operations.
For example, when copying a large file, buffered functions collect data before performing read and write operations. This can reduce the number of system calls.


6) Alternative Evaluation

If a program reads a file one byte at a time using read(), it may require many system calls.

If the program uses a buffer of 4096 bytes, it can read multiple bytes in a single system call. This reduces the number of kernel transitions.

However, larger buffers require additional memory. Therefore, a suitable buffer size should be selected according to the file size and system performance.


Conclusion

System calls provide communication between application programs and the operating system kernel. Functions such as fork(), read(), write() and open() help perform important operating system operations. Tools such as strace help observe system calls, while buffering can reduce system call overhead and improve file-handling performance.













