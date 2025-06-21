# Process Management Code Collection

This document lists all `.c` source files from the ZIP archive with their contents preserved as-is.

## `process_management/child_process.c`

```c
//Write a program in C to create a child process using fork() and print its PID
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() 
{
    pid_t pid;
    pid = fork();
    if (pid < 0)
    {
        perror("fork failed");
        return 1;
    }
    else if (pid == 0) 
    {
           printf("Child:PID is %d\n", getpid());
    }
    else
    {
        printf("Parent:child is created and it's PID is %d\n", pid);
    }

    return 0;
}


```

## `process_management/chroot_systemcall.c`

```c
//Describe the purpose of the chroot() system call and provide an example.
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    // Make sure you have a directory like "/tmp/jail" created with some files inside
    const char *new_root = "/tmp/jail";

    // Step 1: Change root directory
    if (chroot(new_root) != 0) {
        perror("chroot failed");
        exit(EXIT_FAILURE);
    }

    // Step 2: Set working directory to new "/"
    if (chdir("/") != 0) {
        perror("chdir failed");
        exit(EXIT_FAILURE);
    }

    printf("Inside chroot jail: new root is %s\n", new_root);
    system("ls");  // List files inside new root
    return 0;
}


```

## `process_management/cow_fork.c`

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    int val = 10;
    pid_t pid = fork();

    if (pid == 0) {
        // Child process
        val += 5;
        printf("Child (PID: %d), val = %d\n", getpid(), val);
    } else {
        // Parent process
        sleep(1);  // Just for clarity in output
        printf("Parent (PID: %d), val = %d\n", getpid(), val);
    }

    return 0;
}


```

## `process_management/create_zombie_process.c`

```c
//Write a program in C to create a zombie process and explain how to avoid it
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();
    if (pid < 0) 
    {
        perror("fork failed");
        exit(1);
    }
    if (pid == 0)
    {
  	printf("Child: PID = %d\n", getpid());
        printf("Child exiting...\n");
        exit(0); 
    } else 
    {
        printf("Parent: PID = %d\n", getpid());
        printf("Parent sleeping for 15 seconds... (check zombie using ps)\n");
        sleep(15); 
        printf("Parent exiting...\n");
    }

    return 0;
}


```

## `process_management/deamon_process.c`

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <fcntl.h>
#include <sys/stat.h>

void create_daemon() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("fork failed");
        exit(EXIT_FAILURE);
    }

    if (pid > 0) {
        // Exit parent
        printf("Parent exiting, child becomes daemon...\n");
        exit(EXIT_SUCCESS);
    }

    // Step 1: Create a new session
    if (setsid() < 0) {
        perror("setsid failed");
        exit(EXIT_FAILURE);
    }

    // Step 2: Change working directory
    chdir("/");

    // Step 3: Set file permissions
    umask(0);

    // Step 4: Close stdin, stdout, stderr
    close(STDIN_FILENO);
    close(STDOUT_FILENO);
    close(STDERR_FILENO);

    // Optional: Redirect stdout/stderr to a log file
    int fd = open("/tmp/daemon_log.txt", O_CREAT | O_WRONLY | O_APPEND, 0644);

    // Daemon loop
    while (1) {
        dprintf(fd, "Daemon running with PID %d...\n", getpid());
        sleep(5);
    }

    close(fd);
}

int main()
{
    create_daemon();
    return 0;
}


```

## `process_management/demo_execvp.c`

```c
//Program: Demonstrate execvp() Function
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() 
{
    char *args[] = {"ls", "-l", NULL};
    printf("Before execvp()\n");
   if (execvp(args[0], args) == -1)
    {
        perror("execvp failed");
        exit(1);
    }
     printf("After execvp()\n");

    return 0;
}


```

## `process_management/demo_execvpe.c`

```c
//Write a C program to demonstrate the use of the execvpe() function
#define _GNU_SOURCE  // Needed for execvpe
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    char *args[] = {"printenv", "MYNAME", NULL};  // Run: printenv MYNAME

    // Custom environment
    char *env[] = {
        "MYNAME=Hyderabad,Chennai",
        NULL
    };

    printf("Parent: Calling execvpe to run 'printenv MYNAME'\n");

    // Replace current process with 'printenv'
    execvpe("printenv", args, env);

    // If execvpe fails
    perror("execvpe failed");
    return 1;
}


```

## `process_management/demo_fork.c`

```c
//Write a C program to demonstrate the use of fork() system call
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid;
    pid = fork();
    if (pid < 0) 
    {
        perror("fork failed");
        return 1;
    }
    else if (pid == 0) 
    {
        printf("Child Process:\n");
        printf("PID of child: %d\n", getpid());
        printf("PID of parent: %d\n", getppid());
    }
    else 
    {
 	printf("Parent Process:\n");
        printf("PID of parent: %d\n", getpid());
        printf("PID of child: %d\n", pid);
    }

    return 0;
}


```

## `process_management/execve.c`

```c
//how the execve() system call works and provide a code example.
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    char *args[] = {"/bin/ls", "-l", NULL};  
    char *env[] = {NULL};                   
    printf("Before execve()\n");
    if (execve("/bin/ls", args, env) == -1)
    {
        perror("execve failed");
        exit(EXIT_FAILURE);
    }
    printf("After execve()\n");

    return 0;
}


```

## `process_management/fork_child.c`

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() 
{
    pid_t pid = fork();
    if (pid < 0)
    {
        perror("fork failed");
        exit(EXIT_FAILURE);
    }

    if (pid == 0) 
    {
        // Child process
        printf("Child: PID = %d\n", getpid());

        // Arguments to pass: "echo", "Hello", "good","morning", "from", "child", NULL
        char *args[] = {"echo", "Hello", "good", "morning", "from","child", NULL};

        // Replace child with echo command
        execvp("echo", args);

        // execvp only returns if there is an error
        perror("execvp failed");
        exit(EXIT_FAILURE);
    } else 
    {        
    	printf("Parent: PID = %d, created child PID = %d\n", getpid(), pid);
        wait(NULL);  // Wait for child to finish
        printf("Parent: Child finished.\n");
    }

    return 0;
}


```

## `process_management/multiple_child_process.c`

```c
//Write a C program to create multiple child processes using fork() and display their PIDs.
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    int n;
    printf("number of child process you want to create\n");
    scanf("%d",&n);
    for (int i = 0; i < n; i++) 
    {
        pid_t pid = fork();
        if (pid < 0)
       	{
            perror("fork failed");
            exit(1);
        }
        else if (pid == 0)
       	{
            printf("Child %d:PID is %d,and Parent PID is %d\n", i + 1, getpid(), getppid());
            exit(0); 
        }
     
    }
    sleep(1);
    printf("Parent:  PID is %d, I have created %d child process \n", getpid(), n);

    return 0;
}


```

## `process_management/multi_task_fork.c`

```c
//The role of the fork() system call in implementing multitasking.
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();
    if (pid == 0) 
    {
            for (int i = 0; i < 5; i++) 
	    {
            printf("Child doing task %d\n", i);
            sleep(1);
            }
    }
   ` else 
    {
        for (int i = 0; i < 5; i++)
       	{
            printf("Parent doing task %d\n", i);
            sleep(1);
        }
    }

    return 0;
}


```

## `process_management/prevent_zombie_process.c`

```c
//1.way to prevent zombie process is by using wait() or waitpid().
/*
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        printf("Child: PID = %d\n", getpid());
        exit(0);
    } else {
        int status;
        wait(&status);  // Parent reaps the child
        printf("Parent: Cleaned up child. No zombie.\n");
    }

    return 0;
}
*/
//2.way to prevent zombie process is by using a signal handler for SIGCHLD.
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>
#include <sys/wait.h>

void handler(int sig) {
    while (waitpid(-1, NULL, WNOHANG) > 0);
}

int main() {
    signal(SIGCHLD, handler);  // Handle child exit

    if (fork() == 0) {
        printf("Child: PID = %d\n", getpid());
        exit(0);
    }

    printf("Parent sleeping...\n");
    sleep(5);  // Parent won’t create zombies
    return 0;
}


```

## `process_management/semaphore_synchronization.c`

```c
// Write a program in C to demonstrate process synchronization using semaphores
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/wait.h>
#include <semaphore.h>

int main() {
    const char *sem_name = "/mysem";  
    sem_t *sem = sem_open(sem_name, O_CREAT | O_EXCL, 0644, 0);
    if (sem == SEM_FAILED) 
    {
        perror("sem_open");
        exit(EXIT_FAILURE);
    }
    pid_t pid = fork();
    if (pid == 0) 
    {
        // Child process
        printf("Child: Doing some work...\n");
        sleep(3);  // Simulate some work
        printf("Child: Done. Signaling parent using sem_post().\n");
        sem_post(sem);  // Signal parent
        exit(0);
    } 
    else
    {
        printf("Parent: Waiting for child to finish using sem_wait()...\n");
        sem_wait(sem);  // Wait until child signals
        printf("Parent: Got signal. Child has finished.\n");

        wait(NULL);  // Clean up zombie

        // Cleanup: Close and unlink the semaphore
        sem_close(sem);
        sem_unlink(sem_name);
    }

    return 0;
}


```

## `process_management/system.c`

```c
//Write a C program to demonstrate the use of the system() function for executing shell commands
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("Listing current directory:\n");
    system("ls -l");

    printf("\nCreating a new directory named 'test_dir'\n");
    system("mkdir test_dir");  

    printf("\nDisplaying current date and time:\n");
    system("date");  

    printf("\nRemoving the directory 'test_dir'\n");
    system("rmdir test_dir");  

    return 0;
}


```

## `process_management/waitpid.c`

```c
// Write a C program to demonstrate the use of the waitpid() function for process synchronization.
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid;
    int status;
    pid = fork();
    if (pid < 0) 
    {
        perror("fork failed");
        exit(1);
    }
    if (pid == 0)
    {
        printf("Child: PID = %d, doing some work...\n", getpid());
        sleep(3);  
        printf("Child: Finished work and exiting.\n");
        exit(42); // return custom exit code
    } else 
    {
        printf("Parent: Waiting for child PID %d to finish...\n", pid);
        pid_t wpid = waitpid(pid, &status, 0);  //wait for the specified process
        if (wpid == -1) 
	{
            perror("waitpid failed");
        }
       	else 
	{
            printf("Parent: Child PID %d finished.\n", wpid);

            if (WIFEXITED(status))
	    {
                int exit_status = WEXITSTATUS(status);
                printf("Parent: Child exited with status %d.\n", exit_status);
            } 
	    else 
	    {
                printf("Parent: Child did not terminate normally.\n");
            }
        }
    }

    return 0;
}


```

## `process_management/wait_system_call.c`

```c
//demonstrate of wait() system call
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
int main() {
    pid_t pid = fork();
    if (pid == 0)
    {
        printf("Child: PID = %d\n", getpid());
       // return 42;//here we can give any number 
    } else 
    {
        int status;
        wait(&status); 
        printf("Parent: Child exited with status %d\n", WEXITSTATUS(status));
    }

    return 0;
}


```

