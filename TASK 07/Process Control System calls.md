Aim:
To study process creation and management in an operating system using fork(), exec(), waitpid() and _exit() system calls.

Explanation:
1. fork() creates a new child process.
2. execlp() replaces the child process with the specified program (echo).
3. waitpid() makes the parent process wait until the child finishes.
4. _exit() terminates the child process and returns an exit status.
5. WIFEXITED() checks whether the child terminated normally.
6. WEXITSTATUS() retrieves the child's exit status.


Alternative Concept:
An alternative to using fork() followed by exec() is posix_spawn(). It creates a new process and executes a program in a single operation.


Comparison:

- fork() + exec():
- Creates a child process and then executes another program.
- posix_spawn():
- Creates a process and executes a program in one operation.


Comparison Points:
- fork() + exec() provides separate control over process creation and program execution.
- posix_spawn() provides a simpler way to create and execute a process.
- Both methods can be used to execute external programs.


Conclusion:

Thus, process creation and management were studied successfully using fork(), exec(), waitpid() and _exit(). The program demonstrates how a parent process creates child processes, executes programs and collects their exit statuses.
