1.Program to Demonstration of an malloc.
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
    int *var=(int *)malloc(sizeof(int));
    if(var==NULL)
    {
        perror("malloc failed");
    }
    *var=34;
    printf("malloc: %d",*var);
    free(var);
    return 0;
}
```
2.Program to Demonstration of an calloc.
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
    int *ptr=(int *)calloc(2,sizeof(int));
    if(ptr==NULL)
    {
        printf("Calloc failed");
    }
    *ptr=56;
    printf("calloc: %d",*ptr);
    free(ptr);
    return 0;
}
```
3.program of an Demonstration of realloc.
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
    int *ptr=(int *)realloc(NULL,sizeof(int));
    if(ptr==NULL)
    {
        printf("realloc failed");
    }
    *ptr=90;
    printf("realloc: %d",*ptr);
    free(ptr);
    return 0;
}
```
4.Program in C to allocate memory for a linked list node dynamically.
```c

```
5.
