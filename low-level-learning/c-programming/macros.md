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

# Macros

<figure><img src="../../.gitbook/assets/image (6).png" alt="" width="563"><figcaption></figcaption></figure>

Should always use capital letters for macro functions and there are no spaces in names and its limited to one line only.

#### <mark style="color:purple;">Example:</mark>

```c
#define PI 3.14

#define PRNT(a, b))

printf("value1 = %d\n",a); 
printf("value2 = %d\n", b);

int main() {
int x = 2;
int y = 3;
PRNT(x, y);
return 0;
}
```

## <mark style="color:red;">Macros vs functions</mark>

• All macros are preprocessed which means all of them would be processed before your program compiles.&#x20;

Functions are not preprocessed, they are compiled.

• A macro is always faster than a function, functions take longer than inline code (macros for example).

• For one time use a macro is not really a big deal but a macro inside a nested loop is a much better candidate for speed improvements.&#x20;

We can use profilers to determine where a program spends the most time.

• When calling a function it has to locate some data (a newly allocated stack frame).&#x20;

* Macros don't have this overhead. Macros insert directly into the program (textual program) so if we use the same macro 20 times we get 20 lines of code added into our program.
* Functions are preferred over macros when writing large chunks of code.

• With macros you don't have to worry about variable types.

• Functions give us type checking. If a function expects a string but you give it an int, you will get an error.

• Debugging a macro is much harder than debugging a function. A function can be stopped through by the debugger but a macro cannot.

### <mark style="color:yellow;">Alternatives</mark>

Inline functions are the best alternative to macros.&#x20;

When we add the inline keyword in front of the function it hints the compiler to embed the function body inside the caller (just like a macro).

Inline functions can be debugged and also have type checks.

However the inline keyword is just a hint and not a strict rule.

## <mark style="color:red;">Creating macros</mark>

#### <mark style="color:purple;">There are 2 ways of defining macros:</mark>

#### 1. Symbolic constants (constants represented as symbols).

#### 2. Function macros (operations defined as symbols).

### <mark style="color:yellow;">Symbolic constants</mark>

#### <mark style="color:purple;">Example:</mark>

```c
#define NONFMAC some text here
```

#### <mark style="color:purple;">Defines a macro and some replacement text. after definition the macro can be used as follows:</mark>

```c
NOMAC
/* some text here */
```

Leading or trailing white space around the replacement text is discarded.

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>
#define NOMAC 12

int main(){
    printf("%d\n",NOMAC);
    return 0;
}
```

### <mark style="color:yellow;">Function macros</mark>

#### <mark style="color:purple;">Example:</mark>

```c
#define FMAC(a,b) a here, then b

#define macro_name(list_of_identifiers)substitution_string
```

Defines a macro and some replacement text.&#x20;

The list of identifiers separated by commas appears between parentheses following the macro\_name(FMAC).&#x20;

Each identifier can apear one or more times in the substitution string.

#### <mark style="color:purple;">Using:</mark>

```
FMAC(first text, some text)
```

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>

#define Warning(...) fprintf(stderr, __VA_ARGS__)

int main() {
    Warning("%s: this program is here \n","json");
    return 0;
}
```

#### <mark style="color:purple;">Output:</mark>

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:red;">Macros with arguments</mark>

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>
#define PI 3.14
#define CIRCLE_AREA(x) ((PI) * (x) * (x))

int main(){

    int c = 5;
    int area = CIRCLE_AREA(c + 2);
    
    area = PI * c + 2 * c + 2;
    
    printf ("area is %d\n",area);
    
    return 0;
}
```

## <mark style="color:red;">Standard C pre-defined macros</mark>

{% code overflow="wrap" %}
```c
__FILE__  → represent the current file name (string)

__LINE__ → represents the current line number of the current source code (an integer constant)

__func__ → the name of any function when placed inside a function of the current file ( not part of the standard) 

__DATE__ → the date the source file was compiled ( a string of the format "Mmm dd yyyy" such as "jan 19 2002")

__TIME__ → the time the source file was compiled ( a string literal of the format "hh:mm:ss")

__STDC__ → used to indicate if the compiler supports standard C by returning value 1

__STDC_VERSION__ → expands to the C Standard’s version number, a long integer constant of the form yyyymmL where yyyy and mm are the year and month of the Standard version.The value 199409L signifies the 1989 C standard as amended in 1994

__cplusplus → is defined when the C++ compiler is in use.
```
{% endcode %}
