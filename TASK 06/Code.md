#include <stdio.h>

#include <stdlib.h>

#include <fcntl.h>

#include <unistd.h>

#define FILE_SIZE 10000

void createFile()

{
    int fd;
    char data[FILE_SIZE];

    fd = open("input.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);

    if (fd < 0)
    {
        perror("File creation error");
        exit(1);
    }

    for (int i = 0; i < FILE_SIZE; i++)
    {
        data[i] = 'A';
    }

    int offset = 0;

    while (offset < FILE_SIZE)
    {
        int n = write(fd, data + offset, FILE_SIZE - offset);

        if (n <= 0)
        {
            perror("Write error");
            close(fd);
            exit(1);
        }

        offset += n;
    }

    close(fd);
}

void copyFile(int bufferSize)

{
    int source, destination;
    
    char buffer[4096];
    
    int n, readCalls = 0;

    source = open("input.txt", O_RDONLY);

    if (source < 0)
    {
        perror("Input file error");
        exit(1);
    }

    destination = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);

    if (destination < 0)
  
    {
        perror("Output file error");
        close(source);
        exit(1);
    }

    while (1)
    {
        n = read(source, buffer, bufferSize);
        readCalls++;

        if (n < 0)
        {
            perror("Read error");
            close(source);
            close(destination);
            exit(1);
        }

        if (n == 0)
        {
            break;
        }

        int offset = 0;

        while (offset < n)
        {
            int written = write(destination,
                                buffer + offset, n - offset);

            if (written <= 0)
            {
                perror("Write error");
                close(source);
                close(destination);
                exit(1);
            }

            offset += written;
        }
    }

    printf("Buffer size %4d bytes: %d read() calls\n",
           bufferSize, readCalls);

    close(source);
    close(destination);
}

int main()
{
    createFile();

    copyFile(1);
    copyFile(512);
    copyFile(4096);

    return 0;
}

