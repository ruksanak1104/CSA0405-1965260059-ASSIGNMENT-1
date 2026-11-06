#include <stdio.h>

#include <stdlib.h>

#include <unistd.h>

#include <sys/wait.h>

#include <pthread.h>

#include <time.h>

#define COUNT 1000

void *threadFunction(void *arg)

{
    return NULL;
}

double getTime()

{
    struct timespec t;
    clock_gettime(CLOCK_MONOTONIC, &t);

    return t.tv_sec + t.tv_nsec / 1000000000.0;
}

int main()

{
    pid_t pid;
    pthread_t thread;
    int i;
    double start, end;

    /* Creating 1000 processes */
    start = getTime();

    for (i = 0; i < COUNT; i++)
    {
        pid = fork();

        if (pid < 0)
        {
            perror("Fork failed");
            return 1;
        }

        if (pid == 0)
        {
            _exit(0);
        }
        else
        {
            waitpid(pid, NULL, 0);
        }
    }

    end = getTime();

    printf("Processes created: %d\n", COUNT);
    
    printf("Process creation and cleanup time: %f seconds\n",
           end - start);

    /* Creating 1000 threads */
    
    start = getTime();

    for (i = 0; i < COUNT; i++)
    
    {
        if (pthread_create(&thread, NULL, threadFunction, NULL) != 0)
        {
            printf("Thread creation failed\n");
            return 1;
        }

        pthread_join(thread, NULL);
    }

    end = getTime();

    printf("Threads created: %d\n", COUNT);
   
    printf("Thread creation and cleanup time: %f seconds\n",
           end - start);

    return 0;
}

