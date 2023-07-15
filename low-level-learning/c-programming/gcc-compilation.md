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

# Introduction

## <mark style="color:red;">First Program</mark>

```c
#include <stdio.h>

int main()
{
    printf ("Rust is dogshit!");
    return 0;
}
```

## <mark style="color:red;">Detect the version of C</mark>

```c
#include <stdio.h>

int main(int argc, char **argv) {

#if __STDC_VERSION__ >=  201710L
  printf("We are using C18!\n");
  
#elif __STDC_VERSION__ >= 201112L
  printf("We are using C11!\n");
  
#elif __STDC_VERSION__ >= 199901L
  printf("We are using C99!\n");
#else
  printf("We are using C89/C90!\n");
  
#endif

  return 0;
}
```

## <mark style="color:red;">Comments</mark>

```c
/* 
     multiline comment
*/
  
// single line comment
```

## <mark style="color:red;">Taking command line arguments</mark>

```c
#include <stdio.h>

int main(int argc, char *argv[])

{
  int numberOfArgs = argc;
  char *argument = argv[0];
  char *argument2 = argv[1];

  printf("Number of arguments: %d\n", numberOfArgs);
  printf("Argument number 1 is the program name: %s\n", argument);
  printf("Argument2 is the command line argument: %s\n", argument2);
 
  return 0;
}
```

## <mark style="color:red;">NULL "\0"</mark>

A special character with the code value 0 is added to the end of each string to mark where it ends.

A string is always terminated by a null character so the length of a string is always one greater than the number of characters in the string that's why length - 1 is commonly used.

{% hint style="info" %}
NULL is a symbol that represents memory address that doesn't reference anything.
{% endhint %}

## <mark style="color:red;">Modular programming</mark>

We can put our code in multiple separate files and include the headers in the main file and use another source file to import functions and other instructions.&#x20;

#### <mark style="color:purple;">To do this:</mark>

1. Create a header file pointing to a source file:&#x20;

<mark style="color:orange;">**Create**</mark> <mark style="color:orange;">**other.h → header file.**</mark>

```c
#ifndef UNTITLED_OTHER_H
#define UNTITLED_OTHER_H

int getme(void); 

#endif // UNTITLED_OTHER_H
```

2. Create the source file for the header:&#x20;

<mark style="color:orange;">**Create**</mark> <mark style="color:orange;">**other.c → source file for header**</mark><mark style="color:orange;">.</mark>

```c
 int getme(void){
   return 3;
}
```

\
Here we define the **getme() function** which was referred to by header and will be used in the **main.c** source file.

{% hint style="info" %}
**We don't need to include anything in this source file.**
{% endhint %}

3. Include the header in main.c and use the function:&#x20;

<mark style="color:orange;">**Create main.c, include other.h, and use getme() function.**</mark>

```c
#include <stdio.h>
#include "other.h"

int main(){

  printf("%d\n", getme());

  return 0;
}
```

\
Here we have to include the header file with "double quotations" cause we know its in the same directory so we don't need to look for it in the whole system.&#x20;

4. To compile without an IDE from command line & compile all .c source files, headers are checked in compile time and are not included in the command:

<mark style="color:orange;">**gcc \*.c -o \[program name].**</mark>

{% hint style="info" %}
Use with -c flag instead of -o flag to get object files.
{% endhint %}
