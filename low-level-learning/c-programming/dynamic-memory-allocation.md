---
cover: https://cdn.pixabay.com/photo/2016/07/27/10/44/fireworks-1544999_1280.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Dynamic Memory Allocation

#### <mark style="color:purple;">If we have a program that is designed to read in a set of data from a file into an array in memory we have three choices:</mark>

1. Define the array to contain the maximum number of possible elements at compile time: int array\[1000];
2. Use a variable-length array to dimension the size of the array at runtime int array\[a];
3. Allocate the array dynamically using one of C memory allocation routing.

## <mark style="color:red;">Heap and stack</mark>

### <mark style="color:yellow;">What is a stack?</mark>

A stack is a special area of computer's memory which stores temporary variables created by a function.

&#x20;In stack, variables are declared, stored and initialized during runtime.&#x20;

It is a temporary storage memory.&#x20;

When the computing task is complete, the memory of the variable will be automatically erased.&#x20;

The stack section mostly contains methods, local variable, and reference variables.

### <mark style="color:yellow;">What is heap?</mark>

The heap is a memory used by programming languages to store global variables.

By default, all global variable are stored in heap memory space.

It supports Dynamic memory allocation.&#x20;

The heap is not managed automatically for you and is not as tightly managed by the CPU.&#x20;

It is more like a free-floating region of memory.

## <mark style="color:red;">malloc</mark>

The simplest standard library function that allocates memory at runtime.&#x20;

{% hint style="info" %}
We need to include the stdlib.h header file.
{% endhint %}

```c
 #include <stdlib.h>
```

Specify the number of bytes of memory that you want to allocate as the argument.&#x20;

Returns the address of the first byte of memory that is allocated.&#x20;

Because you get an address returned, a pointer is the only place to put it.

```c
 int *pnumber = (int*)malloc(100);
```

Request 100 bytes of memory and assign the address of this memory block to pnumber pnumber will point to the first int location at the 100 bytes that were allocated.&#x20;

Can hold 25 int values on my computer, because they require 4 bytes each assumes that type int requires 4 bytes.

{% hint style="info" %}
Using malloc in this way is not always good because it might not work on different systems.
{% endhint %}

Its better to remove the assumption that integers are 4 bytes and make sure our program works on any system architecture.

```c
 int *pnumber = (int*)malloc(25*sizeof(int));
```

{% hint style="info" %}
This code will now work in different OSs and different architectures.
{% endhint %}

25sizeof(int) indicates that sufficient bytes for accommodating 25 values of type int should be made available so this value is chosen by the compiler on different systems.

Also notice the cast (int) which converts the address returned by the function to the type pointer to int.

Malloc returns a pointer of type pointer to void so we have to cast it.

If the memory that we request can not be allocated for any reason malloc() returns a pointer with the value NULL.&#x20;

It is always a good idea to check any dynamic memory request immediately using an if statement to make sure the memory is actually there before we try to use it.

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdlib.h>
int main(){

    int *pnumber = (int*) malloc(25*sizeof(int));
    if (!pnumber)
    
    {
        // code to deal with the memory shortage
    }
}
```

## <mark style="color:red;">Releasing the memory</mark>

Heap memory is automatically released after program exits.&#x20;

A memory leak occurs when we allocate some memory dynamically and we don't retain the reference to it, so we are unable to release the memory.&#x20;

Because we don't release the memory when we no longer need it, the program consumes more and more of the available memory on each loop iteration and eventually may occupy it all!&#x20;

To free memory that we have allocated dynamically we must still have access to the address that references the block of memory.

#### <mark style="color:purple;">To release the memory for a block of dynamically allocated memory whose address we have stored in a pointer:</mark>

```c
free(pnumber);
pnumber = NULL;
```

## <mark style="color:red;">calloc</mark>

Offers a couple of advantages over malloc() it allocates memory as a number of elements of a given size it initializes the memory that is allocated os that all bytes are zero.

#### <mark style="color:purple;">Requires two arguments:</mark>

1. Number of data items for which space is required 2.&#x20;
2. Size of each data item.

{% hint style="info" %}
#### Is declared in the stdlib.h header.
{% endhint %}

```c
int *pnumber = (int*) calloc(75, sizeof(int));
```

The return value will still be NULL if it was not possible to allocate the memory requested very similar to using malloc() but the big plus is that we know the memory area will be initialized to 0.

## <mark style="color:red;">realloc</mark>

Enables us to reuse or extend memory that we previously allocated using malloc() or calloc().

#### <mark style="color:purple;">Expects two arguments:</mark>&#x20;

1. &#x20;A pointer containing an address that was previously returned by a call to malloc(), alloc() .
2. The size in bytes of the new memory that we want to allocate.

Allocates the amount of memory we specify by the second argument transfers the content of the previously allocated memory referenced by the pointer that you supply as the first argument to the newly allocated memory returns a void\* pointer to the new memory or NULL if the operation fails for some reason.

realloc() preserves the contents of the original memory area.

```c
 ptr = realloc(ptr, n * sizeof(int));
```

#### <mark style="color:purple;">Example:</mark>

```c
char *str;
str = (char*)calloc(25, sizeof(char));

strcpy(str, "this is a test :)");

printf("the pointer address before realloc: %p\n", str);
printf("the pointer value before realloc: %s\n", str);

str = (char*)realloc(str, 30* sizeof(char));

strcat(str, "and this is another one");

printf("the pointer value after realloc: %s\n", str);
printf("the pointer address after realloc: %p\n", str);

free(str);
```

#### <mark style="color:purple;">The output is this:</mark>

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Now we are using the same memory address for new data.

{% hint style="info" %}
Avoid allocating lots of small amounts of memory.&#x20;

Allocating memory on heap carries some overhead with it allocating many small blocks of memory will carry much more overhead than allocating fewer larger blocks
{% endhint %}
