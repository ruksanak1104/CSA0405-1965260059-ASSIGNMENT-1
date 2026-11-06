#include <stdio.h>

#include <stdlib.h>

#include <unistd.h>

#include <sys/wait.h>

int main()

{
   
    pid_t pid;
    
    int status;

    printf("shell: running job 1\n");
   
    fflush(stdout);

    pid = fork();

    if (pid < 0)
    {
        perror("fork failed");
        exit(1);
    }

    if (pid == 0)
    {
        execlp("echo", "echo", "42", NULL);
        perror("exec failed");
        _exit(1);
    }
    else
    {
        waitpid(pid, &status, 0);

        printf("job 1 exit status %d\n",
               WIFEXITED(status) ? WEXITSTATUS(status) : -1);
    }

    printf("shell: running job 2\n");
    fflush(stdout);

    pid = fork();

    if (pid < 0)
    {
        perror("fork failed");
        exit(1);
    }

    if (pid == 0)
    {
        _exit(3);
    }
    else
    {
        waitpid(pid, &status, 0);

        printf("job 2 exit status %d\n",
               WIFEXITED(status) ? WEXITSTATUS(status) : -1);
    }

    return 0;
}

