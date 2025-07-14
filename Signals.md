1.Program to catch and handle the SIGINT signal.
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void signalhandle(int sig)
{
	printf("Caught SIGINT(signal number %d).shutting down...\n",sig);
	exit(0);
}
int main()
{
	signal(SIGINT,signalhandle);
	printf("running press ctrl+ c to trigger SIGINT.\n");
	while (1)
	{
		sleep(1);
	}
	return 0;
}
```
2.Program to send a custom signal to another process. 
```c
                                                            Sender
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
#include<stdlib.h>
int main()
{
	int target;
	printf("Enter the PID of the target process: ");
	scanf("%d",&target);
	if(kill(target,SIGUSR1)==0)
	{
		printf("Sent to SIGUSR1 to process %d sucessfully.\n",target);
	}
	else
	{
		perror("Failed to send signal");
	}
	return 0;
}

                                                              Receiver
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<signal.h>
void handle(int sig)
{
    printf("Received SIGUSR1(signal number %d).\n",sig);
}
int main()
{
    signal(SIGUSR1,handle);
    printf("Receiver PID: %d\n",getpid());
    printf("waiting for SIGUSR1...\n");
    while(1)
    {
        pause();
    }
    return 0;
}
```
3.Program 
