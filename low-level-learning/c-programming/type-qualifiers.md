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

# Type Qualifiers

## <mark style="color:red;">const</mark>

Means the variable will not be changed by the program.

```c
 const int z = 10;
```

## <mark style="color:red;">Volatile</mark>

Volatile means that the variable will change its value so tells the compiler to suppress various kinds of optimizations e.g: caching.

#### <mark style="color:purple;">Only three types if variables should use volatile:</mark>

* Memory-mapped peripheral registers.
* Global variables modified by an interrupt service routine.
* Global variables accessed by multiple tasks within a multi-threaded application.

```c
volatile int z; 
volatile int *z;
```

## <mark style="color:red;">Storage classes</mark>

<figure><img src="../../.gitbook/assets/image (33).png" alt="" width="563"><figcaption></figcaption></figure>

Storage classes are used to describe the features of a variable / function.&#x20;

Include the scope, visibility and life-time.&#x20;

It helps up to trace the existence of a particular variable during the runtime of a program.

The lifetime of a variable is the time period during which variable exist in the memory.

The scope is where the variable can be referenced in a program.

A variables visibility or linkage determines for a multiple-source-file program whether the identifier is known only in the current source or in any source file with proper declarations.

#### <mark style="color:purple;">C provides 4 storage classes:</mark>

1. auto.
2. register.
3. extern.
4. static.

#### <mark style="color:purple;">The 4 classes can be split into 2 storage duration:</mark>

1\. Automatic storage duration.

2\. Static storage duration.

## <mark style="color:red;">Auto storage class</mark>

Created when the block in which they are defined is entered, exits while the block is active, destroyed when the block is exited.

All local variables have auto storage duration by default.

its better not to use auto as a storage-class specifier in c/c++.

```c
 auto int var;
```

## <mark style="color:red;">External variables</mark>

Functions contained in separate files can communicate through external variables, an extension to the concept of global variable.

A global var can be accessed and changed by other modules (files).

In the module that wants to access the external variable the data type is preceded with the key word extern in the declaration, tells the compiler that a globally defined variable from another file is to be accessed.

#### <mark style="color:purple;">Example:</mark>&#x20;

#### <mark style="color:purple;">We have a var called moveNumber to access it in other functions we can define it as a global var outside any function:</mark>

```c
 int moveNumber = 0;
```

#### <mark style="color:purple;">To access this var from another file use:</mark>

```c
 extern int moveNumber;
```

## <mark style="color:red;">Static variables</mark>

We might want to define a var that is global but not external.&#x20;

To do this we use a static variable definition.&#x20;

If the below declaration is made outside any function it makes the value of the variable accessible from any subsequence pointed in the file in which the definition appears and not functions contained in other files.

```c
 static int moveNumber = 0;
```

## <mark style="color:red;">Register</mark>

We can store variables of any kind in CPU registers as well which is much faster than RAM.&#x20;

This should be used only or variables that require quick access.&#x20;

It is the compilers choice to put it in a register or not.&#x20;

It might be stored in a register depending on hardware and implementation restrictions.

The max variable size is equal to the register size.&#x20;

The variable life-time is within the block.&#x20;

We cant obtain the address of a register variable using a pointer, there is no memory address

```c
 register int x;
```
