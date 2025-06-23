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
```
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
```
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
```
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
5. program in C to create a zombie process.
```
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
---
