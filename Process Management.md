## Question and Answers.
1.Program to demonstrate an fork() system call.
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
int main()
{
    int pid=fork();
    if(pid==0)
    {
        printf("Child processID: %d\n",getpid());
    }
    else
    {
        printf("Parent process ID: %d\n",getpid());
    }
    return 0;
}
```
2.Program to Illustrate the execvp() function.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main()
{
    char *args[]={"ls","-l",NULL};
    printf("Before ecevp call execution: ");
    if(execvp(args[0],args)==-1)
    {
        perror("execvp fails");
    }
    printf("After execvp()\n");
    return 0;
}
```
3.Program to create a child process using fork() and printf its PID.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main()
{
    int pid=fork();
    if(pid==0)
    {
        printf("child id: %d,parent id: %d\n",getpid(),getppid());
    }
    else
    {
        printf("child id: %d parent id: %d\n",pid,getpid());
    }
    return 0;
}
```
4.Program to create multiple child processes using fork() and display their 
PIDs. 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main()
{
    int n,i;
    printf("Enter number n: ");
    scanf("%d",&n);
    for(i=0;i<n;i++)
    {
        int pid=fork();
        if(pid<0)
        {
            perror("fork failed");
            exit(1);
        }
            if(pid==0)
            {
                printf("child %d: child id=%d,PPID=%d\n",i+1,getpid(),getppid());
                exit(0);
            }
        }
        for(i=0;i<n;i++)
        {
            wait(NULL);
        }
    printf("Parent Process(PID=%d)has created all children\n",getpid());
    return 0;
}
```
5. program in C to create a zombie process and how to avoid it.
   > To see whether Zombie process is created or not use Command "top" in the another terminal.
```c
    -create a Zombie Process.
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main()
{
    int pid=fork();
    if(pid<0)
    {
        perror("Fork creation Failed");
        exit(1);
    }
    if(pid==0)
    {
        printf("child ID: %d\n",getpid());
        exit(0);
    }
    else
    {
        sleep(30);
        printf("parent ID: %d is sleeping\n",getpid());
      //  sleep(3);
    }
    return 0;
}
                ----------------------------
    -To Avoid the Zombie Process.
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/wait.h>
int main()
{
    int pid=fork();
    if(pid<0)
    {
        perror("fork failed.");
        exit(1);
    }
    if(pid==0)
    {
        printf("Child process ID: %d",getpid());
        exit(0);
    }
    else
    {
        int status;
        wait(&status);
        printf("Parent PID=%d.No Zombie created.",getpid());
    }
    return 0;
}
```
6.program to demonstrate the use of the system() function for executing shell 
commands
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
    printf("List the files in directory");
    system("ls");
    printf("print the current working directory: ");
    system("pwd");
    printf("print date and time: ");
    system("date");
    printf("files date and permissions: ");
    system("ls -l");
    return 0;
}
```
7.Program to create an process using fork() and pass arguments to child process.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
int main()
{
    int pid=fork();
    if(pid<0)
    {
        perror("fork failed");
        exit(1);
    }
    if(pid==0)
    {
        printf("Chlid: Executing ls command...\n");
        execlp("/bin/ls","ls","-l",NULL);
        perror("execlp failed"); //Execute only when execlp fails.
    }
    else
    {
        printf("Parent: Created child with PID=%d",pid);
    }
    return 0;
}
```
8.
