## Questions and Answers
1.Program to create a thread and print "Hello World".
```c
#include<stdio.h>
#include<pthread.h>
void *print(void *arg)
{
    printf("Hello world!\n");
    return NULL;
}
int main()
{
    pthread_t thread;
    pthread_create(&thread,NULL,print,NULL);
    pthread_join(thread,NULL);
    return 0;
}
```
2.Program to create a multiple threads and print hello.
```c
#include<stdio.h>
#include<pthread.h>
#define NUM 5
void *print(void *arg)
{
    int id=*((int *)arg);
    printf("thread %d: hello from thread!\n",id);
    return NULL;
}
int main()
{
    pthread_t threads[NUM];
    int thread_ids[NUM];
    for(int i=0;i<NUM;i++)
    {
        thread_ids[i]=i+1;
        pthread_create(&threads[i],NULL,print,&thread_ids[i]);
    }
    for(int i=0;i<NUM;i++)
    {
        pthread_join(threads[i],NULL);
    }
    return 0;
}
```
3.program to create a thread and print 1 to 10 numbers.
```c
#include<stdio.h>
#include<unistd.h>
#include<pthread.h>
void *printnumbers(void *arg)
{
    int thread_id=*((int *)arg);
    for(int i=1;i<=10;i++)
    {
        printf("Thread %d: %d\n",thread_id,i);
      //  sleep(5);
    }
    return NULL;
}
int main()
{
    pthread_t thread1,thread2;
    int id1=1,id2=2;
    pthread_create(&thread1,NULL,printnumbers,&id1);
    pthread_create(&thread2,NULL,printnumbers,&id2);
    pthread_join(thread1,NULL);
    pthread_join(thread2,NULL);
    return 0;
}
```
4.Program to create thread and get the factorial of a number.
```c
#include<stdio.h>
#include<pthread.h>
int i,num;
unsigned long long result=1;
void *factorial(void *args)
{
   
    for(i=1;i<=num;i++)
    {
        result*=i;
    }
    return NULL;
}
int main()
{
    pthread_t thread;
    printf("Enter a number");
    scanf("%d",&num);
    pthread_create(&thread,NULL,factorial,&num);
    pthread_join(thread,NULL);
    printf("Factorial: %d is %lu\n",num,result);
    return 0;
}
```
5.Program to Create two threads and print the id of two threads.
```c
#include<stdio.h>
#include<pthread.h>
void *threadid(void *args)
{
    pthread_t id=pthread_self();
    printf("Hello from thread! thread ID: %lu\n",(unsigned long)id);
    return NULL;
}
int main()
{
    pthread_t thread1,thread2;
    pthread_create(&thread1,NULL,threadid,NULL);
    pthread_create(&thread2,NULL,threadid,NULL);
    pthread_join(thread1,NULL);
    pthread_join(thread2,NULL);
    return 0;
}
```
6.Program to create a thread and print sum of two numbers.
```c
#include<stdio.h>
#include<pthread.h>
//int num1=2,num2=5;
//int result;
void *sum(void *args)
{
    int *num=(int *)args;
    int result=num[0]+num[1];
   // printf("Result:%d",result);
    return NULL;
}
int main()
{
    pthread_t id1;
    int numbers[2]={12,10};
    pthread_create(&id1,NULL,sum,(void *)numbers);
  //  pthread_create(&thread2,NULL,sum,NULL);
    pthread_join(id1,NULL);
    printf("Result:%d",result);
    return 0;
}
```
7.Program to create a thread and calculates the square of an number.
```c
#include<stdio.h>
#include<pthread.h>
void *squarenumber(void *args)
{
    int num=*(int *)args;
    printf("Result: %d\n",num*num);
}
int main()
{
    pthread_t id1;
    int num,result;
    printf("Enter a number: ");
    scanf("%d",&num);
    pthread_create(&id1,NULL,squarenumber,(void *)&num);
    pthread_join(id1,NULL);
    return 0;
}
```
8.Program to create a thread and print date and time.
```c
#include<stdio.h>
#include<pthread.h>
#include<time.h>
void *dateandtime(void *args)
{
    time_t now;
    struct tm *current;
    time(&now);
    current=localtime(&now);
    printf("time=%s\n",asctime(current));
}
int main()
{
    pthread_t id;
    pthread_create(&id,NULL,dateandtime,NULL);
    pthread_join(id,NULL);
    return 0;
}
```
9.program to create a thread and check whethergiven is prime number or not.
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
void *primenumber(void *args)
{
    int num=*(int *)args;
    int i;
    int *result=malloc(sizeof(int));
    if(result==NULL)
    {
        printf("Memory Allocation failed\n");
        pthread_exit(NULL);
    }
    *result=1;
    if(num<=1)
        *result=0;
    else
    {
        for(i=2;i*i<=num;i++)
        {
            if(num%i==0)
            {
                *result=0;
                break;
            }
        }
    }
    pthread_exit(result);
}
int main()
{
    pthread_t thread;
    int number;
    int *is_prime;
    printf("Enter a number: ");
    scanf("%d",&number);
    pthread_create(&thread,NULL,primenumber,&number);
    pthread_join(thread,(void **)&is_prime);
    if(*is_prime)
    {
        printf("%d is a prime number.\n",number);
    }
    else
    {
        printf("%d is not a prime number.\n",number);
    }
    free(is_prime);
}
```
10.Progran to create a thread and check whether it is a palindrome or not.
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<pthread.h>
void *checkpalindrome(void *args)
{
    int *ptr=(int *)malloc(sizeof(int));
    char *str=(char *)args;
    int len=strlen(str),i=0;
    if(ptr==NULL)
    {
        printf("Malloc failed\n");
        pthread_exit(0);
    }
    *ptr=1;
    for(i=0;i<len;i++)
    {
        if(str[i]!=str[--len])
        {
            *ptr=0;
            return ptr;
            break;
        }
    }
    return ptr;
}
int main(void)
{
    char str[100];
    int *flag;
    pthread_t tid;
    printf("Enter a string(99<): ");
    scanf("%s",str);
    pthread_create(&tid,NULL,checkpalindrome,(void *)str);
    pthread_join(tid,(void **)&flag);
    if(*flag==0)
    {
        printf("Given string is not palindrome\n");
    }
    else
    {
        printf("Given string is palindrome\n");
    }
    free(flag);
    return 0;
}
```
11.Program to create a thread and print "hello world" using thread syncronisation.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
pthread_mutex_t lock;
void *mutex(void *args)
{
   pthread_mutex_lock(&lock);
   printf("Hello,World!\n");
   pthread_mutex_unlock(&lock);
   return NULL;
}
int main()
{
    pthread_t tid;
    pthread_mutex_init(&lock,NULL);
    pthread_create(&tid,NULL,mutex,"HEllO WORLD");
    pthread_join(tid,NULL);
    pthread_mutex_destroy(&lock);
    return 0;
}
```
12.program to create two thread and print it its id and synchronise their output.
```c
#include<stdio.h>
#include<unistd.h>
#include<pthread.h>
pthread_mutex_t lock;
void *mutexlock(void *args)
{
    pthread_mutex_lock(&lock);
    printf("Thread id: %lu\n",(unsigned long) pthread_self());
    pthread_mutex_unlock(&lock);
    return NULL;
}
int main()
{
    pthread_t td1,td2;
    if(pthread_mutex_init(&lock,NULL)!=0)
    {
        printf("Mutex Failed");
        return 0;
    }
    pthread_create(&td1,NULL,mutexlock,NULL);
    pthread_create(&td2,NULL,mutexlock,NULL);
    pthread_join(td1,NULL);
    pthread_join(td2,NULL);
    pthread_mutex_destroy(&lock);
    return 0;
}
```
13.Program to print the create a thread that generates random numbers and syncronizes access to shared buffer.
```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<pthread.h>
#define num 5
int i,size;
pthread_mutex_t lock;
void *randomnumbers(void *args)
{   
    for(i=0;i<num;i++)
    {
    pthread_mutex_lock(&lock);
    size=rand()%100;
    printf("Generated nUmber: %d\n",size);
    pthread_mutex_unlock(&lock);
    }
    return NULL;
}
int main()
{
    pthread_t td;
    srand(time(NULL));
    if(pthread_mutex_init(&lock,NULL)!=0)
    {
        printf("Mutex Failed");
        return 0;
    }
    pthread_create(&td,NULL,randomnumbers,NULL);
    pthread_join(td,NULL);
    printf("Final Value: %d",size);
    pthread_mutex_destroy(&lock);
    return 0;
}
```
14.Program to create a thread and print sum of two numbers with syncronisation.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
pthread_mutex_t lock;
void *sumnumbers(void *args)
{
    int *num=(int *)args;
    pthread_mutex_lock(&lock);
    int result=num[0]+num[1];
    printf("Sum: %d\n",result);
    pthread_mutex_unlock(&lock);
    return NULL;
}
int main()
{
    pthread_t id;
    int num[2]={4,5};
    pthread_mutex_init(&lock,NULL);
    pthread_create(&id,NULL,sumnumbers,num);
    pthread_join(id,NULL);
    pthread_mutex_destroy(&lock);
    return 0;
}
```
15.program to create a thread performs addition of two numbers with mutex locks.
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t lock;
int size=5;
void *increment(void *args)
{
    //int num=*(int *)args;
    int i,num=5;
    for(i=0;i<size;i++)
    {
    pthread_mutex_lock(&lock);
    printf("increment: %d\n",num++);
    pthread_mutex_unlock(&lock);
    }
    return NULL;
}
void *decrement(void *args)
{
    //int num=*(int *)args;
    int i,num=5;
    for(i=0;i<size;i++)
    {
    pthread_mutex_lock(&lock);
    printf("Decrement: %d\n",num--);
    pthread_mutex_unlock(&lock);
    }
    return NULL;
}
int main()
{
    pthread_t id1,id2;
    pthread_mutex_init(&lock,NULL);
    pthread_create(&id1,NULL,increment,NULL);
    pthread_create(&id2,NULL,decrement,NULL);
    pthread_join(id1,NULL);
    pthread_join(id2,NULL);
    pthread_mutex_destroy(&lock);
    return 0;
}
```
16.program to create a thread that reads input from the user and synchronizes access to shared resources? 
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t lock;
char str[100];
void *inputfromuser(void *args)
{
    printf("Enter a string: ");
    pthread_mutex_lock(&lock);
    scanf("%s",str);
    printf("string is: %s\n",str);
    pthread_mutex_unlock(&lock);
    return NULL;
}
int main()
{
    pthread_t id;
    pthread_mutex_init(&lock,NULL);
    pthread_create(&id,NULL,inputfromuser,NULL);
    pthread_join(id,NULL);
    pthread_mutex_destroy(&lock);
    return 0;

}
```
