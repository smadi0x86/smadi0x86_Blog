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

# Preprocessors

#### <mark style="color:purple;">These statements are identified by the presence of a pound sign "#" which must be the first non-space character on the line like:</mark>

```c
#include <stdio.h>
```

#### <mark style="color:purple;">There are two ways to #include header files in a program:</mark>

1. Using angle brackets ( #include ) tells the preprocessor to look for the file in one or more standard system directories.
2. Using double quotes ( #include "smadi.h" ) tells the preprocessor to first look in the current working directory.

## <mark style="color:red;">Conditional compilation</mark>

`#define`\
`#undef`\
`#ifdef`\
`#ifndef`\
`#if`\
`#elif`\
`#else`\
`#endif`\
\
Used to create one program that can be compiled to run on different computer systems.&#x20;

If you had a large program that had many dependencies on specific hardware or software we might end up with many defines whose values would have to be changed when the program was moved to another computer system.\
\
We can help reduce this problem by incorporating the values of these defines for each different machine into the program by using the conditional compilation capabilities of the preprocessor.\
\
Its also used to switch on or off various statements in the program, trace the flow of a program or debug statements that print out the values of various variables.

#### <mark style="color:purple;">#if → Test the value of a constant expression.</mark>

{% hint style="info" %}
Every #if construct ends with an #endif.
{% endhint %}

#### <mark style="color:purple;">#else → To complement #ifdef/#ifndef and #if.</mark>

#### <mark style="color:purple;">#define → Defines an identifier, we can define them from the command line too.</mark>

#### <mark style="color:purple;">#ifdef → Checks whether an identifier is currently defined.</mark>

#### <mark style="color:purple;">#ifndef → The negative of the ifdef.</mark>

{% hint style="info" %}
Directives #ifdef and #ifndef are provided as shorthand for:

`if defined(name)`

`if !defined(name)`
{% endhint %}

#### <mark style="color:purple;">Example:</mark>

`#ifndef SIZE`

`#define SIZE 100`

#### <mark style="color:purple;">Example:</mark>

`#define UNIX 1 OR #define UNIX`

#### <mark style="color:purple;">Example:</mark>

```c
#ifdef UNIX
#define DATADIR "/dev/data"
#elif UNIX = "/data"

#define DATADIR "/usr/data"
#else
#define DATADIR "/dev/null"

#endif
```

#### <mark style="color:purple;">Most compilers also permit you to define a name to the preprocessor when the program is compiled using the special option -D:</mark>

```bash
gcc -D UNIX main.c
```

## <mark style="color:red;">Pragma</mark>

Lets you place compiler instructions in the source code.&#x20;

To control the amount of memory set aside for automatic variables to set the strictness of error checking to enable nonstandard language features.

{% hint style="info" %}
Pragma token\_name → token\_name is a command for the compiler to obey.
{% endhint %}

#### <mark style="color:purple;">gcc has the following pragmas:</mark>

```c
#pragma GCC dependency
#pragma GCC poison
#pragma GCC system_header
#pragma once
#pragma GCC warning
#pragma GCC error
#pragma message
```

#### <mark style="color:purple;">Pragma GCC dependency →</mark> Check the relative date of the current file and another file, if the other file is more recent than the current file, a warning is issued.

#### <mark style="color:purple;">Example:</mark>

```c
#pragma GCC dependency "parse.y"
```

{% hint style="info" %}
Pragma GCC poison → used to remove an identifier completely from the program.
{% endhint %}

#### <mark style="color:purple;">Example:</mark>

```c
#pragma GCC poison printf sprintf fprintf

sprintf(some_string,"hello"); // This will raise an error.
```

#### <mark style="color:purple;">Pragma GCC system\_header →</mark> Tells the compiler to consider the rest of the current include file as a system header.

#### <mark style="color:purple;">Pragma once →</mark> Specifies that the header file containing the directive is included only once even if the programmer includes it multiple times.

#### <mark style="color:purple;">Pragma GCC warning "message" →</mark> Causes the preprocessor to issue a warning diagnostic with the text "message".

#### <mark style="color:purple;">Pragma GCC error "message" →</mark> Cause the preprocessor to issue an error with the text "message".

#### <mark style="color:purple;">Pragma GCC message "message" →</mark> Prints string as a compiler message on compilation. the message is information only not warning or error.

## <mark style="color:red;">Error directive</mark>

Causes the preprocessor to issue an error message that includes any text in the directive.&#x20;

Error message is a sequence of characters by spaces.

{% hint style="info" %}
You do not have to in-close the text in quotes the message is optional.
{% endhint %}

#### <mark style="color:purple;">Example:</mark>

<pre class="language-c" data-overflow="wrap"><code class="lang-c">#if STDC_VERSION != 201112L // Failes if the compiler uses an older standard and successes when it uses C11.

Error Not C11

<strong>#endif
</strong></code></pre>

## <mark style="color:red;"># and ## operators</mark>

These are used for concatenation.&#x20;

Often useful to merge two tokens into one while expanding macros (called token pasting or token concatenation).

#### <mark style="color:purple;"># →</mark> Used within a macro definition causes a replacement text token to be converted to a string surrounded by quotes.

#### <mark style="color:purple;">Example:</mark>

```c
#define HELLO(x) printf("hello," #x "\n");

// When HELLO(john) apears in a program file it is expanded to:

printf("Hello," "john" "\n");
```

#### <mark style="color:purple;">## →</mark> Performs token pasting, cats two tokens.

#### <mark style="color:purple;">Example:</mark>

<pre class="language-c"><code class="lang-c">#define concat(x,y) x ## y
...
int xy = 10;
...

<strong>// This will make the compiler turn:
</strong>printf("%d", concat(x,y));

// To: 
printf("%d", xy);
</code></pre>
