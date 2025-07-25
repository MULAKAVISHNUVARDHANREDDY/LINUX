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
3.program to catch and handle the SIGINT signal.
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
	printf("running press cntrl+ c to trigger SIGINT.\n");
	while (1)
	{
		sleep(1);
	}
	return 0;
}
```
4.Program to send the custom signal to Another process.
```c
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
```
5.program to ignore the SIGCHLD signal temporarily. 
```c
#include<stdio.h>
#include<unistd.h>
#include<signal.h>
#include<stdlib.h>
int main()
{
	signal(SIGCHLD,SIG_IGN);
	int pid=fork();
	if(pid==0)
	{
		printf("Child is executing:%d\n",getpid());
		sleep(1);
		printf("Child is Exiting:%d\n",getpid());
		exit(0);
	}
	sleep(2);
	printf("Parent Continuing: \n");
	signal(SIGCHLD,SIG_DFL);
	printf("SIGCHLD restored: \n");
	return 0;
}
```
6.program to block the SIGTERM signal using sigprocmask().
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handle_sigterm(int sig)
{
	printf("Received SIGTERM (signal %d),but it was unblocked",sig);
}
int main()
{
	sigset_t block_set,old_set;
	signal(SIGTERM,handle_sigterm);
	sigemptyset(&block_set);
	sigaddset(&block_set,SIGTERM);
	if(sigprocmask(SIG_BLOCK,&block_set,&old_set)<0)
	{
		perror("sigprocmask=block");
		exit(1);
	}
	printf("SIGTERM is now blocked.try sending it(e.g kill-15 %d)\n",getpid());
	printf("Sleeping for 10 seconds....\n");
	sleep(10);
	//step 4:unblock SIGTERM
	if(sigprocmask(SIG_SETMASK,&old_set,NULL)<0)
	{
		perror("sigprocmask-unblock");
	}
	printf("SIGTERM unblocked.if it was sent earlier,it will now delivered.\n");
	//wait to see if signal is delivered
	sleep(5);
	return 0;
}
```
7.program to handle the SIGALRM signal using sigaction(). 
```c
#include<stdio.h>
#include<signal.h>
#include<stdlib.h>
#include<unistd.h>
void handle_alarm(int sig)
{
	printf("Received SIGALRM(signal %d).timer expired!\n",sig);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_alarm;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGALRM,&sa,NULL)==-1)
	{
		perror("sigaction");
		exit(1);
	}
	printf("Setting alarm for 5 seconds...\n");
	alarm(5);
	printf("waiting for SIGALRM....\n");
	pause();
	printf("Program Completed.\n");
	return 0;
}
```
8.Program to install a custom signal handler for SIGTERM?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handle_sigterm(int sig)
{
	printf("Received SIGTERM(signal %d),performing cleanup..\n",sig);
	exit(0);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_sigterm;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGTERM,&sa,NULL)==-1)
	{
		perror("sigaction");
		exit(1);
	}
	printf("Custom SIGTERM handler installed.PID:%d\n",getpid());
	printf("Waiting for SIGTERM(try 'KIll-15 %d')..\n",getpid());
	while(1)
	{
		pause();
	}
	return 0;
}
```
/*
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handle_sigsegv(int sig)
{
	printf("caught SIGSEGV(signal %d).Invalid memory access detected!\n",sig);
	exit(1);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_sigsegv;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGSEGV,&sa,NULL)==-1)
	{
		perror("Sigaction");
		exit(1);
	}
	printf("SIGSEGV handler installed.triggering segmentation fault..\n");
	int *ptr=NULL;
	*ptr=42;
	printf("this line will not be reached.\n");
	return 0;
}
*/
/* //Create a program to handle the SIGILL signal (illegal instruction).
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handle_sigill(int sig)
{
	printf("Caught SIGILL(signal %d),IILEGAL instruction detected1\n",sig);
	exit(1);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_sigill;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGILL,&sa,NULL)==-1)
	{
		perror("sigaction");
		exit(1);
	}
	printf("SIGILL handler installed.About to trigger illegal instructons...\n");
	void(*bad_func)()=(void(*)())0xdead;
	bad_func();
	return 0;
}
*/
/* //Create a program to handle the SIGILL signal (illegal instruction).
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handle_sigill(int sig)
{
	printf("caught SIGILL(signal %d): illegal CPU instruction1\n",sig);
	exit(1);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_sigill;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGILL,&sa,NULL)==-1)
	{
		perror("sigaction");
		exit(1);
	}
	printf("Forcing SIGILL with bad instruction....\n");
	#if defined(_x86_64_) || defined(_i386_)
		_asm_("ud2");
	#else
		printf("unsupported architecture.cannot force SIGILL.\n");
	#endif
		return 0;
}
*/
/* //Write a program to handle the SIGABRT signal (abort). 
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handle_sigabrt(int sig)
{
	printf("Caught SIGABRT(signal %d): abort triggered!\n",sig);
	exit(1);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_sigabrt;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGABRT,&sa,NULL)==-1)
	{
		perror("sigaction");
		exit(1);
	}
	printf("SIGABRT handler installed.triggering abort in 3 seconds...\n");
	sleep(3);
	abort();
	return 0;
}
*/
/* // Implement a C program to handle the SIGQUIT signal. 
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handle_sigquit(int sig)
{
	printf("Caught SIGQUIT (signal %d).gracefull exit intiallised..\n",sig);
	exit(0);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_sigquit;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGQUIT,&sa,NULL)==-1)
	{
		perror("sigaction");
		exit(1);
	}
	printf("SIGQUIT handler installed.PID: %d\n",getpid());
	printf("Waiting for SIGQUIT(try Ctrl+\\ or 'kill-3 %d').\n",getpid());
	while(1)
	{
		pause();
	}
	return 0;
}
*/
/* // Write a program to handle the SIGTERM signal (termination request).
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handle_sigterm(int sig)
{
	printf("receievd SIGTERM(signal %d).cleaning up and exiting gracefully..\n",sig);
	exit(0);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_sigterm;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGTERM,&sa,NULL)==-1)
	{
		perror("sig action");
		exit(1);
	}
	printf("SIGTERM handler installed PID: %d\n",getpid());
	printf("Waiting for SIGTERM (use 'kill -15 %d' or 'kill %d').\n",getpid());
	while(1)
	{
		pause();
	}
	return 0;
}
*/
/* // Write a program to handle the SIGTSTP signal (terminal stop).
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handle_sigtstp(int sig)
{
	printf("\nCaught SIGTSTP(signal %d).Ignoring suspension and resuming...\n",sig);
}
int main()
{
	struct sigaction sa;
	sa.sa_handler=handle_sigtstp;
	sigemptyset(&sa.sa_mask);
	sa.sa_flags=0;
	if(sigaction(SIGTSTP,&sa,NULL)==-1)
	{
		perror("sigaction");
		exit(1);
	}
	printf("SIGTSTP handler installed.PID: %d\n",getpid());
	printf("Press Ctrl+Z to send SIGTSTP.it wil be handled and ignored.\n");
	while(1)
	{
		printf("Running...press CTRL+Z to test signal handling.\n");
		sleep(3);
	}
	return 0;
}
*/
/* // Write a program to handle the SIGVTALRM signal (virtual timer expired).
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
#include<sys/time.h>
void handle_vtalrm(int sig)
{
	printf("Alaram Signal(signal: %d)Virtual timer expired\n",sig);
}
int main()
{
	struct sigaction sa;
	struct itimerval timer;
	sa.sa_handler=handle_vtalrm;
	sa.sa_flags=0;
	if(sigaction(SIGVTALRM,&sa,NULL)==-1)
	{
		perror("sig action");
		exit(1);
	}
	timer.it_value.tv_sec=1;
	timer.it_value.tv_usec=0;
	timer.it_interval.tv_sec=1;
	timer.it_interval.tv_usec=0;
	if(setitimer(ITIMER_VIRTUAL,&timer,NULL)==-1)
	{
		perror("setitimer");
		exit(1);
	}
	while(1)
	{
		for(volatile long i=0;i<1e8;i++);
	}
	return 0;
}
*/

