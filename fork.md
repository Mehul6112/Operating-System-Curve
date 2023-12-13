 "child process" is created with the fork system call and in OS returns an integer value and requires no argument. separate the parent from the child by checking the returned value of the fork():

1. Negative: A child process could not be successfully created if the fork() returns a negative value.
2. Zero: A new child process is successfully created if the fork() returns a zero.
3. Positive: The positive value is the process ID of a child's process to the parent.s.
Once the child process has been created, The child process and the parent process will both point to the next command. In this manner, the remaining commands will be executed 
2n times, where n represents the number of fork() system calls.

        #include <stdio.h>
        #include <sys/types.h>
        #include <unistd.h>

        //main function begins
        int main()
        {
            fork();
            fork();
            fork();
            printf("this process is created by fork() system call");
            return 0;
        }
        // fork() is used 3 times: therefore printf will be executed 8 times
getpid() is used to get the process ID
getppid() is used to get the parent process id

