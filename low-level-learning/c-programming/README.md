---
description: C is better than rust, stop being a coward and handle your own memory!
cover: https://cdn.pixabay.com/photo/2016/07/27/10/44/fireworks-1544999_1280.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# C Programming

### <mark style="color:yellow;">First Program</mark>

```c
#include <stdio.h>

int main()
{
    printf ("Oh man, you look like an ape!");
    return 0;
}
```

### <mark style="color:yellow;">Detect the version of C</mark>

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

### <mark style="color:yellow;">Comments</mark>

```c
  /* 
     multiline comment
  */
  
 // single line comment
```

### <mark style="color:yellow;">Taking command line arguments</mark>

```c
#include <stdio.h>

int main(int argc,char *argv[])

{ int numberOfArgs = argc;
 char *argument = argv[0];
 char *argument2 = argv[1];

 printf("number of arguments: %d\n", numberOfArgs);
 printf("argument number 1 is the program name: %s\n", argument);
 printf("argument2 is the command line argument %s\n", argument2);
 
 return 0;
}
```

### <mark style="color:yellow;">NULL "\0"</mark>

A special character with the code value 0 is added to the end of each string to mark where it ends.

A string is always terminated by a null character so the length of a string is always one greater than the number of characters in the string.

We can add a "\0" to the end of a string, this will create two strings but only the first one will be printed.

#### <mark style="color:purple;">NULL is a symbol that represents a memory address that doesn't reference anything.</mark>

### <mark style="color:yellow;">Modular programming</mark>

We can put our code in multiple separate files and include the headers in the main file and use another source file to import functions and other instructions.&#x20;

#### <mark style="color:purple;">To do this:</mark>

1. <mark style="color:orange;">Create a header file pointing to a source file:</mark>&#x20;

<mark style="color:orange;">**Create**</mark> <mark style="color:orange;">**other.h → header file.**</mark>

```c
#ifndef UNTITLED_OTHER_H
#define UNTITLED_OTHER_H
int getme(void); // The function that this header referse to
#endif // UNTITLED_OTHER_H
```

\
This is pointing to a source file with the name **other.c** **(same as header.c)** which contains the source instructions for this header.&#x20;

When we include this header in the **main.c** source file it refers to the header and the header refers to the **other.c** source file.

2. <mark style="color:orange;">Create the source file for the header:</mark>&#x20;

<mark style="color:orange;">**Create**</mark> <mark style="color:orange;">**other.c → source file for header**</mark><mark style="color:orange;">.</mark>

```c
 int getme(void){
   return 3;
}
```

\
Here we define the **getme() function** which was referred to by header and will be used in the **main.c** source file. We don't need to include anything in this source file.

3. <mark style="color:orange;">Include the header in main.c and use the function:</mark>&#x20;

<mark style="color:orange;">**Create main.c, include header.h and use getme() function.**</mark>

```c
#include <stdio.h>
#include "other.h"

int main(){

 printf("%d\n", getme());

 return 0;
}
```

\
Here we have to include the header file with double quotations cause we know its in the same directory so we don't need to look for it in the whole system.&#x20;

Then we can use the **getme() function** defined in **other.c** source file which is referred to by the **other.c** header.

4. <mark style="color:orange;">To compile without an IDE from command line & compile all .c source files, headers are checked in compile time and are not included in the command:</mark>

<mark style="color:orange;">**gcc \*.c -o \[program name].**</mark>

{% hint style="info" %}
Use with -c without -o option to keep the object files if you want.

We can use object files only to compile the program same as we did with the source files, .o and .c is the same here.
{% endhint %}
