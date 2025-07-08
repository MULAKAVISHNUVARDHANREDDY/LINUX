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
17.Program to check if its a leap year by creating thread with mutex locks.
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t lock;
void *leapyear(void *args)
{
    int year=*(int *)args;
    pthread_mutex_lock(&lock);
    if((year%4==0 && year%100!=0)|| year%400==0)
    {
        printf("Leap year:%d\n",year);
    }
    else
    {
        printf("Not Leap year:%d\n",year);
    }
    pthread_mutex_unlock(&lock);
    return NULL;
}
int main()
{
    pthread_t id;
    int year;
    pthread_mutex_init(&lock,NULL);
    printf("Enter the Year:");
    scanf("%d",&year);
    pthread_create(&id,NULL,leapyear,&year);
    pthread_join(id,NULL);
    pthread_mutex_destroy(&lock);
    return 0;
}
```
18.Program to create a thread and check if it is a palindrome with mutex locks.
```c
#include<stdio.h>
#include<string.h>
#include<pthread.h>
char str[100];
pthread_mutex_t lock;
void *palindrome(void *args)
{
    int start=0;
    int end=strlen(str)-1;
    int palindrome=1;
    pthread_mutex_lock(&lock);
    while(start<end)
    {
        char a=str[start];
        char b=str[end];
        if(a>='A' && a<='Z')
            a+=32;
        if(b>='A' && b<='Z')
            b+=32;
        if(a!=b)
        {
            palindrome=0;
            break;
        }
        start++;
        end--;
    }
    if(palindrome)
        printf("%s is an palindrome\n",str);
    else
        printf("%s is not an palindrome\n",str);
    pthread_mutex_unlock(&lock);
    return NULL;
}
int main()
{
    pthread_t id;
    printf("Enter the String:");
    scanf("%s",str);
    pthread_mutex_init(&lock,NULL);
    pthread_create(&id,NULL,palindrome,NULL);
    pthread_join(id,NULL);
    pthread_mutex_destroy(&lock);
    return 0;
}
```
19.Program to create a thread and calculate an area of an triangle.
```c
#include<stdio.h>
#include<pthread.h>
struct triangle
{
    float base;
    float height;
}t;
void *areaoftriangle(void *args)
{
    struct triangle *t=(struct triangle *)args;
    printf("Area of triangle is: \n");
    float area= 0.5 * t->base * t->height;
    printf("%f",area);
    return NULL;
}
int main()
{
    pthread_t id;
    struct triangle t;
    printf("Enter Base: ");
    scanf("%f",&t.base);
    printf("Enter height: ");
    scanf("%f",&t.height);
    pthread_create(&id,NULL,areaoftriangle,&t);
    pthread_join(id,NULL);
    return 0;
}
```
20.Program to create thread and print random array of integers.
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<time.h>
#define size 5
int arr[size];
void *randomarrayintegers(void *args)
{
    srand(time(NULL));
    for(int i=0;i<size;i++)
    {
        arr[i]=rand()%100;
    }
    pthread_exit(NULL);
}
int main()
{
    pthread_t id;
    pthread_create(&id,NULL,randomarrayintegers,NULL);
    pthread_join(id,NULL);
    for(int i=0;i<size;i++)
    {
        printf("%d\n",arr[i]);
    }
    return 0;
}
```
21.program to create a thread that calculates the greatest common divisor (GCD)of two numbers
```c
#include<stdio.h>
#include<pthread.h>
struct numbers
{
    int a;
    int b;
};
void *GCD(void *args)
{
    struct numbers *n=(struct numbers *)args;
    int a=n->a;
    int b=n->a;
    while(b!=0)
    {
        int temp=b;
        b=a%b;
        a=temp;
    }
    printf("GCD of %d and %d is %d\n",n->a,n->b,a);
    return NULL;
}
int main()
{
    pthread_t id;
    struct numbers n;
    printf("Enter the value a:");
    scanf("%d",&n.a);
    printf("Enter the value b:");
    scanf("%d",&n.b);
    pthread_create(&id,NULL,GCD,&n);
    pthread_join(id,NULL);
    return 0;
}
```
22.Program to calculate the sum of fibonacci limit by creating threrads.
```c
#include<stdio.h>
#include<pthread.h>
struct input
{
    int limit;
};
void *fibonacci(void *args)
{
    struct input *limit=(struct input*)args;
    int n=limit->limit;
    int a=0,b=1,sum=0,next;
    if(n<=0)
    {
        printf("sum=0\n");
        return NULL;
    }
    sum=a+b;
    for(int i=2;i<=n;i++)
    {
        next=sum;
        sum+=next;
        a=b;
        b=next;
    }
    printf("Sum of fibonacci %d is %d\n",n+1,sum);
    return NULL;
}
int main()
{
    pthread_t id;
    struct input limit;
    printf("Enter the limit:");
    scanf("%d",&limit.limit);
    pthread_create(&id,NULL,fibonacci,&limit);
    pthread_join(id,NULL);
    return 0;
}
```
23.Program to create a thread and print sum of even numbers 1 to 100.
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
int sum=0;
void *sumeven(void *args)
{
    for(int i=2;i<=100;i+=2)
    {
        sum+=i;
    }
    return NULL;
}
int main()
{
    pthread_t id;
    // printf("Enter the limit: ");
    // scanf("%d",&limit);
    pthread_create(&id,NULL,sumeven,NULL);
    pthread_join(id,NULL);
    printf("Sum of Even=%d",sum);
    return 0;
}
```
24.Program to create the thread and print factorial of a numbers.
```
#include<stdio.h>
#include<pthread.h>
void *factorial(void *args)
{
    int num=*(int *)args;
    int result=1;
    for(int i=1;i<=num;i++)
    {
        result*=i;
    }
    printf("Factoril %d is:%d\n",num,result);
    return NULL;
}
int main()
{
    pthread_t id;
    int num;
    printf("Enter the NUmber: ");
    scanf("%d",&num);
    pthread_create(&id,NULL,factorial,&num);
    pthread_join(id,NULL);
    return 0;
}
```
25.Program to create a pthread and check if the given year is leap year or not.
```c
#include<stdio.h>
#include<pthread.h>
void *leapyear(void *args)
{
    int year=*(int *)args;
    if(year%4==0&&year%100!=0 ||year%400==0)
    {
        printf("Year %d id Leap Year",year);
    }
    else
    {
        printf("Year %d is Not a leap Year",year);
    }
    return NULL;
}
int main()
{
    pthread_t id;
    int year;
    printf("Enter the year: ");
    scanf("%d",&year);
    pthread_create(&id,NULL,leapyear,&year);
    pthread_join(id,NULL);
    return 0;
}
```
26.Program to create a thread and calcuate the multiplication of two matrices.
```c
#include<stdio.h>
#include<pthread.h>
void *matrix(void *args)
{
    int arr1[50][50],arr2[50][50],i,j,m,n,mul[10][10];
    printf("Enter the numbers of rows and coloums:\n");
    scanf("%d%d",&m,&n);
    printf("Elements for 1 matrix:\n");
    for(i=0;i<m;i++)
    {
        for(j=0;j<n;j++)
        {
            scanf("%d",&arr1[i][j]);
        }
    }
    printf("Elements for 2 matrix:\n");
    for(i=0;i<m;i++)
    {
        for(j=0;j<n;j++)
        {
            scanf("%d",&arr2[i][j]);
        }
    }
    for(i=0;i<m;i++)
    {
        for(j=0;j<n;j++)
        {
            mul[i][j]=(arr1[i][j] * arr2[i][j]);
        }
    }
    for(i=0;i<m;i++)
    {
        for(j=0;j<n;j++)
        {
            printf("%d \n",arr1[i][j]);
        }
    }
    for(i=0;i<m;i++)
    {
        for(j=0;j<n;j++)
        {
            printf("%d \n",arr2[i][j]);
        }
    }
    for(i=0;i<m;i++)
    {
        for(j=0;j<n;j++)
        {
            printf("%d \n",mul[i][j]);
        }
      //  printf("\t");
        printf("\n");
    }
    return NULL;
}
int main()
{
    pthread_t id;
    pthread_create(&id,NULL,matrix,NULL);
    pthread_join(id,NULL);
    return 0;
}
```
27.Program to create a thread and cacluate the average of an 1 to 100 numbers.
```c
#include<stdio.h>
#include<pthread.h>
void *average(void *args)
{
    int start=0,end=100;
    float avg,sum=0;
    while(start<=end)
    {
        sum+=start;
        start++;
    }
    avg=(sum/end);
    printf("%.2f avf of %d ,Sum=%.2f",avg,end,sum);
    return NULL;
}
int main()
{
    pthread_t id;
    pthread_create(&id,NULL,average,NULL);
    pthread_join(id,NULL);
    return 0;
}
```
28.Program to create a thread to check the perfect square of an given number.
```c
#include<stdio.h>
#include<pthread.h>
struct square
{
    int n;
};
void *perfect_square(void *args)
{
    struct square *num=(struct square *)args;
    int i=1;
    while(i*i<num->n)
    {
        i++;
    }
    if(i*i==num->n)
    {
        printf("Perfect Square:");
    }
    else
    {
        printf("not a Perfect Square:");
    }
    return NULL;
}
int main()
{
    pthread_t id;
    struct square num;
    printf("Enter a number: ");
    scanf("%d",&num.n);
    pthread_create(&id,NULL,perfect_square,&num);
    pthread_join(id,NULL);
    return 0;
}
```
29.Program to create a thread and calculates the sum of digits ofgiven number.
```c
#include<stdio.h>
#include<pthread.h>
struct number
{
    int n;
};
void *sumofnum(void *args)
{
    struct number *num=(struct number *)args;
    int sum=0,rem;
    int n=num->n;
    while(n>0)
    {
        rem=n%10;
        sum=sum+rem;
        n=n/10;
    }
    printf("%d",sum);
    return NULL;
}
int main()
{
    pthread_t id;
    struct number num;
    printf("Enter the number: ");
    scanf("%d",&num.n);
    pthread_create(&id,NULL,sumofnum,&num);
    pthread_join(id,NULL);
}
```
30.Program to create a thread and calculate factorial of num using recursivly.
```c
#include<stdio.h>
#include<pthread.h>
int num;
int result;
int factorial(int n)
{
    if(n<=1)
        return 1;
    return n*factorial(n-1);
}
void *compute_factorial(void *arg)
{
    result=factorial(num);
    return NULL;
}
int main()
{
    pthread_t id;
    printf("Enter the number: ");
    scanf("%d",&num);
    if(num<=0)
    {
        printf("Factorial is not defined for negative numbers.\n");
        return 1;
    }
    pthread_create(&id,NULL,compute_factorial,NULL);
    pthread_join(id,NULL);
    printf("Factorial of %d is: %d",num,result);
    return 1;
}
```
31.program to create a thread and find max and min in an array.
```c
#include<stdio.h>
#include<pthread.h>
int arr[5]={8,3,1,5,6};
int max,min;
void *maxandmin(void *args)
{
   // int num=*(int *)args;
    int i;
    max=arr[0];
    min=arr[0];
    for(i=1;i<5;i++)
    {
        if(arr[i]>max)
            max=arr[i];
        if(arr[i]<min)
            min=arr[i];
    }
    return NULL;
}
int main()
{
    pthread_t id;
    pthread_create(&id,NULL,maxandmin,NULL);
    pthread_join(id,NULL);
    printf("MAX: %d,MIN: %d",max,min);
    return 0;
}
```
32.Program to create a thread and get an square root of an number.
```c
#include<stdio.h>
#include<pthread.h>
#include <math.h>
int number;
void *square(void *args)
{
    double result;
    if(number<0)
    {
        printf("Negative numbers are invalid.\n");
    }
    else
    {
        result=sqrt(number);
        printf("SquareRoot: %.2f",result);
    }
    return NULL;
}
int main()
{
    pthread_t id;
    printf("Enter the number: ");
    scanf("%d",&number);
    pthread_create(&id,NULL,square,NULL);
    pthread_join(id,NULL);
    return 0;
}
```
33.Program to create thread to find the average of elements in an array.
```c
#include<stdio.h>
#include<pthread.h>
#define size 10
int arr[size]={1,2,3,4,5,6,7,8,9,10};
float average=0;
void *averageofarray(void *args)
{
    int i;
    float sum=0;
    for(i=0;i<size;i++)
    {
        sum+=arr[i];
    }
    average=(float)sum/size;
    return NULL;
}
int main()
{
    pthread_t id;
    pthread_create(&id,NULL,averageofarray,NULL);
    pthread_join(id,NULL);
    printf("Average:%.2f",average);
    return 0;
}
```
34.program to create a thread that generates a random password?
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<time.h>
#define max_len 20
char password[max_len+1];
int length;
void *random_password(void *args)
{
    const char charset[]="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$^&*()_+{}|?><";
    int charset_len=sizeof(charset)-1;
    srand(time(NULL));
    for(int i=0;i<length;i++)
    {
        int index=rand()%charset_len;
        password[i]=charset[index];
    }
    password[length]='\0';
    pthread_exit(NULL);
}   
int main()
{
    pthread_t thread;
    printf("Enter desired password length(%d):",max_len);
    scanf("%d",&length);
    if(length<=0||length>max_len)
    {
        printf("Invalid length.please enter valid length(%d)",max_len);
        return 1;
    }
    pthread_create(&thread,NULL,random_password,NULL);
    pthread_join(thread,NULL);
    printf("Generated random password: %s\n",password);
    return 0;
}
```
35.program to create a thread that generates a random password? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<pthread.h>
#include<time.h>
char password[21];
void *random_password(void *args)
{
    const char charset[]="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$^&*()_+|}{:?><}";
    int charset_len=strlen(charset);
    srand(time(NULL));
    for(int i=0;i<20;i++)
    {
        password[i]=charset[rand()%charset_len];
    }
    password[20]='\0';
}
int main()
{
    pthread_t thread;
    pthread_create(&thread,NULL,random_password,NULL);
    pthread_join(thread,NULL);
    printf("random password: %s\n",password);
    return 0;
}
```
36.program to create a thread and print even or odd.
```c
#include<stdio.h>
#include<pthread.h>
void *prime_numbers(void *args)
{
    int num=*(int *)args;
    if(num % 2==0)
    {
        printf("Even number: %d",num);
    }
    else
    {
        printf("Odd number: %d",num);
    }
    pthread_exit(NULL);
}
int main()
{
    pthread_t thread;
    int num;
    printf("Enter the number: ");
    scanf("%d",&num);
    pthread_create(&thread,NULL,prime_numbers,&num);
    pthread_join(thread,NULL);
    return 0;
}
```
37.

